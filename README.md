# 🛍️ Customer Behavior Analysis — End-to-End Data Project

An end-to-end data analysis project covering **data cleaning in Python**, **SQL querying in SQL Server**, and an **interactive Power BI dashboard** to uncover customer shopping patterns and behavior insights.

---

## 📌 Project Overview

This project analyzes the shopping behavior of **4,000 customers** across product categories, age groups, gender, and subscription status. The goal is to identify trends in purchases, revenue distribution, and customer segments to support data-driven business decisions.

---

## 🔄 Project Workflow

```
Raw CSV Data
     │
     ▼
Python & Pandas (Jupyter Notebook)
  • Load & explore data
  • Handle missing values
  • Rename & standardize columns
  • Feature engineering (age_group, purchase_frequency_days)
  • Drop redundant columns
  • Export cleaned data to SQL Server
     │
     ▼
SQL Server (T-SQL Queries)
  • Revenue by gender & age group
  • Subscriber vs non-subscriber spend analysis
  • Top products by rating & discount rate
  • Customer segmentation (New / Returning / Loyal)
  • Window functions & CTEs
     │
     ▼
Power BI Dashboard
  • KPI Cards, Bar Charts, Donut Chart
  • Interactive slicers & filters
```

---

## 🛠️ Tools & Technologies

Python
Jupyter Notebook
Power BI
SQL


---

## 📂 Project Structure

```
customer-behavior-analysis/
│
├── data/
│   └── customer_shopping_behavior.csv       # Original raw dataset
│
├── notebooks/
│   └── customer_shopping_behaviour.ipynb    # Data cleaning & feature engineering
│
├── sql/
│   └── SQLQuery1_customer_shopping_behaviour.sql  # 10 business SQL queries
│
├── dashboard/
│   ├── customer_behaviour_powerbi_dashboard.pbix  # Power BI file
│   └── dashboard_preview.png                      # Dashboard screenshot
│
└── README.md
```

---

**Dashboard Features:**
- KPI Cards — Total Customers (4K), Avg. Purchase Amount ($59.76), Avg. Review Rating (3.75)
- Sales & Revenue by Category (Clothing, Accessories, Footwear, Outerwear)
- Subscriber % Donut Chart (73% Non-Subscribers vs 27% Subscribers)
- Revenue & Sales by Age Group (horizontal bar charts)
- Interactive Slicers — Subscription Status, Gender, Category, Shipping Type

---

## 📋 Dataset Description

| Column | Description |
|--------|-------------|
| `customer_id` | Unique identifier for each customer |
| `age` | Customer age (used to derive age group) |
| `age_group` | Derived: Young Adult / Adult / Middle-Aged / Senior |
| `gender` | Male / Female |
| `category` | Clothing, Accessories, Footwear, Outerwear |
| `item_purchased` | Specific product bought |
| `purchase_amount` | Transaction value in USD |
| `review_rating` | Customer rating out of 5 |
| `subscription_status` | Yes / No |
| `shipping_type` | Standard, Express, 2-Day, Next Day Air, Free, Store Pickup |
| `discount_applied` | Whether a discount was used (Yes/No) |
| `previous_purchases` | Count of past purchases (used for segmentation) |
| `frequency_of_purchases` | Purchase frequency label (Weekly, Monthly, etc.) |
| `purchase_frequency_days` | Derived: numeric equivalent of frequency in days |

---

## 🧹 Step 1 — Data Cleaning (Python & Pandas)

**File:** `notebooks/customer_shopping_behaviour.ipynb`

### Key Steps Performed:

**1. Load & Explore**
```python
import pandas as pd
df = pd.read_csv('customer_shopping_behavior.csv')
df.head()
df.info()
df.describe(include='all')
```

**2. Handle Missing Values**
- Detected nulls in `Review Rating`
- Imputed using **median rating per category** (category-aware imputation)
```python
df['Review Rating'] = df.groupby('Category')['Review Rating']\
    .transform(lambda x: x.fillna(x.median()))
```

**3. Column Standardization**
- Converted all column names to `snake_case`
- Renamed `purchase_amount_(usd)` → `purchase_amount`

**4. Feature Engineering**
- Created `age_group` using `pd.qcut()` → Young Adult / Adult / Middle-Aged / Senior
- Created `purchase_frequency_days` by mapping text frequency to numeric days

**5. Drop Redundant Columns**
- `promo_code_used` was confirmed to be identical to `discount_applied` → dropped

**6. Export to SQL Server**
- Used `SQLAlchemy` + `pyodbc` with Windows Authentication to push cleaned DataFrame to Microsoft SQL Server

---

## 🗄️ Step 2 — SQL Querying (Microsoft SQL Server / T-SQL)

**File:** `sql/SQLQuery1_customer_shopping_behaviour.sql`

10 business questions answered using SQL:

| # | Question | Concepts Used |
|---|----------|---------------|
| Q1 | Revenue by gender | `GROUP BY`, `SUM` |
| Q2 | Discount users spending above average | Subquery, `WHERE` filter |
| Q3 | Top 5 products by avg review rating | `TOP`, `ROUND`, `CAST`, `ORDER BY` |
| Q4 | Avg purchase: Standard vs Express shipping | `WHERE IN`, `GROUP BY` |
| Q5 | Do subscribers spend more? | `COUNT`, `AVG`, `SUM`, `GROUP BY` |
| Q6 | Top 5 products with highest discount rate | `CASE WHEN`, percentage calculation |
| Q7 | Customer segmentation: New / Returning / Loyal | `CTE`, `CASE WHEN` |
| Q8 | Top 3 products per category | `CTE`, `ROW_NUMBER()`, `PARTITION BY` |
| Q9 | Repeat buyers vs subscription status | `WHERE`, `GROUP BY` |
| Q10 | Revenue contribution by age group | `GROUP BY`, `ORDER BY` |

---

## 💡 Key Insights

- 🏆 **Clothing** leads in both sales volume and revenue across all categories
- 👨‍💼 **Young Adults** contribute the highest total revenue among all age groups
- 🔔 **73% of customers are non-subscribers** — a major untapped growth opportunity
- 💳 Customers who used discounts still spent at or above the average purchase amount
- 🔁 Repeat buyers (5+ purchases) show higher subscription rates — loyalty drives subscriptions
- ⭐ Average review rating of **3.75/5** — room for product/service improvement

---

## 🚀 How to Run This Project

### Step 1 — Jupyter Notebook
```bash
pip install pandas numpy jupyter sqlalchemy pyodbc
jupyter notebook notebooks/customer_shopping_behaviour.ipynb
```

### Step 2 — SQL Server
- Import `customer_shopping_behavior.csv` into Microsoft SQL Server
- Or run the notebook to auto-push cleaned data via SQLAlchemy
- Then execute queries from `sql/SQLQuery1_customer_shopping_behaviour.sql`

### Step 3 — Power BI Dashboard
- Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
- Open `dashboard/customer_behaviour_powerbi_dashboard.pbix`
- Refresh data source if needed

