Draft Version. Updated: March 8, 2018

Introduction
============

This lab walks you through the steps to get started using the Oracle
Autonomous Data Warehouse Cloud (ADWC). You will provision a new ADWC
database, connect to the database using Oracle SQL Developer, and create
DW users.

This is an instructor led lab, please follow the instructor before doing
the exercises. Stop and wait for the instructor led discussion after
each section before going to the next section.

This lab shows you only the functional aspects of ADWC. It does not have
exercises to test the performance of ADWC as the lab environment is not
running on Exadata.

Objectives
==========

-   Learn how to provision an ADWC service

-   Learn how to connect to ADWC

-   Learn how to create users in ADWC

Required Artifacts
==================

-   The following lab requires an Oracle Public Cloud account that will
    be supplied by your instructor.

-   Oracle SQL Developer (see [Oracle Technology Network download
    site](http://www.oracle.com/technetwork/developer-tools/sql-developer/overview/index.html)).
    We recommend that you download version 17.4 or later, because this
    version contains enhancements for key Autonomous DW Cloud features.
    SQL Developer 17.3.1 will also work with Autonomous DW Cloud;
    versions earlier than 17.3.1 will not.\
    **Note**:\
    If you are a Windows user on 64-bit platform, download the 'Windows
    64-bit with JDK 8 included' distribution as it includes both Java 8
    and the Java Cryptography Extension (JCE) files necessary to run SQL
    Developer and connect to your Autonomous DW Cloud.\
    If you are a non-Windows user, download and install the
    appropriate [Java 8
    JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) for
    your Operating System. Download and extract the [Java Cryptography
    Encryption
    Archive](http://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) to
    the directory as indicated in the README.txt.

Provisioning an ADWC Service
============================

### **STEP 1: Log in to Oracle Autonomous Data Warehouse Cloud**

-   Go to the URL below from your browser.

<https://abcd1234.oracle.com>.

-   Enter &lt;account name&gt; in the **Cloud Account Name** field and
    click **My Services**.

![](./images/100/Picture100-1.png)

-   Log in with the following account:

    -   **Username**: &lt;username&gt;

    -   **Password**: &lt;password&gt;

![](./images/100/Picture100-2.png)

-   Click the **Menu Action** icon and select **Open Service Console**.

![](./images/100/Picture100-3.png)

### **STEP 2: Create an ADWC Instance**

-   On the Instances page, click **Create Instance**.

![](./images/100/Picture100-4.png)

-   In the first page of the Create New Instance wizard, enter the
    following information:

    -   **Database Name** - &lt;yourname&gt;dw. Prefix your database
        name with a unique name (you can use your name, for example),
        e.g. JackDW. Since all users will be using the same cloud
        account, it is important that you use a unique database name.

    -   **CPU Count** – 2. Do not use more than 2 as it may cause other
        users not being able to provision as the training environment
        can only host a small number of CPUs.

    -   **Storage Capacity (TB)** - 1

    -   **Administrator Password** – &lt;password&gt;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click **Next**.

![](./images/100/Picture100-5.png)

-   In the second page of the Create New Instance wizard, confirm your
    entries and click **Create**. The Create New Instance wizard closes.

![](./images/100/Picture100-6.png)

-   On the Instances page, the **Status** field indicates the service
    instance is being created. When creation is completed, the Status
    field disappears. You can click the **Refresh** button to see the
    latest creation status.

![](./images/100/Picture100-7.png)

Connecting to ADWC
==================

Downloading the credentials wallet
----------------------------------

As ADWC only accepts secure connections to the database, you need to
download a wallet file containing your credentials first. The wallet is
downloaded from the ADWC service console.

### **STEP 3: Log in to the Service Console**

-   In the Instances screen find your database and click “Service
    Console” in the actions menu.

![](./images/100/Picture100-8.png)

![](./images/100/Picture100-9.png)

-   Log in to the service console with the following information.

    -   Username: admin

    -   Password: &lt;password&gt;. The administrator password you
        specified during provisioning

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The service console opens in the Overview mode.

![](./images/100/Picture100-10.png)

### **STEP 4: Download the credentials wallet**

-   Click the **Administration** tab in the service console.

![](./images/100/Picture100-11.png)

-   Click **Download Client Credentials**.

![](./images/100/Picture100-12.png)

-   You are prompted to create a password for the credentials zip file.
    Enter a password **&lt;password&gt;** and click **Download**.

![](./images/100/Picture100-13.png)

Store the zip file and make note of the password. You will use the zip
file and password in the next step to define a SQL Developer connection
to your Autonomous DW Cloud database.

Connecting to the database using SQL Developer
----------------------------------------------

Start Oracle SQL Developer and create a connection for your database
using the default administrator account, ADMIN, by following these
steps.

### **STEP 5: Connect to the database using SQL Developer**

-   Open SQL Developer on your local computer. Click the **Create
    Connection** icon in the Connections toolbox on the top left of the
    SQL Developer homepage.

![](./images/100/Picture100-14.png)

-   Fill in the connection details as below:

    -   **Connection Name**:** **admin

    -   **Username**: admin

    -   **Password**:** **&lt;password&gt;. The password you specified
        during provisioning

    -   **Connection Type**: Cloud PDB

    -   **Configuration File**: Click the **Browse** button to point to
        the location of the file.

    -   **Keystore Password**: &lt;password&gt;. The password you
        specified when downloading the credentials wallet from the ADWC
        service console

    -   **Service**: There are 3 pre-configured database services for
        each database. Pick &lt;databasename&gt;\_high for this lab. For
        example, if you created a database named JackDW select
        jackdw\_high as the service. As all lab attendees are using the
        same PSM tenant you will see all services for all users in this
        list, pick the one belonging to your database.

        **Note**: Earlier versions of SQL Developer may not support this
        feature.

![](./images/100/Picture100-15.png)

-   Click **Test**.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Success status displays at the left-most bottom of the New/Select
Database Connection dialog.

-   Click **Connect**.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;An entry for the new connection appears under Connections.

 
=

Creating users in ADWC
======================

Using your existing connection in SQL Developer, create a new user named
SH.

### **STEP 6: Create a user in your ADWC database** 

-   Open a SQL Developer worksheet and run the following SQL statements
    to create the user adwc\_user, swapping in a password. ADWC uses the
    password complexity function ora12c\_verify\_function, so you need
    to use a strong password for your users.
`````````````
create user sh identified by "<password>";

grant dwrole to sh;
`````````````````
![](./images/100/Picture100-16.png)

Note that the database role DWROLE includes the privileges required by a
typical DW developer. You can grant additional database privileges if
needed.

You are now ready to move to the next lab.