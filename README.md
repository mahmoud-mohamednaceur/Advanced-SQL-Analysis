# Advanced-SQL-Analysis


                                                                                 **Quick set up**  

To run this project, you'll need the following three thing :

1. **SQL Server Express (Free)**: This is the server where the database will reside. You can download it for free from Microsoft's official website.
    - [***Download SQL Server Express***](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)
2. **SQL Server Management Studio (SSMS)**: SSMS is the graphical user interface (GUI) for interacting with your SQL database, making it easier to run queries and manage your database.
    - [***Download SQL Server Management Studio (SSMS)***](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)







### Project Objective and Purpose

The purpose of this project is to demonstrate my proficiency in SQL by solving real-world business problems and performing complex data analysis. Through a series of SQL queries, this project tackles various tasks commonly faced in data management and reporting. These tasks test a wide range of SQL skills, including data retrieval, aggregation, window functions, and handling missing data.

By completing this project, I aim to validate my SQL capabilities in a practical context, showcasing my ability to perform tasks essential to database management, reporting, and decision-making in business environments.



![sql-data-model](https://github.com/user-attachments/assets/3ca5a823-3081-47db-a51c-b09667663425)
---

### Project Tasks

### **Advanced Tasks**:

- **Task 3:** Calculate total sales per customer, including customer details
- **Task 5 Alternative:** Find total sales for each product using window functions
- **Task 6:** Calculate the percentage contribution of each product's sales to the total sales
- **Task 8:** Find highest and lowest sales for each product
- **Task 10:** Calculate deviation of each salary from the max and min salary
- **Task 11:** Rank the orders from highest to lowest based on sales
- **Task 12:** Find the top highest sales for each product
- **Task 13:** Segment all orders into categories "high", "medium", and "low" based on sales
- **Task 14:** Find products within the top 40% of sales
- **Task 14 Alternative:** Perform month-by-month sales analysis
- **Task 15:** Find the average days between consecutive orders for each customer
- **Task 16:** Calculate the difference between current sales and the highest/lowest sales
- **Task 17:** Combine customer and employee data with unique values (no duplicates)
- **Task 21:** Find employees who are also customers
- **Task 22:** Report showing total sales for categories: high, medium, and low
- **Task 23:** Retrieve employee details with gender displayed as full text
- **Task 25:** Display the full name of customers in a single field by merging their first and last names, and add 10 bonus points to each customer's score
- **Task:** Compare the average salary of men vs women

### **Medium Tasks**:

- **Task 0:** Display all records from the Orders table
- **Task 1:** Count the number of NULL values in the BillAddress column
- **Task 2:** Calculate the total number of orders
- **Task 4:** Check for duplicate rows in the Orders table based on OrderID
- **Task 5:** Find total sales for each product using GROUP BY
- **Task 7:** Calculate average sales for each product considering NULL values
- **Task 9:** Show the employee with the highest salary
- **Task 18:** Combine customer and employee data including duplicates
- **Task 19:** Find customer names not present in the employee list
- **Task 24:** Count how many times each customer made an order with sales greater than 30
- **Task 26:** Replace all NULL values with appropriate defaults (e.g., score = 0), and add 10 to the score
- **Task 27:** Sort customers by score from lowest to highest, with NULL values appearing last
- **Task 28:** Show a list of all customers who have scores
- **Task 30:** Identify customers who don’t have any score
- **Task 31:** Place all customer details who have not placed any orders
- **Task 32:** Ensure that only NULLs and empty strings are used, avoiding any blank spaces

```sql
/*
This SQL script was created to showcase my proficiency in solving business problems and performing complex data analysis using SQL queries. 
The code is structured in a way that addresses multiple tasks commonly encountered in data management and reporting scenarios. 
The aim is to check and validate my SQL skills by solving real-world problems . 
*/

-- Task 0: Display all records from the Orders table
-- Task 1: Count the number of NULL values in the BillAddress column
-- Task 2: Calculate the total number of orders
-- Task 3: Calculate total sales per customer, including customer details
-- Task 4: Check for duplicate rows in the Orders table based on OrderID
-- Task 5: Find total sales for each product using GROUP BY
-- Task 5 Alternative: Find total sales for each product using window functions
-- Task 6: Calculate the percentage contribution of each product's sales to the total sales
-- Task 7: Calculate average sales for each product considering NULL values
-- Task 8: Find highest and lowest sales for each product
-- Task 9: Show the employee with the highest salary
-- Task 10: Calculate deviation of each salary from the max and min salary
-- Task 11: Rank the orders from highest to lowest based on sales
-- Task 12: Find the top highest sales for each product
-- Task 13: Segment all orders into categories "high", "medium", and "low" based on sales
-- Task 14: Find products within the top 40% of sales
-- Task 14 Alternative: Perform month-by-month sales analysis
-- Task 15: Find the average days between consecutive orders for each customer
-- Task 16: Calculate the difference between current sales and the highest/lowest sales
-- Task 17: Combine customer and employee data with unique values (no duplicates)
-- Task 18: Combine customer and employee data including duplicates
-- Task 19: Find customer names not present in the employee list
-- Task 21: Find employees who are also customers
-- Task 22: Report showing total sales for categories: high, medium, and low
-- Task 23: Retrieve employee details with gender displayed as full text
-- Task 24: Count how many times each customer made an order with sales greater than 30
-- Task 25: Display the full name of customers in a single field by merging their first and last names, and add 10 bonus points to each customer's score
-- Task 26: Replace all NULL values with appropriate defaults (e.g., score = 0), and add 10 to the score
-- Task 27: Sort customers by score from lowest to highest, with NULL values appearing last
-- Task 28: Show a list of all customers who have scores
-- Task 30: Identify customers who don’t have any score
-- Task 31: Place all customer details who have not placed any orders
-- Task 32: Ensure that only NULLs and empty strings are used, avoiding any blank spaces
-- Task: Compare the average salary of men vs women

-- Task 0: Display all records from the Orders table
SELECT * 
FROM Sales.Orders;

-- Task 1: Count the number of NULL values in the BillAddress column
SELECT 
    BillAddress,
    COUNT(*) OVER() AS NullValueCount
FROM 
    Sales.Orders
WHERE 
    BillAddress IS NULL;

-- Task 2: Calculate the total number of orders
SELECT 
    COUNT(OrderID) OVER() AS TotalNumberOfOrders 
FROM 
    Sales.Orders;

-- Task 3: Calculate total sales per customer, including customer details
SELECT 
    CustomerID,
    Sales,
    SUM(Sales) OVER (PARTITION BY CustomerID) AS TotalSalesPerCustomer
FROM 
    Sales.Orders;

-- Task 4: Check for duplicate rows in the Orders table based on OrderID
SELECT * 
FROM (
    SELECT 
        OrderID, 
        COUNT(*) OVER (PARTITION BY OrderID) AS Row_Count
    FROM 
        Sales.Orders
) AS CheckForDuplicates
WHERE 
    Row_Count > 1;

-- Task 5: Find total sales for each product using GROUP BY
SELECT 
    ProductID, 
    SUM(Sales) AS TotalSales
FROM 
    Sales.Orders
GROUP BY 
    ProductID;

-- Task 5 Alternative: Find total sales for each product using window functions
SELECT 
    ProductID, 
    Sales,
    SUM(Sales) OVER (PARTITION BY ProductID) AS TotalSales
FROM 
    Sales.Orders;

-- Task 6: Calculate the percentage contribution of each product's sales to the total sales
SELECT  
    ProductID,
    Sales,
    CONVERT(FLOAT, SUM(Sales) OVER (PARTITION BY ProductID)) AS TotalSalesPerProduct,
    CONVERT(FLOAT, SUM(Sales) OVER()) AS TotalSales,
    (CONVERT(FLOAT, SUM(Sales) OVER (PARTITION BY ProductID)) / CONVERT(FLOAT, SUM(Sales) OVER())) * 100 AS ContributionToTotalSales
FROM 
    Sales.Orders;

-- Task 7: Calculate average sales for each product considering NULL values
-- Scenario 1: Treat NULL values as zero
SELECT  
    ProductID,
    Sales,
    AVG(COALESCE(Sales, 0)) OVER (PARTITION BY ProductID) AS AvgSalesPerProduct
FROM 
    Sales.Orders;

-- Scenario 2: Ignore NULL values
SELECT  
    ProductID,
    Sales,
    AVG(Sales) OVER (PARTITION BY ProductID) AS AvgSalesPerProduct
FROM 
    Sales.Orders;

-- Task 8: Find highest and lowest sales for each product
SELECT  
    ProductID,
    Sales,
    MAX(Sales) OVER (PARTITION BY ProductID) AS MaximumSales,
    MIN(Sales) OVER (PARTITION BY ProductID) AS MinimumSales
FROM 
    Sales.Orders;

-- Task 9: Show the employee with the highest salary
SELECT * 
FROM (
    SELECT   
        *,
        MAX(Salary) OVER () AS HighestSalary
    FROM 
        Sales.Employees
) AS HighestSalaryTable 
WHERE 
    Salary = HighestSalary;

-- Task 10: Calculate deviation of each salary from the max and min salary
SELECT 
    EmployeeID,
    Salary,
    MAX(Salary) OVER () AS HighestSalary,
    MIN(Salary) OVER () AS LowestSalary,
    MAX(Salary) OVER () - Salary AS DeviationFromMaxSalary,
    MIN(Salary) OVER () - Salary AS DeviationFromMinSalary
FROM 
    Sales.Employees;

-- Task 11: Rank the orders from highest to lowest based on sales
SELECT 
    OrderID, 
    Sales, 
    ROW_NUMBER() OVER (ORDER BY Sales DESC) AS Ranking
FROM 
    Sales.Orders;

-- Task 12: Find the top highest sales for each product
-- Solution 1: Find the max sales for each product
SELECT  
    ProductID, 
    MAX(Sales) OVER (PARTITION BY ProductID ORDER BY Sales DESC) AS HighestSales
FROM  
    Sales.Orders;

-- Solution 2: Rank sales by product and select the highest ranked
WITH HighestSales AS (
    SELECT  
        ProductID, 
        Sales, 
        ROW_NUMBER() OVER (PARTITION BY ProductID ORDER BY Sales DESC) AS Rank
    FROM  
        Sales.Orders
)
SELECT * 
FROM HighestSales 
WHERE Rank = 1;

-- Task 13: Segment all orders into categories "high", "medium", and "low" based on sales
SELECT  
    OrderID, 
    Sales, 
    NTILE(3) OVER (ORDER BY OrderID) AS BucketNumber, 
    CASE 
        WHEN NTILE(3) OVER (ORDER BY OrderID) = 1 THEN 'low'  
        WHEN NTILE(3) OVER (ORDER BY OrderID) = 2 THEN 'medium'   
        WHEN NTILE(3) OVER (ORDER BY OrderID) = 3 THEN 'high'  
        ELSE NULL 
    END AS Category
FROM  
    Sales.Orders;

-- Task 14: Find products within the top 40% of sales
WITH Contribution AS (
    SELECT
        ProductID, 
        Sales, 
        PERCENT_RANK() OVER (ORDER BY Sales DESC) AS PercentContribution
    FROM 
        Sales.Orders
)
SELECT * 
FROM Contribution 
WHERE PercentContribution <= 0.40;

-- Task 14 (Alternative): Perform month-by-month sales analysis
SELECT 
    OrderDate,
    DATENAME(MONTH, OrderDate) AS MonthName,
    Sales,
    LEAD(Sales, 1, 9999) OVER (ORDER BY DATEPART(MONTH, OrderDate)) AS NextMonthSales,
    LAG(Sales, 1, 9999) OVER (ORDER BY DATEPART(MONTH, OrderDate)) AS PreviousMonthSales,
    Sales - LEAD(Sales, 1, 9999) OVER (ORDER BY DATEPART(MONTH, OrderDate)) AS DifferenceToNextMonth,
    Sales - LAG(Sales, 1, 9999) OVER (ORDER BY DATEPART(MONTH, OrderDate)) AS DifferenceToPreviousMonth,
    ROUND(CONVERT(FLOAT, (Sales - LEAD(Sales, 1, 9999) OVER (ORDER BY DATEPART(MONTH, OrderDate)))) 
          / CONVERT(FLOAT, Sales), 2) AS PercentageChangeToNextMonth
FROM 
    Sales.Orders;

-- Task 15: Find the average days between consecutive orders for each customer
WITH AvgDays AS (
    SELECT 
        CustomerID, 
        OrderID, 
        OrderDate AS CurrentDate, 
        LEAD(OrderDate, 1, NULL) OVER (PARTITION BY CustomerID ORDER BY OrderDate ASC) AS NextOrderDate,
        DATEDIFF(DAY, OrderDate, LEAD(OrderDate, 1, NULL) OVER (PARTITION BY CustomerID ORDER BY OrderDate ASC)) AS DaysBetweenOrders
    FROM 
        Sales.Orders
)
SELECT 
    CustomerID, 
    COALESCE(AVG(DaysBetweenOrders), 9999) AS AvgDaysBetweenOrders
FROM 
    AvgDays
GROUP BY 
    CustomerID
ORDER BY 
    AvgDaysBetweenOrders ASC;

-- Task 16: Calculate the difference between current sales and the highest/lowest sales
SELECT 
    Sales, 
    LAST_VALUE(Sales) OVER (ORDER BY Sales ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS HighestSales,
    FIRST_VALUE(Sales) OVER (ORDER BY Sales) AS LowestSales,
    Sales - LAST_VALUE(Sales) OVER (ORDER BY Sales ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS DiffToHighestSales, 
    Sales - FIRST_VALUE(Sales) OVER (ORDER BY Sales) AS DiffToLowestSales
FROM 
    Sales.Orders;

-- Task 17: Combine customer and employee data with unique values (no duplicates)
SELECT 
    FirstName AS Vorname, 
    LastName AS Nachname 
FROM 
    Sales.Customers   
UNION 
SELECT 
    FirstName, 
    LastName 
FROM 
    Sales.Employees;

-- Task 18: Combine customer and employee data including duplicates
SELECT 
    FirstName AS Vorname, 
    LastName AS Nachname 
FROM 
    Sales.Customers   
UNION ALL
SELECT 
    FirstName, 
    LastName 
FROM 
    Sales.Employees;

-- Task 19: Find customer names not present in the employee list
SELECT 
    FirstName, 
    LastName 
FROM 
    Sales.Customers 
EXCEPT 
SELECT 
    FirstName, 
    LastName 
FROM 
    Sales.Employees;

-- Task 21: Find employees who are also customers
SELECT 
    FirstName, 
    LastName 
FROM 
    Sales.Employees 
INTERSECT 
SELECT 
    FirstName, 
    LastName 
FROM 
    Sales.Customers;

-- Task 22: Report showing total sales for categories: high, medium, and low
WITH SalesCategory AS (
    SELECT 
        OrderID, 
        ProductID, 
        Sales, 
        CASE
            WHEN Sales > 50 THEN 'high' 
            WHEN Sales BETWEEN 21 AND 50 THEN 'medium'  
            ELSE 'low'  
        END AS SalesCategory
    FROM 
        Sales.Orders    
)
SELECT  
    SalesCategory, 
    SUM(Sales) AS TotalSales 
FROM 
    SalesCategory  
GROUP BY 
    SalesCategory 
ORDER BY 
    TotalSales DESC;

-- Task 23: Retrieve employee details with gender displayed as full text
SELECT
    Gender, 
    CASE  
        WHEN Gender = 'M' THEN 'Male'  
        ELSE 'Female'  
    END AS GenderType,
    FirstName, 
    LastName
FROM 
    Sales.Employees;

-- Task 24: Count how many times each customer made an order with sales greater than 30
SELECT 
    CustomerID,
    SUM(CASE WHEN Sales > 30 THEN 1 ELSE 0 END) AS OrderCount
FROM 
    Sales.Orders   
GROUP BY 
    CustomerID;

-- task 25  :  display  the full name  of customers  in a  single  field 
-- by merging  their first adn last  names  
-- and  add 10  bonus  points to each customer s score  

-- Task 26: Replace all NULL values with appropriate defaults.
-- For 'score', replace NULL with 0.
-- For 'firstname' and 'lastname', replace NULL with an empty string.
-- Additionally, calculate a new column 'scoreplus10' by adding 10 to the 'score'.
-- Concatenate 'firstname' and 'lastname' to form a 'fullname' field.

SELECT 
    COALESCE(score, 0) AS score,                      -- Replace NULL score with 0
    COALESCE(score, 0) + 10 AS scoreplus10,           -- Add 10 to the score, replacing NULL with 0
    COALESCE(firstname, '') AS firstname,             -- Replace NULL in firstname with an empty string
    COALESCE(lastname, '') AS lastname,               -- Replace NULL in lastname with an empty string
    COALESCE(firstname, '') + ' ' + COALESCE(lastname, '') AS fullname  -- Concatenate firstname and lastname, separated by a space
FROM 
    Sales.Customers;

-- Task 27: Sort customers by score from lowest to highest, with NULL values appearing last.

-- Solution 1: Using COALESCE to handle NULL values.
-- Replace NULL scores with a very low value (-9999) to ensure they appear last when sorting in ascending order.
SELECT 
    * 
FROM 
    Sales.Customers 
ORDER BY 
    COALESCE(Score, -9999) ASC;  -- NULLs are replaced with -9999 to appear last

-- Solution 2: Using a flag to explicitly move NULL scores to the bottom.
-- Create a flag column where NULL scores are marked with -1, and non-NULL scores are marked with 1.
-- Sort by this flag first, then by score in ascending order.
SELECT 
    *, 
    CASE 
        WHEN Score IS NULL THEN -1 
        ELSE 1 
    END AS NullFlag  -- Flag to push NULL scores to the bottom
FROM 
    Sales.Customers 
ORDER BY 
    NullFlag DESC,   -- Sort by flag first (NULLs last)
    Score ASC;       -- Then sort by score in ascending order

--- task  28   :  show  a list of  all costumers who have scores

-- Task: Filter customers to only include those with non-NULL scores.

-- Solution 1: Using a common table expression (CTE) with a flag to mark non-NULL scores.
-- The flag column is used to differentiate between NULL and non-NULL scores, where -1 represents NULL and 1 represents non-NULL.
WITH Solution_Query AS (
    SELECT 
        *, 
        CASE  
            WHEN Score IS NULL THEN -1 
            ELSE 1 
        END AS Flag  -- Create a flag to identify NULL and non-NULL scores
    FROM 
        Sales.Customers
)
SELECT 
    * 
FROM 
    Solution_Query 
WHERE 
    Flag = 1;  -- Filter to include only customers with non-NULL scores

-- Solution 2: Directly filtering rows with non-NULL scores.
-- This approach avoids using a CTE and filters the rows based on the condition that Score is not NULL.
SELECT 
    * 
FROM 
    Sales.Customers 
WHERE 
    Score IS NOT NULL;  -- Only include customers where Score is not NULL  ; 

-- task 30   : identify customers  who dont have any score 
select  * from  Sales.Customers  ;
select  *  from  Sales.Orders  ; 

-- ta k 31  :  place all costumers  details who have not placed any orders 

select  C.*   ,   O.orderid  from   sales.Customers C   Left join  sales.orders  O  on C.CustomerID   = O.CustomerID where O.CustomerID is  null    ;

-- Task 32: Policy - Ensure that only NULLs and empty strings are used, and avoid any blank spaces.

-- Step 1: Identify if there are any blank spaces in the dataset.

SELECT 
    OrderStatus,
    DATALENGTH(OrderStatus) AS OriginalLength,                            -- Original length in bytes
    DATALENGTH(LTRIM(RTRIM(OrderStatus))) AS TrimmedLength,               -- Length after trimming leading and trailing spaces
    (DATALENGTH(OrderStatus) - DATALENGTH(LTRIM(RTRIM(OrderStatus)))) AS DifferenceInLength,  -- Difference in length due to trimming
    LTRIM(RTRIM(OrderStatus)) AS CleanedOrderStatus                       -- OrderStatus with leading and trailing spaces removed
FROM 
    Sales.Orders;

-- compare te avg salary  of  mann vs women 
select  
    Gender , 
    firstname , 
	lastname , 
	salary  , 
	AVG(convert(float  , salary ) ) over  ( partition by gender order by salary asc rows between unbounded preceding and unbounded following) as avg_salary_by_gender  

from 
	Sales.Employees ; 

```
