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

