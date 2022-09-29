# Enable Database Vault and verify the HR application

## Introduction

In this lab, we will enable Database Vault on our ATP instance and reboot the ATP instance to reflect that change. Then, we will take another look at the Glassfish application and verify everything is still working accoridngly.


### Objectives

In this lab, you will complete the following tasks:

- Enable Database Vault on the ATP instance **(reboot required)**.
- Verify that the HR application still functions.
  
### Prerequisites

This lab assumes you have:
- An Oracle Always Free/Free Tier, Paid or LiveLabs Cloud Account

## Task 1: Enable Database Vault on the ATP instance

As ADMIN in DB Actions

-- Create DV owner
CREATE USER sec_admin_owen IDENTIFIED BY WElcome_123#;
GRANT CREATE SESSION TO sec_admin_owen;
GRANT SELECT ANY DICTIONARY TO sec_admin_owen;
GRANT AUDIT_ADMIN to sec_admin_owen;

-- Create DV account manager
CREATE USER accts_admin_ace IDENTIFIED BY WElcome_123#;
GRANT CREATE SESSION TO accts_admin_ace;
GRANT AUDIT_ADMIN to accts_admin_ace;

-- Enable SQL Worksheet for the users just created
BEGIN
   ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sec_admin_owen'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sec_admin_owen'), p_auto_rest_auth => TRUE);
   ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('accts_admin_ace'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('accts_admin_ace'), p_auto_rest_auth => TRUE);
END;
/

EXEC DBMS_CLOUD_MACADM.CONFIGURE_DATABASE_VAULT('sec_admin_owen', 'accts_admin_ace');

EXEC DBMS_CLOUD_MACADM.ENABLE_DATABASE_VAULT;

SELECT * FROM DBA_DV_STATUS;

Restart the ATP instance

SELECT * FROM DBA_DV_STATUS;

## Task 2: Verify that the HR application still functions

Use a web browser and navigate to the Glassfish App page to verify it functions without issue. 


You may now **proceed to the next lab.**

## Acknowledgements

- **Author**- Ethan Shmargad, North America Specialists Hub
- **Creator**- Richard Evans, Senior Principle Product Manager
- **Last Updated By/Date** - Ethan Shmargad, September 2022
