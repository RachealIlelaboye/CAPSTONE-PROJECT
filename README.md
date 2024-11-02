# CAPSTONE-PROJECT
This is the documentation of Ladies in Tech Africa (LITA) capstone project.

# PROJECT 1 - Sales Performance Analysis for a Retail Store

## Project Overview
In this project, the sales performance of a retail store is analyzed. Sales data was explored to uncover key insights such as top-selling products, regional 
performance, and monthly sales trends. 

## Data Source
The data for this project was gotten from LITA [Official Website](https://theincubatorng.org/LITA/index.html)

## Tools Used
- Microsoft Excel [Download here](https://www.microsoft.com/en-us/microsoft-365/excel)
  1. Data exploration
  2. Report
  
- SQL Server Management Studio [Download here](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)

  Data querying
  
- PowerBI [DOWNLOAD HERE](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

  Visualization
  
- Github
  
  Portfolio building

 ## Excel
 - Data Preprocessing and cleaning
   1.   Removal of duplicates
   2.   Creation of new columns for revenue
   3.   Extraction of year and months from orderdate
      
 - Initial exploration of the sales data. Use of pivot tables to summarize 
total sales by product, region, and month.
 - Excel formulas to calculate metrics such as average sales per product and 
total revenue by region.

![Excel sales](https://github.com/user-attachments/assets/aa94278b-056a-4529-88f4-5b6f49d2468b)

## SQL
- Database creation and data Preprocessing/cleaning
``` SQL
CREATE DATABASE CAPSTONE_PROJECT;
select *
from [dbo].[Capstone Sales]
```
- The total sales for each product category
```SQL
SELECT 
    Product,
    SUM(Revenue) AS TotalSales
FROM 
    [dbo].[Capstone Sales]
GROUP BY 
    Product
ORDER BY 
    TotalSales DESC;
```
- Number of sales transactions in each region.
```SQL
SELECT 
    Region,
    SUM(REVENUE) AS NumberOfTransactions
FROM 
    [dbo].[Capstone Sales]
GROUP BY 
    Region
ORDER BY 
    NumberOfTransactions DESC;
```
- Highest-selling product by total sales value.
```SQL
SELECT
    Product, Sum(Quantity) As SalesValue
FROM
[dbo].[Capstone Sales]
GROUP BY
Product
ORDER BY
SalesValue DESC;
```
- Total revenue per product.
```SQL
SELECT
    Product, Sum(Revenue) As Revenue
FROM
[dbo].[Capstone Sales]
GROUP BY
Product
ORDER BY
Revenue DESC;
```
- Monthly sales totals for the current year.
```SQL
SELECT 
    MONTH(OrderDate) AS Month,
    SUM(Quantity) AS MonthlyTotalSales
FROM 
    [dbo].[Capstone Sales]
WHERE 
    YEAR(OrderDate) = YEAR(GETDATE())
GROUP BY 
    MONTH(OrderDate)
ORDER BY 
    Month;
```
- Top 5 customers by total purchase amount.
```SQL
SELECT 
  TOP 5 CustomerID,
    SUM(Revenue) AS TotalPurchaseAmount
FROM 
    [dbo].[Capstone Sales]
GROUP BY 
    CustomerID
ORDER BY 
    TotalPurchaseAmount DESC;
```
- Percentage of total sales contributed by each region.
```SQL
SELECT 
    Region,
    SUM(Revenue) AS TotalSales,
    (SUM(Revenue) * 100.0 / (SELECT SUM(Revenue) FROM [dbo].[Capstone Sales])) AS SalesPercentage
FROM 
    [dbo].[Capstone Sales]
GROUP BY 
    Region
ORDER BY 
    SalesPercentage DESC;
```
- Products with no sales in the last quarter.
```SQL
SELECT  Product
FROM [dbo].[Capstone Sales]
    WHERE Quantity = 0
      AND OrderDate >= DATEADD(QUARTER, -1, GETDATE());
```
## PowerBI
This dashboard visualizes the insights found in Excel and SQL. The 
dashboard includes a sales overview, top-performing products, and 
regional breakdowns.

![BI sales](https://github.com/user-attachments/assets/8727182c-a36a-4755-a578-58ea22cfd603)


# PROJECT 2 - Customer Segmentation for a Subscription Service

## Project Overview 
This project involves customer data analysis for a subscription service to identify segments and trends. The goal is to understand customer behavior, track subscription types, and identify key trends in cancellations and renewals. 

## Data Source
The data for this project was gotten from LITA [Official Website](https://theincubatorng.org/LITA/index.html)

## Tools Used
- Microsoft Excel [Download here](https://www.microsoft.com/en-us/microsoft-365/excel)
  1. Data exploration
  2. Report
  
- SQL Server Management Studio [Download here](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)

  Data querying
  
- PowerBI [DOWNLOAD HERE](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

  Visualization
  
- Github
  
  Portfolio building

## Excel 
 - Data Preprocessing and cleaning
   1.   Removal of duplicates
   2.   Creation of new columns for revenue
   3.   Extraction of year and months from orderdate

  - Customer data analysis using pivot tables to find subscription patterns.
    
  - Average subscription duration calculation and identifying the most popular 
subscription types

![Excel customer](https://github.com/user-attachments/assets/435c4e3a-3f06-4c8a-8b01-2ccd3eba5e1d)

## SQL
- Total number of customers from each region.
```SQL
SELECT 
    Region,
    COUNT(DISTINCT CustomerID) AS TotalCustomers
FROM 
    [dbo].[Captstone]
GROUP BY 
    Region
ORDER BY 
    TotalCustomers DESC;
```
- Most popular subscription type by the number of customers.
```SQL
SELECT 
   TOP 1 SubscriptionType,
    COUNT(DISTINCT CustomerID) AS NumberOfCustomers
FROM 
    [dbo].[Captstone]
GROUP BY 
    SubscriptionType
ORDER BY 
    NumberOfCustomers DESC;
```
- Cstomers who canceled their subscription within 6 months.
```SQL
SELECT 
    CustomerID,
    SubscriptionStart,
   SubscriptionEnd
FROM 
    [dbo].[Captstone]
WHERE 
    SubscriptionEnd IS NOT NULL  
    AND DATEDIFF(MONTH, SubscriptionStart, SubscriptionEnd) <= 6;
```
- The average subscription duration for all customers
```SQL
SELECT 
    AVG(DATEDIFF(MONTH, SubscriptionStart, 
        CASE 
            WHEN SubscriptionEnd IS NOT NULL THEN SubscriptionEnd 
            ELSE GETDATE() 
        END)) AS AverageSubscriptionDuration
FROM 
   [dbo].[Captstone] ;
```
- Customers with subscriptions longer than 12 months.
```SQL
SELECT 
    CustomerID
FROM 
  [dbo].[Captstone]
WHERE 
    DATEDIFF(MONTH, SubscriptionStart, 
        CASE 
            WHEN SubscriptionEnd IS NOT NULL THEN SubscriptionEnd
            ELSE GETDATE()  
        END) > 12;
```
- Total revenue by subscription type.
```SQL
SELECT 
    SubscriptionType,
    SUM(Revenue) AS TotalRevenue
FROM 
   [dbo].[Captstone] 
GROUP BY 
    SubscriptionType
ORDER BY 
    TotalRevenue DESC; 
```
- Top 3 regions by subscription cancellations.
```SQL
SELECT 
    Region,
    COUNT(Canceled) AS CancellationCount
FROM 
     [dbo].[Captstone] 
WHERE 
    Canceled ='TRUE'  
GROUP BY 
    Region
ORDER BY 
    CancellationCount DESC
```
- Total number of active and canceled subscriptions.
```SQL
SELECT 
   Canceled,
    COUNT(*) AS SubscriptionCount
FROM 
     [dbo].[Captstone] -- Replace with your actual table name
WHERE 
    Canceled IS NOT NULL
GROUP BY 
    Canceled;
```
## PowerBI
This dashboard visualizes key customer segments, cancellations, and subscription trends. Including slicers for interactive analysis

![Customer BI](https://github.com/user-attachments/assets/cc92b581-f68e-4e42-88c9-c3e28d55abe9)

[PowerBI Dashboard]

[Excel Sheet]([Racheal T. Ilelaboye project.xlsx](https://github.com/user-attachments/files/17606075/Racheal.T.Ilelaboye.project.xlsx)

[SQL scripts]
