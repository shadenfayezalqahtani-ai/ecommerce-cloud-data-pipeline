# API-Driven Sales Analytics Platform & Relational Data Warehouse

## Project Overview

This project demonstrates an end-to-end analytics workflow that extracts product data from a REST API, transforms it using Python, stores it in a relational star schema, and analyzes business performance using SQL and Power BI.

The objective was to simulate a simplified retail analytics pipeline by combining data engineering fundamentals with business intelligence reporting. The project covers API integration, data transformation, dimensional modeling, SQL analytics, and interactive dashboard development.

---

# Architecture

The project follows a simple ETL (Extract, Transform, Load) workflow.

### 1. Data Extraction

Product data is retrieved from the Fake Store REST API using Python's `requests` library.

### 2. Data Transformation

The extracted JSON data is converted into tabular format using Pandas. Basic cleaning and preparation include:

* Converting data types
* Standardizing numeric values
* Handling missing values where appropriate
* Recording ingestion timestamps

### 3. Data Warehouse

The processed data is organized into a relational star schema consisting of fact and dimension tables.

The schema separates transactional data from descriptive attributes to support analytical queries and reporting.

### 4. Analytics

SQL aggregation, joins, and window functions are used to calculate business metrics such as:

* Revenue by product category
* Order volume
* Units sold
* Category performance ranking

### 5. Business Intelligence

The warehouse is connected to Power BI to create an executive dashboard that summarizes sales performance using KPI cards and interactive visualizations.

---

# Technologies Used

* Python
* Pandas
* REST API
* SQL
* PostgreSQL
* Power BI
* DAX
* Star Schema Data Modeling

---

# Data Extraction (Python)

```python
import requests
import pandas as pd

url = "https://fakestoreapi.com/products"

response = requests.get(url)

if response.status_code == 200:
    df = pd.DataFrame(response.json())
    df["price"] = df["price"].astype(float).round(2)
```

---

# Data Warehouse Schema

```sql
CREATE TABLE dim_products (
    product_id INTEGER PRIMARY KEY,
    product_title TEXT NOT NULL,
    base_price REAL NOT NULL
);

CREATE TABLE fact_sales (
    transaction_id INTEGER PRIMARY KEY AUTOINCREMENT,
    product_id INTEGER,
    category_id INTEGER,
    units_sold INTEGER,
    total_revenue REAL,
    FOREIGN KEY (product_id) REFERENCES dim_products(product_id)
);
```

The warehouse follows a star schema design, where dimension tables store descriptive information and the fact table stores measurable business events.

---

# SQL Analytics

Business metrics are calculated using SQL joins, aggregation functions, and window functions.

```sql
SELECT
    c.category_name AS [Product Category],
    COUNT(f.transaction_id) AS [Total Orders],
    SUM(f.units_sold) AS [Total Volume Sold],
    ROUND(SUM(f.total_revenue), 2) AS [Gross Revenue],
    RANK() OVER (
        ORDER BY SUM(f.total_revenue) DESC
    ) AS [Revenue Performance Rank]
FROM fact_sales f
JOIN dim_products p
    ON f.product_id = p.product_id
JOIN dim_categories c
    ON f.category_id = c.category_id
GROUP BY c.category_name;
```

Example output:

| Product Category | Total Orders | Total Volume Sold | Gross Revenue | Revenue Rank |
| ---------------- | ------------ | ----------------- | ------------- | ------------ |
| Men's Clothing   | 42           | 115               | $3,240.50     | 1            |
| Jewelry          | 12           | 14                | $1,180.00     | 2            |

---

# Power BI Dashboard

The Power BI report connects directly to the star schema and presents key business metrics through interactive visuals.

### KPIs

| Metric              |     Value |
| ------------------- | --------: |
| Gross Revenue       | $4,420.50 |
| Average Order Value |    $81.86 |
| Total Orders        |        54 |

### Dashboard Features

* Revenue by product category
* KPI summary cards
* Sales ranking by category
* Data ingestion timestamp monitoring

---

# Project Structure

```
project/
│
├── data/
├── sql/
├── notebooks/
├── powerbi/
├── screenshots/
├── README.md
└── requirements.txt
```

---

# Future Improvements

This project is intentionally lightweight and designed for learning purposes. Possible enhancements include:

* Incremental data loading
* Scheduled ETL execution
* Logging and exception handling
* Automated data quality validation
* Docker containerization
* Cloud deployment
* Authentication for secured APIs

---

# Skills Demonstrated

* REST API integration
* Python data processing
* Data cleaning with Pandas
* Relational database design
* Star schema modeling
* SQL analytics
* Window functions
* Business intelligence reporting
* Power BI dashboard development
* DAX measures
