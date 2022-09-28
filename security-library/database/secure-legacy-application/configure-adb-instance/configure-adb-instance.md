# Configure the Autonomous Database instance

## Introduction

In this lab, we will explore the Oracle Cloud Platform (OCI) and configure the Autonomous Transaction Processing database instance (ATP).

For more information on the Autonomous Transaction Processing database click [here](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/).

### Objectives

In this lab, you will complete the following tasks:

- Create an ATP database instance.
- Create the `EMPLOYEESEARCH_PROD` schema using **Database Actions**.
- Save the **non-mTLS** connection string in the Glassfish application server's properties file.

### Prerequisites

This lab assumes you have:
- An Oracle Always Free/Free Tier, Paid or LiveLabs Cloud Account

## Task 1: Create an ATP database instance.

## Task 2: Save the non-mTLS connection string in the Glassfish application server's properties file.

./save_connection_string.sh

./test_connection_string.sh

## Task 3: Create the EMPLOYEESEARCH_PROD schema using SQL*Plus from the Glassfish App Server.

./load_app_data.sh

./update_app_connection_string.sh 

./startGlassfish.sh


You may now **proceed to the next lab.**

## Acknowledgements

- **Author**- Ethan Shmargad, North America Specialists Hub
- **Creator**- Richard Evans, Senior Principle Product Manager
- **Last Updated By/Date** - Ethan Shmargad, September 2022

