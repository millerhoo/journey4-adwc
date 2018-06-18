![](images/100/TITLE100.JPG)

Updated: June 15, 2018

# ADWC Lab 100: Provisioning and Getting Started

## Introduction

This lab walks you through the steps to get started using the Oracle Autonomous Data Warehouse Cloud (ADWC) on Oracle Infrastructure Cloud (OCI). You will provision a new ADWC database and connect to the database using Oracle SQL Developer.

<a href="https://raw.githubusercontent.com/millerhoo/journey4-adwc/master/workshops/journey4-adwc/images/ADWC%20HOL%20-%20Provision%20and%20Connect.mp4" target="_blank">**Click here**</a> to watch a video demonstration of provisioning and connecting to a new ADWC database.


To **log issues**, click [here](https://github.com/millerhoo/journey4-adwc/issues/new) to go to the github oracle repository issue submission form.

## Objectives

-   Learn how to provision an ADWC instance

-   Learn how to connect to ADWC

## Required Artifacts

-   The following lab requires an Oracle Public Cloud account. You may use your own cloud account, a cloud account that you obtained through a trial, or a training account whose details were given to you by an Oracle instructor.

-   Oracle SQL Developer 18.1 (see [Oracle Technology Network download site](http://www.oracle.com/technetwork/developer-tools/sql-developer/overview/index.html)).
    Please be sure to use SQL Developer version 18.1 or later because this version contains enhancements for key Autonomous DW Cloud features. 

    * Note *: If you are a Windows user on 64-bit platform, download the 'Windows 64-bit with JDK 8 included' distribution as it includes both Java 8 and the Java Cryptography Extension (JCE) files necessary to run SQL Developer and connect to your Autonomous DW Cloud.
    If you are a non-Windows user, download and install the appropriate [Java 8 JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) for your Operating System. Download and extract the [Java Cryptography Encryption Archive](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) to the directory as indicated in the README.txt.

# Section 1. Provisioning an ADWC Instance

In this section you will be provisioning an ADWC database using the cloud console.

## Steps

### **STEP 1: Sign in to Oracle Cloud**

-   Go to [cloud.oracle.com](https://cloud.oracle.com), click **Sign In** to sign in with your Oracle Cloud account.

![](./images/100/Picture100-2.png)

-   Enter your **Cloud Account Name** and click **My Services**.

![](./images/100/Picture100-3.png)

-   Enter your Cloud **username** and **password**, and click **Sign In**.

![](./images/100/Picture100-4.png)

### **STEP 2: Create an ADWC Instance**

-   Once you are logged in, you are taken to the cloud dashboard where you can see all the services available to you. To access Oracle Cloud Infrastructure (OCI) dashboard, click on **Customize Dashboard**.

![](./images/100/Picture100-36.JPG)

-  Find **Compute** under Infrastructure and click on **Show**

![](./images/100/Picture100-37.JPG)

-  Now you should see Compute service under your dashboard. Go ahead and click on **Compute**

![](./images/100/Picture100-38.JPG)

-  This will bring you to the service details page. Click on **Open Service Console** on top right of your screen to bring you to OCI dashboard.

![](./images/100/Picture100-39.JPG)

![](./images/100/Picture100-19.jpeg)

-  Once you are on OCI dashboard page, click on the hamburger menu icon on the top left of the screen

![](./images/100/Picture100-20.jpeg)

-  Click on **Autonomous Data Warehouse** from the menu

![](./images/100/Picture100-21.jpeg)


-  Click on **Create Autonomous Date Warehouse** button to start the instance creation process

![](./images/100/Picture100-23.jpeg)

-  This will bring up Create Autonomous Data Warehouse screen where you specify the configurations of the instance

![](./images/100/Picture100-24.jpeg)

-  Click on **Click here** to enable compartment selection so that the ADWC instance is in a specific compartment

![](./images/100/Picture100-25.jpeg)

-  Select a compartment of your choice

![](./images/100/Picture100-26.jpeg)

-  Specify a name for the instance

![](./images/100/Picture100-27.jpeg)

-  You can update the storage size if needed

![](./images/100/Picture100-28.jpeg)

-  Then specify the password for the instance

![](./images/100/Picture100-29.jpeg)

-  Make sure you have everything fill out

![](./images/100/Picture100-30.jpeg)

-  Click on **Create Autonomous Data Warehouse** to start provisioning the instance

![](./images/100/Picture100-31.jpeg)

![](./images/100/Picture100-32.jpeg)

-  Once it finishes provisioning, you can click on the instance name to see details of it

![](./images/100/Picture100-33.jpeg)

You now have created your first Autonomous Data Warehouse Cloud instance.

# Section 2. Connecting to ADWC

## Downloading the credentials wallet


As ADWC only accepts secure connections to the database, you need to download a wallet file containing your credentials first. The wallet is downloaded from the ADWC service console.

## Steps

### **STEP 3: Sign in to the Service Console**

-   In the instance details page find your database and click **Service Console** 

![](./images/100/Picture100-34.jpeg)

-   This will open a new browser tab for the service console. Sign in to the service console with the following information.

    -   **Username:** admin

    -   **Password:** The administrator password you specified during provisioning

![](./images/100/Picture100-35.jpeg)

### **STEP 4: Download the credentials wallet**

-   Click the “**Administration**” tab and click “**Download Client Credentials**” to download the wallet.

![](./images/100/Picture100-15.png)

-   Specify a password of your choice for the wallet, you will need this password when connecting to the database later. Click **Download** to download the wallet file to your client machine.

![](./images/100/Picture100-16.png)





## Connecting to the database using SQL Developer

Start SQL Developer and create a connection for your database using the default administrator account, ADMIN, by following these steps.

## Steps

### **STEP 5: Connect to the database using SQL Developer**

-   Click the **Create Connection** icon in the Connections toolbox on the top left of the SQL Developer homepage.

![](./images/100/snap0014653.jpg)

-   Fill in the connection details as below:

    -   **Connection Name:** admin_low

    -   **Username:** admin

    -   **Password:** The password you specified during provisioning

    -   **Connection Type:** Cloud PDB

    -   **Configuration File:** Enter the full path for the wallet file you downloaded before, or click the **Browse button** to point to the location of the file.

    -   **Keystore Password:** The password you specified when downloading the wallet from the ADWC service console

    -   **Service:** There are 3 pre-configured database services for each database. Pick **&lt;databasename&gt;_low** for this lab. For
        example, if you created a database named testdw select testdw_low as the service.

![](./images/100/Picture100-18.png)

-   Test your connection by clicking the **Test** button, if it succeeds save your connection information by clicking **Save**, then connect to your database by clicking the **Connect** button. An entry for the new connection appears under Connections.

-   Create another connection named as **admin_high** using the same information as above, this time pick **&lt;databasename&gt;_high** as the service name, for example, testdw_high.

-   You are now ready to move to the next lab.
