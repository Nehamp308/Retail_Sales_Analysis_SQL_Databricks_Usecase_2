# Retail Sales Analysis SQL Project

## Project Overview 

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Data Cleaning**: Identify and remove any records with missing or null values.
2. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
3. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

###  Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT * 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf;

SELECT COUNT(*) 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf;


-- CHECKING NULL VALUES --
SELECT * 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf 
WHERE 
  transactions_id IS NULL
  OR
  sale_date IS NULL
  OR
  sale_time IS NULL
  OR
  customer_id IS NULL
  OR
  gender IS NULL
  OR
  age IS NULL
  OR
  category IS NULL
  OR
  quantiy IS NULL
  OR
  price_per_unit IS NULL
  OR
  cogs IS NULL
  OR
  total_sale IS NULL;

-- REMOVING NULL VALUES --
DELETE 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
WHERE 
  transactions_id IS NULL
  OR
  sale_date IS NULL
  OR
  sale_time IS NULL
  OR
  customer_id IS NULL
  OR
  gender IS NULL
  OR
  age IS NULL
  OR
  category IS NULL
  OR
  quantiy IS NULL
  OR
  price_per_unit IS NULL
  OR
  cogs IS NULL
  OR
  total_sale IS NULL;


-- DATA EXPLORATION

-- How many sales we have?

SELECT COUNT(transactions_id) 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf;

-- How many unique customers we have?

SELECT COUNT(DISTINCT customer_id) 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf;

-- How many categories we have?

SELECT COUNT(DISTINCT category) 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf;

SELECT DISTINCT category 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf;
```

###  Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT * 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf 
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than or equal to 4 in the month of Nov-2022**:
```sql
SELECT * 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf 
WHERE category = 'Clothing' AND  quantiy >= 4 AND sale_date BETWEEN '2022-11-01' AND '2022-11-30';
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT category, SUM(total_sale) as Total_Sales, COUNT(*) as Total_Orders 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
GROUP BY 1;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT category, ROUND(AVG(age),2) as Average_Age 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
WHERE category = 'Beauty'
GROUP BY 1;
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * 
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf 
WHERE total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT gender, category, COUNT(transactions_id) as Total_Transactions
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
GROUP BY 1,2
ORDER BY 1,2;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT Yearr, Month, Average_Sales FROM(
SELECT 
  RANK() OVER(PARTITION BY Yearr ORDER BY Average_Sales DESC) as rank,
  Month,
  Yearr,
  Average_Sales
FROM (
  SELECT 
    month(sale_date) as Month,
    year(sale_date) as Yearr,
    ROUND(AVG(total_sale), 2) as Average_Sales
  FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
  GROUP BY 1, 2
  
)) AS T1
WHERE rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT customer_id, SUM(total_sale) as Total_Sales
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT category, COUNT(DISTINCT customer_id) as UNIQUE_CUSTOMERS
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf
GROUP BY 1;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH HOURLY_SALES AS (
SELECT *,
    CASE
        WHEN hour(sale_time) < 12 THEN 'Morning'
        WHEN hour(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
        END AS shift
FROM sql_warehouse.stg.sql_retail_sales_analysis_utf

) 

SELECT shift, COUNT(*) as Total_Orders
FROM HOURLY_SALES
GROUP BY 1;

```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.
