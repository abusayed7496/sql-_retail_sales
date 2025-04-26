
                                                        Retail Sales Analysis   

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

                                                              Objectives

1. Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. Data Cleaning**: Identify and remove any records with missing or null values.
3. xploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.


                                                            Table Creation:
-  A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql


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
```

                                                          Data Exploration & Cleaning

- Record Count**: Determine the total number of records in the dataset.
- Customer Count**: Find out how many unique customers are in the dataset.
- Category Count**: Identify all unique product categories in the dataset.
- Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT *
FROM retail_sales;

SELECT COUNT(*)
FROM retail_sales;

SELECT COUNT(DISTINCT customer_id)
FROM retail_sales;

SELECT * 
FROM retail_sales
WHERE transactions_id IS NULL;

SELECT * 
FROM retail_sales
WHERE sale_time IS NULL;

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

```

                                       ---data analysis & key business problem--

--1.Write a SQL query to retrieveHow many clothing items were sold each month?--
```sql
 SELECT * FROM retail_sales
 WHERE sale_date = '2022-11-05';
```


--2.Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022--Write a SQL query to calculate the total sales (total_sale) for each category--
```sql
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

```

--3.Write a SQL query to calculate the total sales (total_sale) for each category.--

```sql

SELECT category,
        SUM(total_sale) as total_sales,
		COUNT(*)AS total_Order
FROM retail_sales
GROUP BY category;

```

--4.Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
```sql
SELECT AVG (age)
FROM retail_sales
WHERE category = 'Beauty';
```

--5.Write a SQL query to find all transactions where the total_sale is greater than 1000--
```sql
SELECT *
FROM retail_sales
WHERE total_sale>1000
ORDER BY total_sale;

```

--6.Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
```sql

SELECT 
    category,
     gender,
     COUNT(*) as total_number_transictions
FROM retail_sales
GROUP BY category,gender;
```

--7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
WITH monthly_avg_sales AS (
```sql
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

```

--8.Write a SQL query to find the top 5 customers based on the highest total sales--
SELECT 
```sql
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
```

--9.What is the distribution of sales orders across different time shifts (Morning, Afternoon, Evening) for each category?--:
```sql
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

                                                        Findings
Customer Demographics: The dataset includes customers from diverse age groups, with sales spread across categories like Clothing and Beauty.
High-Value Transactions: Multiple transactions exceeded a total sale amount of 1000, highlighting premium purchases.
Sales Trends: Monthly sales analysis reveals fluctuations, helping identify peak seasons.
Customer Insights: The analysis pinpoints top-spending customers and identifies the most popular product categories.
                            
			                               Reports

Sales Summary: A detailed report on total sales, customer demographics, and category performance.
Trend Analysis: Insights into sales trends across various months and shifts.
Customer Insights: Reports on top customers and the unique customer count per category.

                                                      Conclusion

This project offers a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory analysis, and business-driven SQL queries. The insights gained from this project can help inform business decisions by understanding sales patterns, customer behavior, and product performance.

##                                                   Author - Md abu sayed





