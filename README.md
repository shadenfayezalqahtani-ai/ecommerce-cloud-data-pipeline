Markdown# Enterprise ERP Integration: Cloud-Native Data Pipeline & Analytics Engine

## Project Overview
This project serves as an end-to-end middleware data integration pipeline. It extracts raw transactional, product, and inventory records from a live e-commerce ERP system API, transforms and cleans the unstructured payloads using Python, architectures a normalized Relational Data Warehouse (Star Schema), and executes analytical window functions to rank business performance.

## System Architecture
* **Data Extraction:** Middleware ingestion via Python `requests` connecting to live REST API endpoints.
* **Data Processing:** Automated schema formatting, currency rounding, missing variable handling, and ledger ingestion timestamps via `pandas`.
* **Data Warehouse Layer:** Optimized SQL Star Schema partitioning data into relational Fact and Dimension tables for maximum query efficiency.
* **Analytics Layer:** Complex multi-table `JOIN` aggregations and analytical window functions (`RANK() OVER`).

## Core Code Artifacts

### 1. Ingestion Engine (Python Extract-Transform-Load)
```python
# Automated API extraction and transformation layer
import requests
import pandas as pd

url = "https://fakestoreapi.com/products"
response = requests.get(url)
if response.status_code == 200:
    df = pd.DataFrame(response.json())
    df['price'] = df['price'].astype(float).round(2)
2. Data Warehouse Schema (SQL DDL)SQLCREATE TABLE dim_products (
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
3. Advanced Enterprise Query & Calculated Business MetricsSQLSELECT 
    c.category_name AS [Product Category],
    COUNT(f.transaction_id) AS [Total Orders],
    SUM(f.units_sold) AS [Total Volume Sold],
    ROUND(SUM(f.total_revenue), 2) AS [Gross Revenue],
    RANK() OVER (ORDER BY SUM(f.total_revenue) DESC) AS [Revenue Performance Rank]
FROM fact_sales f
JOIN dim_products p ON f.product_id = p.product_id
JOIN dim_categories c ON f.category_id = c.category_id
GROUP BY c.category_name;
4. Output SummaryProduct CategoryTotal OrdersTotal Volume SoldGross RevenueRevenue Performance Rankmen's clothing420$744.781jewelry11$695.002
