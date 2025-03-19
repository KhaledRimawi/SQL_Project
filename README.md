# 🎯 SQL_Project: Investigate a Relational Database  

## 📌 Overview  
This project is part of the **Palestine Launchpad with Google – Programming for Data Science with Python Nanodegree Program**.  
The goal is to analyze a relational database using **SQL** to extract meaningful insights.  

### 🔥 SQL Techniques Used:  
- ✅ **SQL Joins**  
- ✅ **SQL Aggregations**  
- ✅ **SQL Subqueries & Temporary Tables**  
- ✅ **SQL Data Cleaning**  
- ✅ **SQL Window Functions**  
- ✅ **SQL Advanced JOINS & Performance Tuning**  

---

## 🚀 SQL Queries & Analysis  

### 📂 Set 1: Family-Friendly Movies Analysis  

#### 🎬 Q1: Most Popular Family Movies  
We want to understand which movies families are watching the most.  
The following genres are considered family-friendly: **Animation, Children, Classics, Comedy, Family, and Music**.  

**Query:**
```sql
SELECT f.title AS film_title, c.name AS category_name, COUNT(*) AS rental_count
FROM film f
JOIN film_category fc ON f.film_id = fc.film_id
JOIN category c ON c.category_id = fc.category_id
JOIN inventory i ON i.film_id = f.film_id
JOIN rental r ON r.inventory_id = i.inventory_id
WHERE c.name IN ('Animation', 'Children', 'Classics', 'Comedy', 'Family', 'Music')
GROUP BY category_name, f.title
ORDER BY category_name;
---




Q2: Rental Duration Analysis by Quartiles
We want to compare the rental duration of these family-friendly movies to all movies and categorize them into quartiles.

Query:

sql
Copy
Edit
SELECT m.title, cat.name, m.rental_duration, 
       NTILE(4) OVER (ORDER BY m.rental_duration) AS standard_quartile
FROM film_category fcat
JOIN category cat ON cat.category_id = fcat.category_id
JOIN film m ON m.film_id = fcat.film_id
WHERE cat.name IN ('Animation', 'Children', 'Classics', 'Comedy', 'Family', 'Music')
ORDER BY m.rental_duration;
📂 Set 2: Customer Payment Analysis
Q1: Top 10 Paying Customers (Monthly Breakdown in 2007)
We need to identify the top 10 customers based on total payments in 2007, along with the number of transactions they made per month.

Query:

sql
Copy
Edit
WITH best_clients AS (
    SELECT 
        py.customer_id,
        CONCAT(c.first_name, ' ', c.last_name) AS full_name,
        SUM(py.amount) AS total_spent
    FROM payment py
    JOIN customer c ON py.customer_id = c.customer_id
    WHERE TO_CHAR(py.payment_date, 'YYYY') = '2007'
    GROUP BY py.customer_id, c.first_name, c.last_name
    ORDER BY total_spent DESC
    LIMIT 10
)
SELECT
    TO_CHAR(py.payment_date, 'YYYY-MM') AS payment_month,
    bc.full_name,
    COUNT(py.payment_id) AS transactions_per_month,
    SUM(py.amount) AS total_payments
FROM payment py
JOIN best_clients bc ON py.customer_id = bc.customer_id
WHERE TO_CHAR(py.payment_date, 'YYYY') = '2007'
GROUP BY bc.full_name, payment_month
ORDER BY bc.full_name, payment_month;
Q2: Monthly Payment Differences Among Top Customers
We want to track the month-to-month changes in payments for each top-paying customer in 2007 and find out the customer with the highest fluctuation.

Query:

sql
Copy
Edit
WITH top_payers AS (
    SELECT 
        pm.customer_id,
        CONCAT(c.first_name, ' ', c.last_name) AS full_name,
        SUM(pm.amount) AS total_spent
    FROM payment pm
    JOIN customer c ON pm.customer_id = c.customer_id
    WHERE TO_CHAR(pm.payment_date, 'YYYY') = '2007'
    GROUP BY pm.customer_id, c.first_name, c.last_name
    ORDER BY total_spent DESC
    LIMIT 10
),
monthly_summary AS (
    SELECT 
        tp.full_name,
        tp.customer_id,
        TO_CHAR(pm.payment_date, 'YYYY-MM') AS payment_month,
        SUM(pm.amount) AS monthly_total
    FROM payment pm
    JOIN top_payers tp ON pm.customer_id = tp.customer_id
    WHERE TO_CHAR(pm.payment_date, 'YYYY') = '2007'
    GROUP BY tp.full_name, tp.customer_id, payment_month
),
monthly_differences AS (
    SELECT 
        full_name,
        payment_month,
        monthly_total,
        LAG(monthly_total) OVER (PARTITION BY full_name ORDER BY payment_month) AS previous_month_total,
        monthly_total - LAG(monthly_total) OVER (PARTITION BY full_name ORDER BY payment_month) AS difference_from_last_month
    FROM monthly_summary
)
SELECT * 
FROM monthly_differences
WHERE difference_from_last_month IS NOT NULL
ORDER BY difference_from_last_month DESC;
🎯 Key Takeaways
✔ Applied advanced SQL queries to analyze customer behavior and movie rental trends.
✔ Used window functions to categorize rental durations into quartiles.
✔ Leveraged CTEs and aggregations for customer payment analysis.
✔ Improved query efficiency with performance tuning techniques.

A huge thank you to my mentor @Mohamad Shbaitah for his guidance and support throughout the project.




