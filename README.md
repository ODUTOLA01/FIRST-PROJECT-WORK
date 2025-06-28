# FIRST-PROJECT-WORK
 I started creating my portofio building during my training as a Data Analyst with the Incubator Hub to demostrate core skills in data cleaning,trend analysis,and dashboarding.

I have learnt quite a number of things ranging from Ms Excel to SQL,and Power BI and now to my Portfolio Building

## Project Topic: Kultra Mega Store(KMS) Analysis

### Project Overview

Kultra Mega Stores (KMS) is a Lagos-based retailer of office supplies and furniture with a large corporate operations across Lagos,Nigeria. This case study focuses on analyzing order data from the Abuja division from (2009-2012). By analysing and identifying sales trends by category, segment, and region,uncover high-value and low-performing customers, assess shipping cost efficiency relative to order priority, and to provide actionable insights to help management increase revenue and customer satisfaction based on the data gotten to enable me to tell compelling stories around the data from the insights gotten.

### Data Sources
The primary data source used for this analysis is the KMS_Case_Study.csv and the Order_status.csv file containing detailed information about the orders made  from 2009 to 2012

### Tools
- Ms Excel ( For Data Collection and Cleaning)
   -  [Download Here](https://microsoft.com)
- SQL server (For Querying and Analyis)
  -  [Download Here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

 ### Data Cleaning and Preparation

 In the initial phase of the Data cleaning and preparation,we perform the following action;
 1. Data loading and Inspection,
 2. Handling missing variables,
 3. Data cleaning and formating.

### Exploratory Data Analysis

EDA involves the exploring of the Data to answer some questions about the Data such as;
1. Which product category had the highest sales?  
2. What are the Top 3 and Bottom 3 regions in terms of sales?  
3. What were the total sales of appliances in Ontario?  
4. Advise thecmanagement of KMS on what to do to increase the revenue from the bottom 10 customers?  
5. KMS incurred the most shipping cost using which shipping method?
6. Who are the most valuable customers and what products or services do they typically purchase?  
7. Which small business customer had the highest sales?  
8. Which corporate customer placed the most number of orders in 2009–2012?  
9. Which consumer customer was the most profitable one?  
10. Which customers returned items and what segment do they belong to?  
11. Was shipping cost appropriately spent based on order priority?

### Data Analysis

This is where we include some basic lines of codes/querry during analysis

```SQL
WITH TopCustomers AS (
    Select Top 5 Customer_Name, sum(Sales) 
AS TotalSales
   FROM dbo.[KMS Sql Case Study]
   GROUP BY Customer_Name
   ORDER BY TotalSales Desc
)
SELECT *FROM TopCustomers;

WITH TopCustomers AS (
    Select Top 5 Customer_Name
    FROM dbo.[KMS Sql Case Study]
   GROUP BY Customer_Name
   ORDER BY SUm(Sales) Desc
)
SELECT o.Customer_Name, o.Product_Category,
sum(o.Sales) AS CategorySales
FROM dbo.[KMS Sql Case Study] o
JOIN TopCustomers tc ON o.Customer_Name =
tc.Customer_Name
GROUP BY o.Customer_Name, o.Product_Category
ORDER BY o.Customer_Name, CategorySales
Desc;
```

 ### Results
 
 The analysis results are summarized as following;
 1. Product category 'TECHNOLOGY' as the highest sales.
 2. The regions with the Top 3 sales are;
    |Region|Total Sales|
    |------|-----------|
    |West  |3,597,549.329|
    |Otario|3,063,212.527|
    Prane|2,837,304.650|

    The regions with Bottom 3 are;
    |Region|Total Sales|
    |------|-----------|
    |Nunavut|116,376.486|
    |Northwest Territories|800,847.341|
    |Yukon|975,867.382|
3. The total sales of 'Appliances' in Ontario is 202,346.84
4. The following are the advice for increasing the revenue from bottom 10 customers
 -
    
