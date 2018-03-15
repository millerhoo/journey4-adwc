![](images/200/TITLE200.JPG)

Draft Version. Updated: March 13, 2018

# ADWC Lab 200: Working with Database Services and Sample Data Sets

## Introduction

In this lab you will **watch** a demo that shows the performance and concurrency impacts of scaling your service online.

Autonomous Data Warehouse Cloud provides three database services that you can choose when connecting to the database. These are named as HIGH, MEDIUM, and LOW services and provide different levels of performance and concurrency. 

The HIGH database service provides the maximum amount of resources for a query, this also means the number of concurrent queries you can run in this service will not be as much as the other services. 

The MEDIUM database service provides multiple compute and IO resources for a query. This service also provides more concurrency compared to the HIGH database service. 

The LOW database service provides the least amount of resources for a query, this also means the number of concurrent queries you can run in this service will  be higher than the other services. 

As a user you need to pick the database service based on your performance and concurrency requirements.

The demo will use the LOW and HIGH database services to understand the performance differences between them. The demo will run queries on sample data sets provided out of the box with ADWC. ADWC provides the Oracle Sales History sample schema and the Star Schema Benchmark (SSB) data set, these data sets are in the SH and SSB schemas respectively. 

You will run queries on the SSB data set which is a 1TB data set with one fact table with around 6 billion rows, and several dimension tables.


*** VIDEO LINK GOES HERE ***

To **log issues**, click [here](https://github.com/millerhoo/journey4-adwc/issues/new) to go to the github oracle repository issue submission form.

