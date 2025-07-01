# KMS-PROJECT-WORK
 I started creating my portofio building during my training as a Data Analyst with the Incubator Hub to demostrate core skills in data cleaning,trend analysis,and dashboarding.

I have learnt quite a number of things ranging from Ms Excel to SQL,and Power BI and now to my Portfolio Building

## Project Topic: Kultra Mega Store(KMS) Analysis

## Table Of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)

### Project Overview

Kultra Mega Stores (KMS) is a Lagos-based retailer of office supplies and furniture with a large corporate operations across Lagos,Nigeria. This case study focuses on analyzing order data from the Abuja division from (2009-2012). By analysing and identifying sales trends by category, segment, and region,uncover high-value and low-performing customers, assess shipping cost efficiency relative to order priority, and to provide actionable insights to help management increase revenue and customer satisfaction based on the data gotten to enable me to tell compelling stories around the data from the insights gotten.

### Data Sources
The primary data source used for this analysis is the KMS_Case_Study.csv and the Order_status.csv file containing detailed information about the orders made  from 2009 to 2012

### Tools
- Ms Excel ( For Data Collection)
   -  [Download Here](https://microsoft.com)
- SQL server (For Querying and Analyis)
  -  [Download Here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)

 ### Data Cleaning and Preparation

 In the initial phase of the Data cleaning and preparation, I perform the following action;
 1. Data loading and Inspection,
 2. Handling missing variables,
 3. Data formating.

### Exploratory Data Analysis

EDA involves the exploring of the Data to answer some questions about the Data such as;
1. Which product category had the highest sales?  
2. What are the Top 3 and Bottom 3 regions in terms of sales?  
3. What were the total sales of appliances in Ontario?  
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers?  
5. KMS incurred the most shipping cost using which shipping method?
6. Who are the most valuable customers and what products or services do they typically purchase?  
7. Which small business customer had the highest sales?  
8. Which corporate customer placed the most number of orders in 2009–2012?  
9. Which consumer customer was the most profitable one?  
10. Which customers returned items and what segment do they belong to?  
11. Was shipping cost appropriately spent based on order priority?

### Data Analysis

This is where I include some basic lines of codes/querry during analysis

```SQL
create database KMS__db

Select *from dbo.[KMS Sql Case Study]

--------QUESTIONS-----

-----Which Product Category had the Highest Sales?----

SELECT TOP 1 Product_Category, 
sum(Sales) As TotalSales
FROM dbo.[KMS Sql Case Study]
GROUP BY Product_Category
ORDER BY TotalSales Desc;

---What are the Top 3 and Bottom 3 regions in terms of Sales?---
-----TOP 3---

Select Top 3 Region, sum(Sales) AS TotalSales
FROM dbo.[KMS Sql Case Study]
GROUP BY Region
ORDER BY TotalSales Desc;
----BOTTOM 3----

Select Top 3 Region, sum(Sales) AS TotalSales
FROM dbo.[KMS Sql Case Study]
GROUP BY Region
ORDER BY TotalSales Asc;

---What Were The Total Sales Of Appliances In Ontario?---

select Region, sum(Sales) AS TotalSales
FROM dbo.[KMS Sql Case Study]
WHERE Product_Sub_Category ='Appliances' AND Province = 'Ontario'
group by Region;

--Advise the KMS on what to do to increase the revenue from the bottom 10 customers

Select Top 10 Customer_Name, sum(Sales) AS TotalSales
FROM dbo.[KMS Sql Case Study]
GROUP BY Customer_Name
ORDER BY TotalSales Asc;
```
```
---KMS incured the most shipping cost using which shipping method?----


Select Top 1 Ship_Mode, sum(Shipping_Cost) AS TotalShippingCost
FROM dbo.[KMS Sql Case Study]
GROUP BY Ship_Mode
ORDER BY TotalShippingCost Desc;

----Who are the most valuable customers,and what products or services do they typically purchase?

GET 5 HIGHEST CUSTOMERS BY SALES

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

----Which small business customer had the highest sales?---

SELECT Top 1 Customer_Name, sum(Sales) AS
TotalSales
FROM dbo.[KMS Sql Case Study]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY TotalSales Desc
```
```
----WHICH CORPORATE CUSTOMER PLACED THE MOST NUMBER OF ORDER IN (2009-2012)?--

SELECT Top 1 Customer_Name,count(Order_ID) AS
Orders
FROM dbo.[KMS Sql Case Study]
WHERE Customer_Segment = 'Corporate'
GROUP BY Customer_Name
ORDER BY Orders Desc


Select *from dbo.[KMS Sql Case Study]


------WHICH CONSUMER CUSTOMER WAS THE MOST PROFITABLE ONE?----

SELECT Top 1 Customer_Name,SUM(Profit) AS
TotalProfit
FROM dbo.[KMS Sql Case Study]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY TotalProfit Desc
```
```
-----WHICH CUSTOMER RETURNED ITEMS,AND WHAT SEGMENT DO THEY BELONG TO?

-----Create VIEW

CREATE VIEW CustomerReturnedSegment AS
SELECT
    k.Order_ID,
    k.Customer_Name,
    k.Customer_Segment,
    s.Status AS return_Status
FROM [dbo].[KMS Sql Case Study] AS k
JOIN [dbo].[Order_Status] AS s
 ON k.Order_ID = s.Order_ID;

SELECT *FROM CustomerReturnedSegment

-----Customer who Returned Items & Their Segment

SELECT
    Customer_Name,
	Customer_Segment,
	return_Status
FROM dbo.[CustomerReturnedSegment]
WHERE
return_Status = 'Returned';


-----Was shipping cost spent appropriately by order priority?

SELECT
     [Order_priority],[Ship_mode],
     COUNT([Order_ID]) AS Order_Count,
     sum(Sales-Profit) AS 
Estimated_ShippingCost,
     AVG(DATEDIFF(DAY,[Order_Date],[Ship_Date])) AS Avg_Ship_Days
 FROM dbo.[KMS Sql Case Study]
Group by [Order_Priority], [Ship_Mode]
Order by  [Order_Priority], [Ship_Mode] Desc;
```

 ### Results
 
 The analysis results are summarized as following;
 1. Product category 'TECHNOLOGY' as the highest sales.
 2. The regions with the Top 3 sales are;
    |Region|Total Sales|
    |------|-----------|
    |West  |#3,597,549.329|
    |Ontario|#3,063,212.527|
    Prane|#2,837,304.650|

    The regions with Bottom 3 are;
    |Region|Total Sales|
    |------|-----------|
    |Nunavut|#116,376.486|
    |Northwest Territories|#800,847.341|
    |Yukon|#975,867.382|
3. The total sales of 'Appliances' in Ontario is 202,346.84
4. The following are the advice for increasing the revenue from bottom 10 customers
  - Leveraging on the use of email marketing,Whatsapp,or SMS to keep them engaged with new arrivals,offers or the company success stories.
  - Introducing gifts or discounts
  - Offer loyalty reward
  - Conduct feedback interviews or send short surveys to learn why they are spending less.
  - Recommend complementary products or upgrades.
5. Delivery Truck incurred the most cost of #51,971.940
6. Five customer are the most valuables with their products. They are;
   |Customer Name|Product|
   |-----------|----------|
   |Emily Phan|Office Supplies,Furniture and Technology|
   |Deborah Brumfield|Office Supplies,Furniture and Technology|
   |Roy Skaria|Office Supplies,Furniture and Technology|
   |Sylvia Foulston|Office Supplies,Furniture and Technology|
   |Grant Carrol|Office Supplies,Furniture and Technology|
7. Dennis Kane is a small business with the highest sales of #75,967.591
8. Adam Hart has the most number of order(27 Orders) from 2009-2012
9. The most profitable consumer customer as a total profit of #34,005.440
10. 872 customers returned items in Consumer,Small business,Home office and Corporate.
11. Delivery Truck used for high priority orders caused unnecessary shipping cost and delay.
    Therefore,55% of low priority orders was used on Express Air,indicating a potential overspend on fast delivery for non_urgent items.So, cost usage in this terms is not appropraite.

### Recommendations
Based on the analysis,I recommen the following actions; 
 - Reward top customers with loyalty programs.
 - Optimize shipping by aligning order priority with appropriate shipping mode
    - For Example: Critical-Express Air,
    - Low-Delivery Truck.
 - Retain low-value customers through targeted offers/discounts or loyalty programs  
 - Monitor returns in the Office Home,Technology,Furniture segment and improve product info accuracy.

### Limitations
I had to change some of my datatype with int. before i can successfully load the data set which would have affected my analysis.

### References
1. Incubator Hub.(2025.May 19).Sql class on creating View using JOIN [video].https://www.youtube.com/live/9J2-a81TrZg?si=3vknoceAqpsxNNBc
2. OpenAI.(2025,June 28).Syntax Error.https://chat.openai.com
3. Smith,J.(2022).How to analyse sales data.DataPro Solutions.https://www.dataprosolutions.com/analyse-sales-data


    
