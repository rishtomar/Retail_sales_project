# 🛍️ Retail Sales Analysis – SQL Project

## 📌 Project Overview

**Project Title:** Retail Sales Analysis
**Database:** `p1_retail_db`

This project demonstrates practical SQL skills used by data analysts to explore, clean, and analyze retail sales data. The project covers database setup, data cleaning, exploratory data analysis (EDA), and solving business problems using SQL queries.

The primary goal of this project is to simulate a real-world retail analytics workflow and build a strong foundation in SQL-based data analysis.

---

## 🎯 Project Objectives

* Set up a retail sales database
* Perform data cleaning and validation
* Conduct exploratory data analysis (EDA)
* Solve real-world business problems using SQL
* Generate meaningful insights from retail sales data

---

# 🗂️ Project Structure

---

## 1️⃣ Database Setup

### 🔹 Step 1: Create Database

```sql
CREATE DATABASE p1_retail_db;
```

---

### 🔹 Step 2: Create Table

```sql
CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

### 📊 Table Description

The `retail_sales` table stores:

* Transaction details (ID, Date, Time)
* Customer information (ID, Gender, Age)
* Product category details
* Sales metrics (Quantity, Price, COGS, Total Sale)

---

# 2️⃣ Data Exploration & Cleaning

Before analysis, data quality checks were performed.

---

### 🔹 Question 1: Determine the total number of records in the dataset.

```sql
SELECT COUNT(*) 
FROM retail_sales;
```

---

### 🔹 Question 2: Find out how many unique customers are in the dataset.

```sql
SELECT COUNT(DISTINCT customer_id) 
FROM retail_sales;
```

---

### 🔹 Question 3: Identify all unique product categories in the dataset.

```sql
SELECT DISTINCT category 
FROM retail_sales;
```

---

### 🔹 Question 4: Check for any null values in the dataset.

```sql
SELECT *
FROM retail_sales
WHERE 
    sale_date IS NULL OR 
    sale_time IS NULL OR 
    customer_id IS NULL OR 
    gender IS NULL OR 
    age IS NULL OR 
    category IS NULL OR 
    quantity IS NULL OR 
    price_per_unit IS NULL OR 
    cogs IS NULL;
```

---

### 🔹 Question 5: Delete records with missing or null values.

```sql
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR 
    sale_time IS NULL OR 
    customer_id IS NULL OR 
    gender IS NULL OR 
    age IS NULL OR 
    category IS NULL OR 
    quantity IS NULL OR 
    price_per_unit IS NULL OR 
    cogs IS NULL;
```

---

# 3️⃣ Data Analysis & Business Questions

---

### 🔹 Question 6: Write a SQL query to retrieve all columns for sales made on '2022-11-05'.

```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

---

### 🔹 Question 7: Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of November 2022.

```sql
SELECT *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND quantity >= 4;
```

---

### 🔹 Question 8: Write a SQL query to calculate the total sales (total_sale) for each category.

```sql
SELECT 
    category,
    SUM(total_sale) AS net_sales,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
```

---

### 🔹 Question 9: Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

---

### 🔹 Question 10: Write a SQL query to find all transactions where the total_sale is greater than 1000.

```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
```

---

### 🔹 Question 11: Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
```

---

### 🔹 Question 12: Write a SQL query to calculate the average sale for each month and find out the best-selling month in each year.

```sql
SELECT 
    year,
    month,
    avg_sale
FROM 
(
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS ranked_months
WHERE rank = 1;
```

---

### 🔹 Question 13: Write a SQL query to find the top 5 customers based on the highest total sales.

```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

---

### 🔹 Question 14: Write a SQL query to find the number of unique customers who purchased items from each category.

```sql
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

---

### 🔹 Question 15: Write a SQL query to create each shift and calculate the number of orders (Example: Morning <12, Afternoon Between 12 & 17, Evening >17).

```sql
WITH hourly_sales AS
(
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sales
GROUP BY shift;
```

---

# 📊 Key Insights

* Sales are distributed across multiple product categories such as Clothing and Beauty.
* Several high-value transactions exceed 1000 in total sales.
* Monthly analysis reveals peak-performing months within each year.
* Top customers contribute significantly to total revenue.
* Customer purchasing behavior varies by time of day (Morning, Afternoon, Evening shifts).

---

# 🏁 Conclusion

This project serves as a comprehensive introduction to SQL for data analysis. It covers:

* Database creation
* Data cleaning
* Aggregations and grouping
* Window functions
* Business-oriented SQL problem solving

The analysis provides valuable insights into customer behavior, sales trends, and product performance, helping support data-driven business decisions.



