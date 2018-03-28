![](images/400/TITLE400.png)

Updated: March 18, 2018

# ADWC Lab 400: Query External Data 

## Introduction

In this lab, you will be querying files on the Oracle Cloud Infrastructure Object Storage (OCI) directly without loading them to your database.

**Note:** Make sure you have completed <a href="https://millerhoo.github.io/journey4-adwc/workshops/journey4-adwc/LabGuide300.md" target="_blank">Lab 300: Loading Data</a> before you take this lab. Because you will use the data files on the OCI Object Storage and the credential object from Lab 300 in this lab.

To **log issues**, click [here](https://github.com/millerhoo/journey4-adwc/issues/new) to go to the github oracle repository issue submission form.

## Objectives

-   Learn how to create external tables on top of files residing on the object store

-   Learn how to query external data by the external tables

## Required Artifacts

-   The following lab requires an Oracle Public Cloud account. You may your own cloud account, a cloud account that you obtained through a trial, or a training account whose details were given to you by an Oracle instructor.

-   Oracle SQL Developer (see <a href="http://www.oracle.com/technetwork/developer-tools/sql-developer/overview/index.html" target="_blank">Oracle Technology Network download site</a>).
    We recommend that you download version 17.4 or later, because this version contains enhancements for key Autonomous DW Cloud features. SQL Developer 17.3.1 will also work with Autonomous DW Cloud; versions earlier than 17.3.1 will not.
    **Note**:
    If you are a Windows user on 64-bit platform, download the 'Windows 64-bit with JDK 8 included' distribution as it includes both Java 8 and the Java Cryptography Extension (JCE) files necessary to run SQL Developer and connect to your Autonomous DW Cloud.
    If you are a non-Windows user, download and install the appropriate <a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html" target="_blank">Java 8 JDK</a> for your Operating System. Download and extract the <a href="http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html" target="_blank">Java Cryptography Encryption Archive</a> to the directory as indicated in the README.txt.

# Querying External Data
## Steps
### STEP 1: Create External Tables

-   Connected as your user in SQL Developer, copy and paste <a href="./scripts/400/create_external_tables.txt" target="_blank">this code snippet</a> to SQL Developer worksheet.  

    Use the **create\_external\_table** procedure of the **DBMS\_CLOUD** package to create two external tables on the files (**sale1v3.dat** and **cust1v3.dat**) staged in your object store. Note that you are still using the same credential and the URLs of flies on OCI Object Storage you used when loading data in the previous lab.

    -   For the **file\_uri\_list** parameter, it is assigned the value of the **sale1\_v3\_dat\_URL** variable. At the top of the script, specify the Object Store base URL in the definition of the **base\_URL** variable.
    ![](./images/400/snap0014527.jpg)



-   **Run the script**.

    Now you have external tables for the sample data pointing to files in the object store. Any query against the external tables will return the same result as against the original tables.

### STEP 1: Querying External Data

-   Copy and paste <a href="./scripts/400/query_external_data.txt" target="_blank">this code snippet</a> to SQL Developer worksheet. We only replaced the original table names <TABLE_NAME> with <TABLE_NAME_EXT> in the sample query.  
    ![](images/400/Picture400-4.png)

-   **Run the script**. It will return the same result as against the original tables.

-   You are now ready to move to the next lab.
