# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.


---Create Table----

CREATE TABLE retail_sales  (
     	  transactions_id INT PRIMARY KEY,
		  sale_date	DATE,
		  sale_time	 TIME,
		  customer_id INT,
		  gender VARCHAR(15),
		  age INT,
		  category VARCHAR (15),
		  VARCHAR(15),
		  quantiy INT,
		  price_per_unit FLOAT,
		  cogs FLOAT,
		  total_sale FLOAT
		  );

---DATA CLEANING---
-- 1. Retrieve all records from the retail_sales table
SELECT *
FROM retail_sales;

-- 2. Count the total number of records in the retail_sales table
SELECT COUNT(*)
FROM retail_sales;

-- 3. Count the number of unique customers in the retail_sales table
SELECT COUNT(DISTINCT customer_id)
FROM retail_sales;

-- 4. Retrieve records with NULL transaction_id values
SELECT * 
FROM retail_sales
WHERE transactions_id IS NULL;

-- 5. Retrieve records with NULL sale_time values
SELECT * 
FROM retail_sales
WHERE sale_time IS NULL;

-- 6. Retrieve records with any NULL values in key columns
SELECT * 
FROM retail_sales
WHERE 
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;

-- 7. Delete records with NULL values in key columns
DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL
    OR sale_date IS NULL
    OR sale_time IS NULL
    OR customer_id IS NULL
    OR gender IS NULL
    OR category IS NULL
    OR cogs IS NULL
    OR total_sale IS NULL;

                         ---DATA EXPLORATION---


 ---1.HOW MANY SALES WE HAVE----

 SELECT COUNT (*)AS total_sales FROM retail_sales;

----2.how many customer we have---

 SELECT COUNT (DISTINCT customer_id)AS total_customer FROM retail_sales;

---unique catagory----
SELECT DISTINCT category FROM retail_sales;


                    ---data analysis & key business problem--

--1.Write a SQL query to retrieveHow many clothing items were sold each month?--
 SELECT * FROM retail_sales
 WHERE sale_date = '2022-11-05';
 
--2.Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022--Write a SQL query to calculate the total sales (total_sale) for each category--
SELECT 
    TO_CHAR(sale_date, 'YYYY-MM') AS sale_month,
    SUM(quantiy) AS total_quantity
FROM 
    retail_sales
WHERE 
    category = 'Clothing'
GROUP BY 
    TO_CHAR(sale_date, 'YYYY-MM')
ORDER BY 
    sale_month;

--3.Write a SQL query to calculate the total sales (total_sale) for each category.--

SELECT category,
        SUM(total_sale) as total_sales,
		COUNT(*)AS total_Order
FROM retail_sales
GROUP BY category;

--4.Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

SELECT AVG (age)
FROM retail_sales
WHERE category = 'Beauty';

--5.Write a SQL query to find all transactions where the total_sale is greater than 1000--
SELECT *
FROM retail_sales
WHERE total_sale>1000
ORDER BY total_sale;

--6.Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
SELECT 
    category,
     gender,
     COUNT(*) as total_number_transictions
FROM retail_sales
GROUP BY category,gender;

--7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
WITH monthly_avg_sales AS (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale
    FROM 
        retail_sales
    GROUP BY 
        1, 2
)
SELECT 
    year,
    month,
    avg_sale,
    RANK() OVER (PARTITION BY year ORDER BY avg_sale DESC) AS month_rank
FROM 
    monthly_avg_sales
ORDER BY 
    year, month_rank;

--8.Write a SQL query to find the top 5 customers based on the highest total sales--
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM 
    retail_sales
GROUP BY 
    customer_id
ORDER BY 
    total_sales DESC
LIMIT 5;



--9.What is the distribution of sales orders across different time shifts (Morning, Afternoon, Evening) for each category?--
SELECT 
    CASE 
        WHEN EXTRACT(HOUR FROM sale_date::timestamp) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_date::timestamp) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END AS shift,
    COUNT(*) AS number_of_orders
FROM 
    retail_sales
GROUP BY 
    shift
ORDER BY 
    shift;


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

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Zero Analyst

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Stay Updated and Join the Community

For more content on SQL, data analysis, and other data-related topics, make sure to follow me on social media and join our community:

- **YouTube**: [Subscribe to my channel for tutorials and insights](https://www.youtube.com/@zero_analyst)
- **Instagram**: [Follow me for daily tips and updates](https://www.instagram.com/zero_analyst/)
- **LinkedIn**: [Connect with me professionally](https://www.linkedin.com/in/najirr)
- **Discord**: [Join our community to learn and grow together](https://discord.gg/36h5f2Z5PK)

Thank you for your support, and I look forward to connecting with you!
