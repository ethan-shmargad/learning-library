# Explore the Glassfish HR application functions with Database Vault enabled 

## Introduction

In this lab, we will now flip our Database Vault Realm from **simulation** to **enforcement** mode. This will commit our realm into production. Then, we will put our realm to the test and demonstrate the inability to query the application data under a particular user.


### Objectives

In this lab, you will complete the following tasks:

- Flip the Database Vault realm from **simulation** to **enforcement** mode
- Explore the application and demonstrate the inability to query application data through Database Actions as ADMIN or DBA_DEBRA

### Prerequisites

This lab assumes you have:
- An Oracle Always Free/Free Tier, Paid or LiveLabs Cloud Account

## Task 1: Flip the Database Vault realm from simulation to enforcement mode

		   BEGIN
			  DVSYS.DBMS_MACADM.UPDATE_REALM(
				  realm_name => 'PROTECT_MYHRAPP'
				  ,description => 'A mandatory realm, in simulation mode, to show how EMPLOYEESEARCH_PROD would be protected'
				  ,enabled => DBMS_MACUTL.G_YES
				  ,audit_options => DBMS_MACUTL.G_REALM_AUDIT_FAIL
				  ,realm_type => 1); 
		   END;
		   /

## Task 2: Explore the application and demonstrate the inability to query application data through Database Actions

select userid, firstname, lastname, emptype, position, ssn, sin, nino
  from employeesearch_prod.demo_hr_employees
 where rownum < 10;

You may now **proceed to the next lab.**

## Acknowledgements

- **Author**- Ethan Shmargad, North America Specialists Hub
- **Creator**- Richard Evans, Senior Principle Product Manager
- **Last Updated By/Date** - Ethan Shmargad, September 2022
