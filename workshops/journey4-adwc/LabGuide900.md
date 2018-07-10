![](./images/900/title900-2.jpg)

# Lab 900: Create Data Visualizations from ADWC

## Introduction

This lab will walk you through the steps to connect Oracle Data Visualization Desktop to an instance of ADWC and create data visualizations.  Unlimited Data Visualization Desktop licenses are included when connecting to an ADWC data source.  Instructions will be provided to connect your previously created ADWC instance (using sample data loaded into data warehouse) to Oracle Data Visualization Desktop.  We will demonstrate how you can immediately gain insights and create beautiful data visualizations.

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

- Type __‘CMD’__.  At the DOS prompt, navigate to the location where you extracted the files (cwallet.sso and tnsnames.ora).  I suggest that you cut the file location from an explorer window to the desktop.  It will save you typing in future steps.  __NOTE:__ The directions from here will show the steps as if you stored the wallet in the location c:\wallet

- In the DOS window, type the command __‘cd c:\wallet’__ and hit return to execute.  As a shortcut, type __‘cd ‘__ and then do a right click to paste the text on the clipboard from the previous step.  

- We will now create the necessary directory location and copy the file cwallet.sso to the proper location for DV Desktop (__%HOMEPATH%\AppData\Local\DVDesktop\components\OBIS\DWCS__) to find.  Execute the commands (one by one).  If the directory has already been created, then skip the MKDIR command below and just COPY the wallet file.   

   ```
   MKDIR C:\%HOMEPATH%\AppData\Local\DVDesktop\components\OBIS\DWCS\
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
   
- Once the __SALES_HISTORY__ Data Set has successfully been created, click on the main menu item.

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
 
## Create a Another Project with Multiple Canvases in Oracle Data Visualization Desktop

### Create a New Data Visualization project

In this part of the lab, we will create 3 basic interactive canvases within a single project.  You will see how easy and powerful you can gain insights and visualize your data in just a few clicks.  This lab is not intended to be an exhaustive view of all Oracle Data Visualization capabilities.  Much in fact has not been included to keep the lab short.

While this will provide specific instructions to replicate the intended visualizations, don’t feel that you must be constrained.  Try other visualization types and layouts.  Modify filters and colors as you like.  If you run into trouble you can always undo the previous step with Ctrl-Z or the back arrow at the top right of the screen.

   ![](./images/900/imageE002.png)
   
- Create a new Data Visualization project

   You will create a basic interactive dashboard with 3 canvases that summarizes:
    - __Sales Summary__
    - __Product Summary__
    - __Customer Demographics Summary__

- Select the __“Data”__ menu option on the right to reveal the available Data Sets.  Your newly created __SALES_HISTORY__ should be visible with a red icon indicating a live ADWC connection.

- Right click on the __SALES_HISTORY__ data set and choose __“Create Project”__.
   
   ![](./images/900/imageE005.png)

- Click on the Hamburger to hide the navigator and increase your working screen view.

   ![](./images/900/imageE008.png)
   
- This is your new Data Visualization project (blank canvas). 

   ![](./images/900/imageE010.png)

### Canvas 1 - Sales Summary

Questions Answered with Data in this section:

```
‘What are my monthly sales by dollar and quantity sold?’
‘What are my sales in each country region? And what products sell best in each country?’
```

#### Create Sales Summary using a Combo Chart and Trend Line

We will create Sales Summary using a Combo Chart to show the Amount Sold, Quantity Sold by Month.  This will help answer the question, *‘What are my monthly sales by dollar and quantity sold?’*

- Expand the __TIME_ID__ attribute to expose the component time dimension elements.  Create the first visualization by holding down __CTRL__ and multi-selecting __AMOUNT_SOLD__, __QUANTITY_SOLD__ and __MONTH__.  Drag those 3 fields from the left navigation pane to the blank canvas.  A scatterplot is created automatically as you added 2 numeric fields and 1 dimension.  From the __chart__ icon at the top right change the chart type to a __Combo__ chart.

   ![](./images/900/imageE014.png)

- Right click on the metric __QUANTITY_SOLD__ and choose __Y2 Axis__.  Right click the metric __QUANTITY_SOLD__ again and this time choose __Bar__ type display.

   ![](./images/900/imageE019.png)
   
   ![](./images/900/imageE021.png)

- Select the __Advanced Analytics__ option from the left icon.  Then double click on __Trend Line__.

   ![](./images/900/imageE025.png)
   
#### Create Sales Summary using an Area Chart

We will create a Sales Summary using a Trend Line to show the Amount Sold by Product Category and Country Region.  This will help answer the questions such as, *‘What are my sales in each country region? And what products sell best in each country?’*

- Return to the __Data Elements__ view in the Menu Navigator. 

   ![](./images/900/imageE028.png)
   
- Drag __AMOUNT_SOLD__, __COUNTRY_REGION__ and __PROD_CATEGORY__ from the navigator (using __CTRL__ for multi-select) onto the canvas at the bottom of the existing visual.  When the blue bar appears, drop the elements to create the next visualization.

   ![](./images/900/imageE031.png)

   ![](./images/900/imageE033.png)
   
- Change the chart type to an __Area__ chart. 

   ![](./images/900/imageE036.png)
   
- Switch the two attributes locations.  Drag __COUNTRY_REGION__ over __PRODUCT_CATEGORY__ until the green (switch) icons appear over the top right corner of the 2 elements.  Then drop the element to swap their locations.

   ![](./images/900/imageE039.png)
   
- Rename the current canvas by click on the down triangle on the canvas name and choosing __rename__.  Change the name to __Sales Summary__.  

- Save your project as __SALES_HISTORY__ 

   ![](./images/900/imageE042.png)
   
Great job!!  You have created the first of the 3 views for this project.  This is the __Sales Summary__ view.  Next you will create the Product Summary view.

### Canvas 2 - Product Summary 

We will create the __Product Summary__ using a Scatter Chart to show the Quantity Sold, Product List Price by Month, Product Category, Amount Sold and Country Region. 

Questions Answered with Data:

```
‘What products are selling the best?’
‘What products categories are trending?  Are there trends based upon price?  Or by regions?’
```

#### Create Product Summary using a Sunburst Chart

We will create the Product Summary using a Sunburst Chart to show the Quantity Sold by Product Category and Product Sub-Category.  This will help answer the question, *‘What products are selling the best?’*

- Rename the current canvas by click on the down triangle on the canvas name and choosing __‘Rename’__.  Change the canvas name to __‘Sales Summary’__.  

- Save your project as __'SALES_HISTORY'__.

- Add a new blank canvas using the __+__ icon at the bottom of the screen beside the current __'Sales Summary'__ canvas name.  

- On the new blank canvas, choose the __Visualizations__ menu option from the left-hand navigator pane.  Then drag the __Sunburst__ chart onto the canvas.

   ![](./images/900/imageE046.png)

- Switch back to the __Data Elements__ view.  

   ![](./images/900/imageE049.png)

- Add the following attributes and metrics:
    - __QUANTITY_SOLD__ to the __Values (Slice size)__.  
    - __PROD_CATEOGORY__ to the __Category (slices)__.
    - __PROD_SUBCATEGORY__ to the __Color__. 

   ![](./images/900/imageE053.png)
   

#### Create Product Summary using a Scatter Chart

- Follow the same steps from above to a 2nd visualization on this canvas.  This will help answer, *‘What products categories are trending?  Are there trends based upon price?  Or by regions?’*

- Use the navigator to switch to the chart types view.  Drag the __Scatter__ chart option onto the canvas to the right of the sunburst chart you created earlier.  

- Switch back to the __data elements__ view and drag the following items into the new scatter chart.

   Add the following attributes and metrics:  
    - __PROD_LIST_PRICE__ to __Values (Y-Axis)__
    - __QUANTITY_SOLD__ to __Values (X-Axis)__
    - __TIME_ID (Month of Year)__ to __Category (Points)__
    - __PROD_CATEGORY__ to __Color__
    - __AMOUNT_SOLD__ to __Size__
    - __COUNTRY_REGION__ to __Shape__

   ![](./images/900/imageE057.png)
   
- Rename this canvas to __Product Summary__.

You have created 2 of the 3 views for this project.  We will finish this exercise by adding a Demographics Summary view.
   
### Canvas 3 - Demographics Summary 

Questions Answered with Data:

```
‘Of all my customers, which customer income level segment drives my sales?’
‘Do Male or Females buy more from us? And what are the ages of our customers who purchase the most?’
‘What products and product subcategories sell the best to our male or female customers?’
```

#### Create Demographics Summary using a Tree Map

We will create the __Demographics Summary__ using 3 different charts.  We will start by creating a Tree Map to show the Amount Sold segmented by Customer Income Level.  This will help answer the question such as, *‘Of all my customers, which customer income level segment drives my sales?’*

- Create a new blank canvas.  This is the 3rd canvas.  Rename the canvas to __'Demographics'__.

- Hit the __CTRL__ key and multi-select __AMOUNT_SOLD__ and __CUST_INCOME_LEVEL__ in the Data Elements section and drag them onto the canvas.

   ![](./images/900/imageE059.png)
   
- Change the chart type to a __Tree Map__.  Then drag __CUST_INCOME_LEVEL__ to the Color

   ![](./images/900/imageE063.png)
   
#### Create Demographics Summary using a Horizontal Stacked Bar Chart

We will create another chart on our Demographics Summary.  We will create a chart to show the Amount Sold based upon our Customers Year of Birth and their Gender.  This will help answer the question such as, *‘Do Male or Females buy more from us? And what are the ages of our customers who purchase the most?’*

- Multi-select __AMOUNT_SOLD__ and __CUST_YEAR_OF_BIRTH__ and drag them onto the canvas below the tree map to create a new visual. 

- Change the chart type to __Horizontal Stacked__. 

- Add __CUST_GENDER__ to the __Trellis Rows__

   ![](./images/900/imageE066.png)
   
- Resize the chart to make the horizontal bar take up more room.  Hover your mouse between the 2 charts until it turns into a double headed arrow.  Then drag the chart up to resize it.

#### Create Demographics Summary using a Scatter Chart

We will create our last chart on our Demographics Summary.  We will create a chart to show the Amount Sold and Amount Sold for Product Category and Product Sub Category based upon our Customers Gender.  This will help answer the question such as, *‘What products and product subcategories sell the best to our male or female customers?’*

- Create the final chart.  Add a __Scatter__ chart to the right of the horizontal bar chart and below the tree map.  Watch the blue insertion line change from inserting to the right of both charts versus to the right of the bar chart **_and_** below the tree map.

- Add the following attributes and metrics:  

    - __AMOUNT_SOLD__ to __Values (Y-Axis)__
    - __QUANTITY_SOLD__ to __Values (X-Axis)__
    - __PROD_CATEGORY__ to __Category (Points)__
    - __PROD_SUBCATEGORY__ to __Color__
    - __AMOUNT_SOLD__ to __Size__
    - __CUST_GENDER__ to __Trellis Rows__

   ![](./images/900/imageE069.png)
   
 - Resize the scatter chart to be larger.  Hover the mouse between the scatter chart and the horizontal bar until the double headed mouse appears.  Drag the mouse left to increase the scatter plots size.

### Switching to presentation mode - Narrate Your Data, Insights and Analytics

The narrate capability is intended to provide a live presentation mode that an analyst can use to explain their findings during meetings.  Instead of screen grabbing or exporting charts and pasting them statically into a DOC or PPT, this presentation mode provides a versatile graphical mode for stepping through your storyline.  Your storyline, or narrative, is the thinking that led to your finding – your workings that justify your conclusion.  During a meeting, in presentation mode, all navigation and build interfaces are hidden to maximize the view of the visualizations.  All visualizations remain active and are drillable or you can still apply filters, just in case anyone asks questions during the meeting.  You’re not trapped in static PPT.

- Click on the __Narrate__ button at the top right.  This switches the modes from creating visualizations to presenting them.

   ![](./images/900/imageE072.png)
   
- Drag your 3 new canvases into the storyboard (at the bottom).

   ![](./images/900/imageE076.png)
   
- Switch the order of your story so that you present Sales Summary, then Demographics and finally Product Summary.

- Click on the __Present__ button.

   ![](./images/900/imageE077.png)

- Use the navigation dots at the center bottom of the screen to step through your presentation and explain your findings.

   ![](./images/900/imageE079.png)
   ![](./images/900/imageE081.png)
   ![](./images/900/imageE083.png)   

### Exporting your DVA (project) file - Optional Step

This step enables you to share your project file with colleges.

- Exit the Narrate mode by clicking the __X__ at the top right.

- Click on the __Home__ icon which is at the top right of the screen.

- Your most recent project (DVA) will appear at the top left of your home page tiles, under __What’s New__.  Once you mouse over your project the properties icon will appear at the top right of the project tile. Click the properties and then choose __Share__.

   ![](./images/900/imageE086.png)  
   
- Choose to export as a __File__.  Email may work if DV Desktop is able to interact with your local email client.  Cloud provides the mechanism to upload and share your project to Oracle Autonomous Analytics Cloud.

- Choose the export options as shown below and save the DVA file to your desktop. 
 
   ![](./images/900/imageE088.png)  
   
__Great job!!  You have completed the Lab.  Now that you have connected the Oracle Data Visualization Desktop to your Autonomous Data Warehouse, you can find more insights to unleash the power of your data. __ 
