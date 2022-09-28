# Load and verify the data in the Glassfish application

## Introduction

In this lab, we will populate the Glassfish application with data and then verify that the HR application still functions appropriately. This will involve loading the `EMPLOYEESEARCH_PROD` schema objects to the ATP instance.

<!---
**Download the data lab files:** [Link](https://objectstorage.us-ashburn-1.oraclecloud.com/p/tVAwp-XWRsm1oouSHDzzZwyUQ5TErSPpPNhuYPMTbSJOZlC-Pvsed-caGfHYrkV5/n/orasenatdpltsecitom03/b/Twitter_LL/o/Twitter_LL2.zip)
-->

### Objectives

In this lab, you will complete the following tasks:

-  Load the `EMPLOYEESEARCH_PROD` schema objects and data into the **ATP instance**.
-  Verify the HR app functions using the Glassfish app **public IP**.

### Prerequisites

This lab assumes you have:
- An Oracle Always Free/Free Tier, Paid or LiveLabs Cloud Account

## Task 1: Load the EMPLOYEESEARCH_PROD schema objects and data into the ATP instance

This occurs after you have already copied the Connection String and saved it using ./save_connection_string.sh

To load the data, run: ./load_app_data.sh

## Task 2: Configure the HR app to use the ATP connection string

./update_app_connection_string.sh 



## Task 3: Verify the HR app functions using the Glassfish app public IP

You may now **proceed to the next lab.**

## Acknowledgements

- **Author**- Ethan Shmargad, North America Specialists Hub
- **Contributers**- Richard Evans, Senior Principle Product Manager
- **Last Updated By/Date** - Ethan Shmargad, September 2022
