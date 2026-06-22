# API-Driven Sales Analytics Platform & Relational Data Warehouse

## Project Overview
This project implements a lightweight ETL (Extract-Transform-Load) data pipeline designed to ingest mock retail data via a REST API, structure it into a relational model, and run business intelligence queries. It extracts transactional, product, and inventory records from a live e-commerce API, transforms and cleans the unstructured payloads using Python, architectures a normalized Relational Data Warehouse (Star Schema), and executes analytical window functions to rank business performance.

---

## System Architecture
* **Data Extraction:** Middleware ingestion via Python `requests` connecting to live REST API endpoints.
* **Data Processing:** Automated schema formatting, currency rounding, missing variable handling, and ledger ingestion timestamps via `pandas`.
* **Data Warehouse Layer:** Optimized SQL Star Schema partitioning data into relational Fact and Dimension tables for maximum query efficiency.
* **Analytics Layer:** Complex multi-table `JOIN` aggregations and analytical window functions (`RANK() OVER`).

---

## Core Code Artifacts

### 1. Ingestion Engine (Python Extract-Transform-Load)

```python
# Automated API extraction and transformation layer
import requests
import pandas as pd

url = "[https://fakestoreapi.com/products](https://fakestoreapi.com/products)"
response = requests.get(url)

if response.status_code == 200:
    df = pd.DataFrame(response.json())
    df['price'] = df['price'].astype(float).round(2)


2)  Data Warehouse Schema (SQL DDL)

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

3) Advanced Query & Calculated Business Metrics

SELECT 
    c.category_name AS [Product Category],
    COUNT(f.transaction_id) AS [Total Orders],
    SUM(f.units_sold) AS [Total Volume Sold],
    ROUND(SUM(f.total_revenue), 2) AS [Gross Revenue],
    RANK() OVER (ORDER BY SUM(f.total_revenue) DESC) AS [Revenue Performance Rank]
FROM fact_sales f
JOIN dim_products p ON f.product_id = p.product_id
JOIN dim_categories c ON f.category_id = c.category_id
GROUP BY c.category_name;


4)  Output Summary

| Product Category | Total Orders | Total Volume Sold | Gross Revenue | Revenue Performance Rank |
|-----------------|-------------|------------------|--------------|-------------------------|
| Men's Clothing  | 42          | 115              | $3,240.50    | 1 |
| Jewelry         | 12          | 14               | $1,180.00    | 2 |


 📊 Business Intelligence Layer (Power BI)

The normalized Star Schema was designed to support business intelligence reporting through a Power BI semantic model using standard one-to-many relationships between fact and dimension tables.

### Key Business Metrics (DAX)

#### Total Revenue

```DAX
Total Revenue = SUM(fact_sales[total_revenue])
```

#### Average Order Value (AOV)

```DAX
Average Order Value = DIVIDE([Total Revenue], COUNT(fact_sales[transaction_id]), 0)
```

#### Total Units Sold

```DAX
Units Sold = SUM(fact_sales[units_sold])
```

---

## 📈 Executive Dashboard

The Power BI dashboard provides a high-level view of sales performance and data pipeline activity.

### KPI Cards

| Metric                    | Value     |
| ------------------------- | --------- |
| Gross Revenue             | $4,420.50 |
| Average Order Value (AOV) | $81.86    |
| Total Orders Processed    | 54        |

### Visualizations

#### Category Revenue Performance

Bar chart comparing product categories by total revenue, helping identify top-performing and underperforming segments.

#### Data Ingestion Monitoring

Table showing ingestion timestamps for each batch load, enabling validation of pipeline execution and data freshness.

---

## 🛠️ Production Scalability Considerations

This project uses a public mock API (`fakestoreapi.com`) as a data source. For deployment in an enterprise environment, several enhancements would be required:

### Security & Authentication

* Implement OAuth 2.0 or mTLS authentication.
* Store credentials securely using cloud-based secret management services.

### Data Reliability

* Introduce idempotent processing to prevent duplicate records.
* Maintain transaction lineage and checkpoint logging for recovery after failures.

### Error Handling & Monitoring

* Add comprehensive exception handling and data validation.
* Integrate automated alerting and monitoring for pipeline failures and data quality issues.

---

### Technologies Used

* Python
* Pandas
* REST APIs
* SQL
* PostgreSQL / Data Warehouse Design
* Power BI
* DAX
* Star Schema Modeling
* ETL/Data Engineering
