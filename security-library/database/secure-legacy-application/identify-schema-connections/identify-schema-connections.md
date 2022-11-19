# Identify the database users accessing EMPLOYEESEARCH_PROD data

## Introduction

In this lab, we will explore creating a realm in our ATP instance. We will create a Databse Vault realm in **simulation mode**. This will enable us to capture database users accessing data in the `EMPLOYEESEARCH_PROD` schema. 

### Objectives

In this lab, you will complete the following tasks:

- Create a Database Vault realm.
- Use **simulation mode** to identify the database users accessing data in the `EMPLOYEESEARCH_PROD` schema.
- Authorize the appropriate database users to access the `EMPLOYEESEARCH_PROD` schema.

### Prerequisites

This lab assumes you have:
- An Oracle Always Free/Free Tier, Paid or LiveLabs Cloud Account

## Task 1: Create a Database Vault realm in simulation mode

As sec_admin_owen

		-- Create the "PROTECT_MYHRAPP" DV realm
		   BEGIN
			  DVSYS.DBMS_MACADM.CREATE_REALM(
				  realm_name => 'PROTECT_MYHRAPP'
				  ,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
				  ,enabled => DBMS_MACUTL.G_SIMULATION
				  ,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
				  ,realm_type => 1); 
		   END;
		   /
		-- Show the current DV realm
		SELECT name, description, enabled FROM dba_dv_realm WHERE id# >= 5000 ORDER BY 1;
    
    -- Add the EMPLOYEESEARCH_PROD schema and all objects to this realm
    BEGIN
       DVSYS.DBMS_MACADM.ADD_OBJECT_TO_REALM(
           realm_name   => 'PROTECT_MYHRAPP',
           object_owner => 'EMPLOYEESEARCH_PROD',
           object_name  => '%',
           object_type  => '%');
    END;
    /
    
    Typically, we would then add the EMPLOYEESEARCH_PROD app to be an authorized participant in the realm. We are going to skip that step for now because we want to capture access to objects in the EMPLOYEESEARCH_PROD schema, including access by the schema itself. This will help us understand which objects are accessed and where they are accesed from. For example, from the Glassfish app, DB Actions, or SQL*Plus. 	


## Task 2: Use simulation mode to identify the connections to the EMPLOYEESEARCH_PROD schema

Connect to the App using Glassfish then see the results in DB Actions or SQL*Plus

As sec_admin_owen

select * from dba_dv_simulation_log

Connect to the HR Application

http://<PUBLIC_IP>:8080/myhrapp

- Login as `hradmin` with the password `Oracle123`
- Click Search navigate to the search page
- Before clicking `Search`, check `Yes` in the Debug option to see the query the application peforms. You will use this query in another step
- Click on an employee name to navigate to the supplemental data pages.

Connect to the EMPLOYEESEARCH_PROD schema using SQL*Plus or DB Actions. 

`alter session set current_schema = EMPLOYEESEARCH_PROD;

select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME`


Repeat queries as DBA_DEBRA in DB Actions

`alter session set current_schema = EMPLOYEESEARCH_PROD;

select a.USERID, a.FIRSTNAME, a.LASTNAME, a.EMAIL, a.PHONEMOBILE, a.PHONEFIX, a.PHONEFAX, a.EMPTYPE, a.POSITION, a.ISMANAGER, a.MANAGERID, a.DEPARTMENT, a.CITY, a.STARTDATE, a.ENDDATE, a.ACTIVE, a.COSTCENTER, b.FIRSTNAME as MGR_FIRSTNAME, b.LASTNAME as MGR_LASTNAME, b.USERID as MGR_USERID from DEMO_HR_EMPLOYEES a left outer join DEMO_HR_EMPLOYEES b on a.MANAGERID = b.USERID where 1=1 order by a.LASTNAME, a.FIRSTNAME`

From the Glassfish VM, execute the shell script to login as ADMIN and run a similar query:

`./dv_query_employee_search.sh`


As sec_admin_owen, you will now see several realm violations in the simulation log. You will see who accessed the objects, the time, the source IP address, and the client program/module that was used.  

select * from dba_dv_simulation_log


Now we know we want `EMPLOYEESEARCH_PROD` to be able to access their own objects so we will add this schema to the realm as an authorized owner.

begin
  DVSYS.DBMS_MACADM.ADD_AUTH_TO_REALM(
	realm_name => 'PROTECT_MYHRAPP'
	, grantee => 'EMPLOYEESEARCH_PROD'
	, rule_set_name => null
	, auth_options => DBMS_MACUTL.G_REALM_AUTH_OWNER ); 
end;
/

As sec_admin_owen, We can clear the simulation log to see new results

delete from dvsys.simulation_log$;
commit;

If we re-execute steps <x> and <y> above, we will no longer see `EMPLOYEESEARCH_PROD` queries showing up in the simulation log. This is because we have authorized `EMPLOYEESEARCH_PROD` and it is no longer a realm volation.  You can see that `DBA_DEBRA` and `ADMIN` are still violations of the realm. Since we do not want them accessing the HR data, this is expected behavior and next we can enable enforcement mode for the `PROTECTHR_MYHRAPP` realm. 



You may now **proceed to the next lab.**

## Learn more
- [Configuring Database Vault Realms](https://docs.oracle.com/database/121/DVADM/cfrealms.htm#DVADM003)
- [Oracle Databse Vault Simulation Mode](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dvadmusing-training-mode-to-log-realm-and-command-rule-activities.html)

## Acknowledgements

- **Author**- Ethan Shmargad, North America Specialists Hub
- **Creator**- Richard Evans, Senior Principle Product Manager
- **Last Updated By/Date** - Ethan Shmargad, September 2022
