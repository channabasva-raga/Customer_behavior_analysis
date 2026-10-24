# Customer Shopping Behavior Analysis

An end-to-end data analytics project: **Python (EDA & cleaning) → PostgreSQL (business SQL queries) → Power BI (dashboard) → written report**, built on 3,900 retail transactions to understand spending patterns, customer segments, and subscription behavior.

---

## Table of Contents
1. [Overview](#overview)
2. [Dataset](#dataset)
3. [Tools & Technologies](#tools--technologies)
4. [Project Workflow](#project-workflow)
5. [Dashboard](#dashboard)
6. [Key Results](#key-results)
7. [Business Recommendations](#business-recommendations)
8. [Repository Structure](#repository-structure)
9. [How to Run](#how-to-run)
10. [Skills Demonstrated](#skills-demonstrated)

---

## Overview

**Goal:** Turn raw transaction data into insights that can guide decisions on marketing, subscriptions, discounting, and product positioning.

**What this project covers:**
- Loading and profiling a dataset in Python
- Cleaning data and engineering new features
- Loading cleaned data into PostgreSQL and answering 10 business questions with SQL
- Building an interactive Power BI dashboard
- Summarizing findings and recommendations in a PDF report

---

## Dataset

| Property | Detail |
|---|---|
| File | `customer_shopping_behavior.csv` |
| Rows | 3,900 purchases |
| Columns | 18 |
| Missing data | 37 null values in `Review Rating` |

**Column groups**
- **Customer demographics:** Customer ID, Age, Gender, Location, Subscription Status
- **Purchase details:** Item Purchased, Category (Clothing, Footwear, Outerwear, Accessories), Purchase Amount (USD), Size, Color, Season
- **Shopping behavior:** Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating, Shipping Type, Payment Method

---

## Tools & Technologies

| Stage | Tools |
|---|---|
| Data analysis & cleaning | Python, Pandas, Jupyter Notebook |
| Database | PostgreSQL, SQLAlchemy, psycopg2 |
| Querying | SQL (aggregations, CTEs, window functions, CASE, subqueries) |
| Visualization | Power BI |
| Reporting | PDF report |

---

## Project Workflow

### 1. Data Loading & EDA (Python)
- Loaded the CSV with Pandas
- Used `df.info()` and `df.describe(include='all')` to understand structure, data types, and distributions
- Checked for null values with `df.isnull().sum()`

### 2. Data Cleaning & Feature Engineering (Python)
- **Missing values:** Filled the 37 missing `Review Rating` values with the **median rating of each product category**
- **Column standardization:** Converted all column names to `snake_case` (e.g. `Purchase Amount (USD)` → `purchase_amount`)
- **`age_group`:** Split customers into four equal-sized groups (quartiles) labeled *Young Adult, Adult, Middle-aged, Senior*
- **`purchase_frequency_days`:** Converted text frequency (Weekly, Monthly, Quarterly, etc.) into numeric days
- **Redundancy check:** Confirmed `discount_applied` and `promo_code_used` were identical, so dropped `promo_code_used`

### 3. Database Integration
- Connected Python to a local PostgreSQL database using SQLAlchemy
- Loaded the cleaned DataFrame into a table named `customer`

### 4. SQL Analysis (PostgreSQL)
Ten business questions answered in [`Sql_queries.sql`](Sql_queries.sql):

| # | Business Question | SQL Techniques |
|---|---|---|
| 1 | Revenue by gender | `GROUP BY`, `SUM` |
| 2 | Customers who used a discount but spent above average | Subquery |
| 3 | Top 5 products by average review rating | `AVG`, `ORDER BY`, `LIMIT` |
| 4 | Average spend: Standard vs. Express shipping | `WHERE IN`, `ROUND` |
| 5 | Subscribers vs. non-subscribers (spend & revenue) | Multi-aggregate `GROUP BY` |
| 6 | Top 5 products with highest discount rate | Conditional aggregation (`CASE`) |
| 7 | Customer segmentation (New / Returning / Loyal) | CTE + `CASE` |
| 8 | Top 3 products within each category | CTE + `ROW_NUMBER() OVER (PARTITION BY ...)` |
| 9 | Are repeat buyers more likely to subscribe? | Filtering + grouping |
| 10 | Revenue contribution by age group | `GROUP BY`, `ORDER BY` |

### 5. Dashboard (Power BI)
Built an interactive dashboard on top of the cleaned data (see next section).

### 6. Reporting
Compiled methodology, SQL outputs, dashboard, and recommendations into a PDF report.

---

## Dashboard

An interactive Power BI dashboard with filters for **Subscription Status, Gender, Category, and Shipping Type**.

**KPIs:** 3.9K customers · $59.76 average purchase amount · 3.75 average review rating

**Visuals:**
- % of customers by subscription status
- Revenue and sales by product category
- Revenue and sales by age group

<img width="1362" height="748" alt="dashboard_preview" src="https://github.com/user-attachments/assets/79c7af8e-4f3d-42b7-89a7-0659c8aaa80d" />


File: [`customer_behavior_dashboard.pbix`](customer_behavior_dashboard.pbix)

---

## Key Results

- **Total revenue:** about $233K across 3,900 purchases (average ≈ $59.76 per purchase)
- **Gender:** Male customers generated $157,890 vs. $75,191 for female customers. Males make up ~68% of the customer base, so most of this gap comes from customer count rather than higher individual spend.
- **Subscriptions:** Only 27% of customers subscribe, and their average spend ($59.49) is nearly identical to non-subscribers ($59.87), so subscribing doesn't currently translate into higher spend.
- **Customer segments:** ~80% of customers (3,116) fall into the **Loyal** segment, 701 are **Returning**, and 83 are **New**.
- **Repeat buyers:** Among customers with more than 5 previous purchases, about 28% subscribe, roughly the same as the overall rate. Repeat buying alone does not predict subscribing.
- **Shipping:** Express shipping customers spend slightly more ($60.48) than Standard ($58.46).
- **Discounts:** Hat, Sneakers, Coat, Sweater, and Pants have the highest discount rates (≈ 47–50% of purchases). 839 customers used a discount and still spent above the average.
- **Top-rated products:** Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), Skirt (3.78).
- **Age groups:** Revenue is fairly evenly spread, with Young Adults highest ($62,143) and Seniors lowest ($55,763).

---

## Business Recommendations

- **Boost subscriptions:** Offer clear, exclusive subscriber benefits. Subscribers currently don't spend more than non-subscribers.
- **Loyalty programs:** Reward repeat buyers to keep them in the Loyal segment and nudge them toward subscribing.
- **Review discount policy:** Heavily discounted products may be losing margin; test whether discounts are needed to drive sales.
- **Product positioning:** Feature top-rated and best-selling products in campaigns.
- **Targeted marketing:** Focus on high-revenue age groups and express-shipping users.

---

## Repository Structure

```
├── customer_shopping_behavior.csv        # Raw dataset
├── customer_shopping_behavior.ipynb      # Python: EDA, cleaning, feature engineering, load to PostgreSQL
├── Sql_queries.sql                       # 10 business-question SQL queries
├── customer_behavior_dashboard.pbix      # Power BI dashboard
├── Customer_Shopping_Behavior_Analysis.pdf  # Full project report
├── images/
│   └── dashboard.png                     # Dashboard screenshot
└── README.md
```

---

## How to Run

> You don't need to run anything to evaluate this project: the results, SQL outputs, and dashboard are all shown in the report and above. These steps are for anyone who wants to reproduce it.

**Prerequisites:** Python 3.8+, PostgreSQL, Power BI Desktop (Windows)

1. **Clone the repository**
   ```bash
   git clone https://github.com/channabasva-raga/Customer_behavior_analysis.git
   cd Customer_behavior_analysis
   ```

2. **Install Python dependencies**
   ```bash
   pip install pandas jupyter sqlalchemy psycopg2-binary
   ```

3. **Create a PostgreSQL database**
   ```sql
   CREATE DATABASE customer_behavior;
   ```

4. **Run the notebook**
   - Open `customer_shopping_behavior.ipynb` in Jupyter
   - Keep the CSV in the same folder as the notebook (the path is relative)
   - Set your PostgreSQL credentials as environment variables before launching Jupyter (no passwords are stored in the code):
     ```bash
     # macOS / Linux
     export PG_USER=postgres
     export PG_PASSWORD=your_password

     # Windows (PowerShell)
     $env:PG_USER="postgres"
     $env:PG_PASSWORD="your_password"
     ```
   - If your host, port, or database name differ from `localhost`, `5432`, and `customer_behavior`, edit them in the connection cell
   - Run all cells. This cleans the data and creates the `customer` table in PostgreSQL.

5. **Run the SQL queries**
   - Open `Sql_queries.sql` in pgAdmin, DBeaver, or `psql`
   - Run each query against the `customer_behavior` database

6. **Open the dashboard**
   - Open `customer_behavior_dashboard.pbix` in Power BI Desktop
   - If prompted, refresh or re-point the data source to your local PostgreSQL database or the cleaned dataset

---

## Skills Demonstrated

- Data cleaning and feature engineering with Pandas
- Writing analytical SQL (CTEs, window functions, subqueries, conditional aggregation)
- Connecting Python to a relational database
- Dashboard design and KPI selection in Power BI
- Translating data into business recommendations and clear written communication
