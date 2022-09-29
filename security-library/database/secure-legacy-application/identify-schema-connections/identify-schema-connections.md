# Identify the connections to the EMPLOYEESEARCH_PROD schema

## Introduction

In this lab, we will explore creating a realm in our ATP instance. We will keep our realm in **simulation mode**. In turn, this will enable us to capture a record of errors during the development phase of a realm or command rule. Using simulation mode, we will then identify our connections to the `EMPLOYEESEARCH_PROD` schema.

### Objectives

In this lab, you will complete the following tasks:

- Create a Database Vault realm.
- Use **simulation mode** to identify the connections to the `EMPLOYEESEARCH_PROD` schema (app versus non-app connections).

### Prerequisites

This lab assumes you have:
- An Oracle Always Free/Free Tier, Paid or LiveLabs Cloud Account

## Task 1: Create a Database Vault realm

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

Connect to the EMPLOYEESEARCH_PROD schema using SQL*Plus or DB Actions. 

As sec_admin_owen

select * from dba_dv_simulation_log

Repeat queries as DBA_DEBRA

As sec_admin_owen: 

select * from dba_dv_simulation_log




You may now **proceed to the next lab.**

## Learn more
- [Configuring Database Vault Realms](https://docs.oracle.com/database/121/DVADM/cfrealms.htm#DVADM003)
- [Oracle Databse Vault Simulation Mode](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dvadmusing-training-mode-to-log-realm-and-command-rule-activities.html)

## Acknowledgements

- **Author**- Ethan Shmargad, North America Specialists Hub
- **Creator**- Richard Evans, Senior Principle Product Manager
- **Last Updated By/Date** - Ethan Shmargad, September 2022
