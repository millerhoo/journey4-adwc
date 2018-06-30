![](./images/900/title900.jpg)

# Lab 900: Creating Visualization from ADWC

## Introduction

This lab will walk you through the steps to connect Oracle Data Visualization Desktop to an instance of ADWC.  Unlimited Data Visualization Desktop licenses are included when connecting to an ADWC data source.  Instructions will be provided to connect your previously created ADWC instance (using sample data loaded into data warehouse) to Oracle Data Visualization Desktop.  We will demonstrate how you can immediately gain insights and create beautiful data visualizations.

At this point, you should have performed the following:

   1. Created an ADWC instance and downloaded the credentials wallet.  

## Objectives
- Learn how to connect a desktop analytics tool to the powerful Autonomous Data Warehouse 
- Learn how to secure a desktop client connection to Autonomous Data Warehouse
- Learn how to create a simple data visualization project with Oracle Data Visualization Desktop
- Learn how to access and gain insights from data in the Autonomous Data Warehouse


## Required Artifacts
- Installation of Oracle Data Visualization Desktop
- Access to an ADWC instance 

## Set Up Local Windows Desktop Environment

### Installing Oracle Data Visualization Desktop on a Windows Desktop

- Downloaded the latest version of Oracle Data Visualization Desktop (DVD) from [here](http://www.oracle.com/technetwork/middleware/oracle-data-visualization/downloads/oracle-data-visualization-desktop-2938957.html).
- After saving the installer .ZIP to your desktop, open the compressed file and click on the .exe installer and follow the guided steps.

   ![](./images/900/image002.png)

   ![](./images/900/image003.png)

   ![](./images/900/image004.png)

   ![](./images/900/image006.png)

### Securing Your Client Connection to Autonomous Data Warehouse

You want to secure your data from the desktop all the way from the client application to the server where your data is stored.  Password credentials for connecting to databases can now be stored in a client-side Oracle wallet, a secure software container used to store authentication and signing credentials.  This wallet usage can simplify large-scale deployments that rely on password credentials for connecting to databases. When this feature is configured, application code, batch jobs, and scripts no longer need embedded user names and passwords. Risk is reduced because such passwords are no longer exposed in the clear, and password management policies are more easily enforced without changing application code whenever user names or passwords change.

- Go to the directory that you saved your credentials wallet file from the previous lab.  Open up the .ZIP file.

   ![](./images/900/image008.png)
   ![](./images/900/image009.png)

- Copy (extract or drag) two files to the wallet directory.

   - cwallet.sso
   - tnsnames.ora

- To secure the client communications between Oracle Data Visualization Desktop, we will copy the wallet to a location that Data Visualization Desktop expects.  In Windows, click on the __Start menu__ and select __Run….__   

   ![](./images/900/image010.png)

- Type __‘CMD’__.  At the DOS prompt, navigate to the location where you extracted the files (cwallet.sso and tnsnames.ora).  I suggest that you cut the file location from an explorer window to the desktop.  It will save you typing in future steps.  The directions from here will show the steps as if you stored the wallet in the location c:\adw-wallet

- In the DOS window, type the command __‘cd c:\wallet’__ and hit return to execute.  As a shortcut, type __‘cd ‘__ and then do a right click to paste the text on the clipboard.  

- We will now copy the file cwallet.sso to the proper location for DV Desktop (__%HOMEPATH%\AppData\Local\DVDesktop\components\OBIS\DWCS__) to find.  Execute the command, 

   ```
   COPY cwallet.sso C:\%HOMEPATH%\AppData\Local\DVDesktop\components\OBIS\DWCS\
   ```

- Go to the directory and ensure that the file was copied over.  At the command prompt, type 

   ```
   CD C:\%HOMEPATH%\AppData\Local\DVDesktop\components\OBIS\DWCS
   ```
   and

   ```
   ‘DIR’
   ```

   ![](./images/900/image011.png)


## Create a View using Tables in the SH Schema

### Execute the Provided Script in SQL Developer

For the sake of this test drive exercises, we are helping make it as simple as possible for you.  We don’t want the required steps of creating a data model required in any reporting, analytics or data visualization tool to overshadow the fact that Autonomous Data Warehouse is simple.  In this exercise, we are using the SH schema provided and will be creating a simple view.

- Go back to SQL Developer as you did in the previous exercises and connect to the __‘admin’__ user.   Cut and paste and execute the following script.  

   ```
   drop view DV_SH_VIEW;

   create or replace view DV_SH_VIEW as select 
   P.PROD_NAME,
   P.PROD_DESC,
   P.PROD_CATEGORY,
   P.PROD_SUBCATEGORY,
   P.PROD_LIST_PRICE,
   S.QUANTITY_SOLD,
   S.AMOUNT_SOLD,
   X.CUST_GENDER,
   X.CUST_YEAR_OF_BIRTH,
   X.CUST_MARITAL_STATUS,
   X.CUST_INCOME_LEVEL,
   R.COUNTRY_NAME,
   R.COUNTRY_SUBREGION,
   R.COUNTRY_REGION,
   T.TIME_ID,
   T.DAY_NAME,
   T.CALENDAR_MONTH_NAME,
   T.CALENDAR_YEAR from
   SH.PRODUCTS P,
   SH.SALES S,
   SH.CUSTOMERS X,
   SH.COUNTRIES R,
   SH.TIMES T where
   S.PROD_ID=P.PROD_ID and
   S.CUST_ID=X.CUST_ID and
   S.TIME_ID=T.TIME_ID and
   X.COUNTRY_ID=R.COUNTRY_ID;
   ```

   ![](./images/900/image012.png)


## Create a Connection to ADWC from Data Visualization Desktop

### Create Connection

- Start Oracle Data Visualization Desktop from the Start Windows Menu

   ![](./images/900/image015.png)
   
   ![](./images/900/image016.png)

- When Oracle Data Visualization Desktop opens, click on the __‘Create’__ button and __‘Connection’__ selection highlighted.

   ![](./images/900/image018.png)

- In the Create Connection Dialog, select the highlighted option for __‘Oracle Autonomous Data Warehouse’__ and start moving through the wizard.

   ![](./images/900/image021.png)

- Go back to the directory where you saved your wallet file and extracted the file, __‘tnsnames.ora’__.  Open the file and search for information that you will use to connect with.  I have provided an entry from the file as an example.

   ```
   kzengdw1_high = (description= (address=(protocol=tcps)(port=1522)(host=adwc.us-e1-1.oraclecloud.com))
   (connect_data=(service_name=tuak88quycc88vq_kzengdw1_high.adwc.oraclecloud.com))
   (security=(ssl_server_cert_dn="CN=adwc.us-e1.oraclecloud.com,OU=Oracle BMCS US,O=Oracle Corporation,
   L=Redwood City,ST=California,C=US"))   )
   ```
 
 
 
     | Connection Info       | Entry                                             |  
     | --------------------- | :--------------------------------------------- |
     | New Connection Name:  | Type in 'SALES_HISTORY'                             |
     | Host:                 | e.g. adwc.us-e1-1.oraclecloud.com (from above) |
     | Port:                 | 1522                                              |
     | Username:             | <your username> Insert username created in previous labs.  Same as SQL Developer credentials. |                                            
     | Password              | <your password> Insert username created in previous labs.  Same as SQL Developer credentials. | 
     | Service Name:         | e.g. tuak89quycc88vqkzengdw1high.adwc.oraclecloud.com (This information is also found in the ‘tnsnames.ora’ file. Example highlighted above.) |

- After completing the fields, click on __‘Save’__ button.

   ![](./images/900/image023.png)

- Upon success of creating a new connection to the Autonomous Data Warehouse, select the __Create__ button and select __Data Set__.  

   ![](./images/900/image025.png)
   
- We will now choose to select the sales data we want to analyze and visualize in our first project.  Select the connection we just created named __SALES_HISTORY__.

   ![](./images/900/image027.png)
   
- Click on the __ADMIN__ schema in the data warehouse.

   ![](./images/900/image029.png)

- Select (Click) on __DB_SH_VIEW__.

   ![](./images/900/image031.png)
   
- First click on the __Add All__ Label in the left column, type a new Name for the Data Set called, __‘SALES_HISTORY’__ and then Click on the __Add__ button.  __NOTE:__  It is important to use the new name of __‘SALES_HISTORY’__ as the rest of the lab exercises will reference that name.  Optionally, you can click on the __'Get Preview'__ to see some example records.

   ![](./images/900/image033.png)
   
- Once the SALES_HISTORY Data Set has successfully been created, click on the main menu item.

   ![](./images/900/image035.png)
   
- Select the __Data__ menu option on the left.  This should reveal your new __SALES_HISTORY__ Data Set you created.  Right click on __SALES_HISTORY__ label and choose the __'Inspect__ sub-menu item.

   ![](./images/900/image037.png)
   
 - We are going to override the data types for two columns recognized as numeric and correctly set them as attributes-- __CALENDAR_YEAR__ and __CUST_YEAR_OF_BIRTH__.  Hover the mouse over the names column looking for the fields, __CALENDAR_YEAR__.  Change the __‘Treat As’__ field as an __‘Attribute’__.  Repeat for the field, __CUST_YEAR_OF_BIRTH__.  When both field have been set to Attribute, click __OK__ when done.  You will be promoted to confirm that you want to change the Data Set.  Confirm, __‘Yes’__.

   ![](./images/900/image039.png)
   
   ![](./images/900/image041.png)  
   
- Once the Data Set has been updated successfully, we are ready to create our first project.  Click on the __Create Project__ button.

   ![](./images/900/image043.png)
   
- Click on the __SALES_HISTORY__ Data (highlight) and click on the __‘Add to Project’__ button.

   ![](./images/900/image045.png)

## Create a New Project in Oracle Data Visualization Desktop

### Project Introduction

No matter what your role is in the organization, access to timely data can provide greater insights to improve the performance of your business.  Whether you’re creating a data warehouse or data mart for yourself or others, Autonomous Data Warehouse Cloud is making it far simpler than ever before.  Easy, fast and elastic.   This small project demonstrates this.  This is how business users would interact with the Autonomous Data Warehouse.

SCENARIO:  For a moment, rewind yourself back a couple of decades.  You work at an electronics reseller company.  The founder started his business by selling camera and photography equipment.  He’s already diversified his business portfolio as he already owns many 1-hour photo processing and video rental stores.  Over the last few years, his computer reselling business has grown, but he’s not convinced that the PC/server business will last.  His instincts tell him to continue to focus on growing his photography equipment and supplies business rather than PCs.  If you had access to this technology and solution, what would this data tell him?  What insights could you share?  How could this data help him focus on the right investments, grow his business and better target his existing and potential customers?

### Browse and Explore the Data With Data Tiles

- We will first start by browsing the data that’s available in our Data Set. Click on the highlighted __Prepare__ button and then click to select the menu option __Data Tiles__.  

   ![](./images/900/image047.png)
   
   Notice how easy it is to browse the data elements to see what is available for you to further explore.  After scrolling through the data, click back on the highlighted __Visualize__ option to bring up the blank canvas.  

   ![](./images/900/image049.png)
   
### Create Your First Data Visualization

- We will now create a very simple visualization project to finish this lab.  Multi-select (ctrl+click) the 5 Data Elements within __SALES_HISTORY__ including __PROD_NAME__, __AMOUNT_SOLD__, __CALENDAR_YEAR__, __PROD_CATEGORY__, and __QUANTITY_SOLD__.  

- Drag the five selected columns to the middle of the screen.
   ![](./images/900/image051.png)
   
- Based upon this data, Oracle Data Visualization Desktop will choose a default visualization.  If not, choose the __Scatter__ chart so it matches the view below.   

   ![](./images/900/image053.png)
   
 At this point with a very few steps, you now have something that can further bring your data to life and begin to make some data-driven decisions.  And of course, as you share this with others, more and more people will want to gain access to the data.  But, don’t forget how easy, fast and elastic the Autonomous Data Warehouse is and will be able to quickly scale to meet your growing data and end users.
 
## Create a Another Project with Multiple Canvas in Oracle Data Visualization Desktop

### Create a new Data Visualization project

### Canvas 1 - Sales Summary

Questions Answered with Data:
- ‘What are my monthly sales by dollar and quantity sold?’
- What are my sales in each country region? And what products sell best in each country?’

### Canvas 2 - Product Summary 

Questions Answered with Data:
- ‘What products are selling the best?’
- ‘What products categories are trending?  Are there trends based upon price?  Or by regions?’

### Canvas 3 - Demographics Summary 

Questions Answered with Data:
- ‘Of all my customers, which customer income level segment drives my sales?’
- ‘Do Male or Females buy more from us? And what are the ages of our customers who purchase the most?’
- ‘What products and product subcategories sell the best to our male or female customers?’

### Switching to presentation mode - Narrate Your Data, Insights and Analytics

The narrate capability is intended to provide a live presentation mode that an analyst can use to explain their findings during meetings.  Instead of screen grabbing or exporting charts and pasting them statically into a DOC or PPT, this presentation mode provides a versatile graphical mode for stepping through your storyline.  Your storyline, or narrative, is the thinking that led to your finding – your workings that justify your conclusion.  During a meeting, in presentation mode, all navigation and build interfaces are hidden to maximize the view of the visualizations.  All visualizations remain active and are drillable or you can still apply filters, just in case anyone asks questions during the meeting.  You’re not trapped in static PPT.

### Exporting your DVA (project) file - Optional Step

This step enables you to share your project file with colleges.

