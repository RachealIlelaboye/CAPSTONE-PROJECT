# LITA CAPSTONE PROJECT
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

  - The product with the highest sales is Hat with over 80,000 quantity sold. The least product in terms of sales is Jacket.
  
  - The Region with the highest sales is the South. The region with the least sales and revenue is the West.
  
  - The highest sales were recorded in the month of June.
  
  - The highest revenue is recorded in the year 2023.

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
  - A total number of 500customers is recorded for this retail store
  - Total quantity of products sold is 345,000 units
  - Total Revenue is $10,587,500
  - Customer with customerId cus1151 has the highest number of product purchases

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
  - The regions has equal number of customers
  - Basic subscription type has the highest number of subscriptions
  - The regions have equal number of subscription cancellations
  - A total of 11 customer is recorded
    
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
- The subscription service has a total of 20customers, 9 of which have cancelled.
- The total revenue is $41,465
- The average subscription duration is 365days(1 year)
- The highest revenue is recorded in 2022
- The highest subscription cancellation is recorded in year 2023
- Basic subscription type has the highest number of subscriptions as well as revenue


#Project Documents

[PowerBI Dashboard](https://github.com/user-attachments/files/17606102/Racheal.T.Ilelaboye.PowerBI.pdf)

[Excel Sheet](https://github.com/user-attachments/files/17606075/Racheal.T.Ilelaboye.project.xlsx)

[SQL script pdf 1](https://github.com/user-attachments/files/17606236/Racheal.Titilayo.Ilelaboye.Subscription.pdf)

[SQL script pdf 2](https://github.com/user-attachments/files/17606237/Racheal.Titilayo.Ilelaboye.Sales.pdf)

# Project Conclusion
In this analysis projects, I effectively utilized Excel, SQL, and Power BI to transform raw data into actionable insights. Here’s a summary of the outcomes and tools applied:
- Data Preparation: Using SQL, I imported and structured the raw data, performed initial data cleaning, and created a well-organized database. This set the foundation for accurate, consistent analysis and allowed seamless querying for key metrics.
- Exploratory Data Analysis (EDA): With Excel, I performed preliminary data exploration, calculated summary statistics, and identified key trends and patterns. Excel's pivot tables and basic statistical functions helped to quickly assess the data and identify any anomalies.
- Data Visualization and Reporting: In Power BI, I built a comprehensive dashboard, displaying key metrics, trends, and comparisons for an interactive, real-time view of the data. Visualizations enabled stakeholders to easily interpret insights, understand performance trends, and make data-driven decisions.
  
-- Key Insights:
- Customer data: Analyzed customer data
- Sales Trends: Highlighted trends in sales and subscription.
- Product/SubscriptionType Performance: Identified top and underperforming products and subscription types, providing guidance for future improvement.

This project demonstrates the power of integrated tools in data analysis, where Excel facilitated data exploration, SQL managed and queried datasets efficiently, and Power BI presented findings through accessible, interactive dashboards. This combination allowed for a streamlined, accurate, and insightful analysis, providing a solid foundation for informed business decisions.

# Project Recommendations
Based on the analysis conducted using Excel, SQL, and Power BI, here are actionable recommendations for enhancing business performance:

- Enhance Product Portfolio Management:
Focus resources on high-performing products and subscription types while re-evaluating or discontinuing consistently underperforming items. 
- Strengthen Regional Strategies:
Prioritize high-performing territories for further expansion efforts.
- Implement Regular Data Monitoring with Power BI:
Set up automated, real-time reporting in Power BI to monitor key performance metrics continuously. This will allow for quicker response to changing trends and make it easier to adjust strategies based on current insights.
- Improve Customer Retention:
Since customer retention is crucial, implement loyalty programs and post-purchase follow-ups to engage customers. Encourage repeat purchases through exclusive offers, targeted emails, and loyalty incentives for high-potential segments.






