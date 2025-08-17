# -SQL-data-analysis-on-Zomato-Transactions-PROJECT-
Analyzed Zomato transaction data using SQL to extract key business insights. Processed and queried the  dataset to determine customer spending, visit frequency, top-purchased items, membership impact on  purchases, and loyalty points earned. Ranked customer transactions and identified trends to enhance  data-driven decision-making. 
**🍴 Zomato Data Analytics Project**
Analyzed Zomato transaction data using SQL to extract key business insights. Processed and queried the 
dataset to determine customer spending, visit frequency, top-purchased items, membership impact on 
purchases, and loyalty points earned. Ranked customer transactions and identified trends to enhance 
data-driven decision-making. 

 **Project Summary**

The Zomato Database Analytics Project is designed to perform a comprehensive exploratory data analysis (EDA) and BI evaluation on a structured restaurant dataset obtained from Kaggle. The dataset is organized into five relational tables capturing details about users, restaurants, menus, food items, and order transactions.

The goal is to generate business-ready insights by addressing strategic questions such as:

Which restaurants generate the most revenue?

What cuisines are the most popular?

How does customer spending differ across demographics and geographies?

What order patterns emerge across time?

The work leverages PostgreSQL for advanced queries and Python for cleaning and visualization.

This project enables food-tech companies, delivery platforms, and restaurant chains to refine marketing, pricing, customer segmentation, and expansion strategies by converting raw data into actionable intelligence through relational modeling and advanced SQL analysis.

**Dataset, Cleaning & Visualization**

The Kaggle dataset covers users, restaurants, menus, food items, and orders, linked via relational keys. Examples include:

Orders table – sales amount, date, quantity, user ID, restaurant ID

Food table – item IDs, veg/non-veg classification

Menu table – food-to-restaurant mappings, price, cuisine type

Restaurants table – location, rating, average cost, license details

Users table – demographic data (age, income, family size, education, occupation, etc.)

Python visualizations included pie charts (e.g., veg vs. non-veg, cuisine split) and bar graphs (price categories, order counts, most popular restaurants/foods).

**Data Preparation & Cleaning**

To ensure quality analysis, several inconsistencies were fixed before querying:

Currency values standardized by converting USD to INR at a constant rate.

Restaurant ratings normalized and converted into numeric form.

Menu prices, initially stored as text, cleaned and cast to floats.

Removed orphaned records pointing to non-existent restaurants.

Cost fields stripped of non-numeric symbols and standardized.

Missing restaurant names rebuilt from URL data.

All steps were documented in data_cleaning.sql.

**ER Model Adjustments**

Post-cleaning, relational integrity issues were corrected:

Restaurant ID (INT) aligned across menu and order tables (initially VARCHAR and FLOAT mismatches).

Composite keys created in the menu table to eliminate duplicate menu IDs.

📂 Repository Structure
Zomato_Data_Analysis/
│
├── Dataset/              # Raw CSV + compressed data
├── Images/               # Visualizations + ER diagrams
├── SQL/                  # SQL cleaning + query scripts
├── Solutions/            # Outputs + dashboards
├── Requirements.txt      # Dependencies
├── README.md             # Overview
└── Licence               # License file

**Workflow**

Schema Design – defined tables & primary keys.

Data Import – loaded CSVs into PostgreSQL tables.

Cleaning – handled duplicates, nulls, and inconsistencies.

Relationship Setup – applied foreign key constraints.

SQL Analysis – queried for insights on sales, users, and menus.

Visualization – connected SQL outputs to Tableau dashboards for interactive storytelling.

 **Key Business Questions Answered**

Top 10 restaurants by revenue.

Most popular cuisines and menu diversity.

Peak ordering days and times.

Spending behavior across demographics (income, gender, age).

Vegetarian vs. non-vegetarian order share.

Customer concentration (top spenders, top cities).

Seasonal/monthly order trends.

**Sample SQL Queries**

1. Identify top 15 high-spending users:

WITH user_spending AS (
    SELECT user_id, SUM(sales_amount) AS total_spent
    FROM orders
    GROUP BY user_id
),
percentile_value AS (
    SELECT PERCENTILE_CONT(0.99) WITHIN GROUP (ORDER BY total_spent) AS threshold
    FROM user_spending
)
SELECT us.user_id, us.total_spent
FROM user_spending us, percentile_value p
WHERE us.total_spent > p.threshold
LIMIT 15;


2. Find top 10 restaurants with the largest menu variety:

SELECT r.name, COUNT(DISTINCT m.f_id) AS item_count
FROM restaurant r
JOIN menu m ON r.id = m.r_id
GROUP BY r.name
ORDER BY item_count DESC
LIMIT 10;


3. Analyze peak order weekdays:

SELECT 
  TRIM(TO_CHAR(order_date, 'Day')) AS weekday,
  EXTRACT(DOW FROM order_date) AS weekday_num,
  COUNT(*) AS total_orders,
  SUM(sales_amount) AS total_sales
FROM orders
GROUP BY weekday, weekday_num
ORDER BY weekday_num;


4. Link income groups with order activity:

SELECT 
  u.Monthly_Income, 
  COUNT(o.*) AS order_count
FROM users u
JOIN orders o ON u.user_id = o.user_id
GROUP BY u.Monthly_Income
ORDER BY order_count DESC;

**Tableau Insights**

Spending Distribution – most users spend < ₹10,000; a small minority accounts for disproportionate high spending.

Income vs. Orders – users earning > ₹50,000/month order significantly more often.

Cuisine Popularity – casual dining and quick bites dominate volumes, while fine dining drives higher order value.

Peak Times – weekends (Fri–Sun) and meal hours (lunch/dinner) see surges in orders.

**Insights & Takeaways**

Market Patterns: Metro cities (Delhi, Bangalore) dominate restaurant presence and ratings.

Customer Trends: North Indian, Chinese, and South Indian cuisines remain most ordered.

Revenue Drivers: A small segment of high-income users & top restaurants generate bulk of revenue.

Operational Strategy: Peak-time demand can guide staffing, promotions, and delivery optimization.

Menu Optimization: Balanced veg/non-veg preferences highlight the importance of variety.

**Tools & Tech Stack**

PostgreSQL – relational modeling, advanced queries.

Python (Pandas, Matplotlib, Seaborn) – preprocessing & EDA.

Tableau – interactive dashboarding.

Excel – data formatting checks.

**Real-World Applications**

Business Expansion: Identify high-performing cities, cuisines, and restaurants for scaling.

Customer Segmentation: Leverage demographic-spending links for targeted marketing.

Operational Efficiency: Forecast demand by time/day to optimize resources.

Menu & Pricing: Align pricing with customer preferences and spending power.
