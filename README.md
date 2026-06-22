# API-Driven Sales Analytics Platform & Relational Data Warehouse

## Project Overview
This project implements a lightweight ETL (Extract-Transform-Load) data pipeline designed to ingest mock retail data via a REST API, structure it into a relational model, and run business intelligence queries. It extracts transactional, product, and inventory records from a live e-commerce API, transforms and cleans the unstructured payloads using Python, architectures a normalized Relational Data Warehouse (Star Schema), and executes analytical window functions to rank business performance.

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

url = "[https://fakestoreapi.com/products](https://fakestoreapi.com/products)"
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
3. Advanced Query & Calculated Business MetricsSQLSELECT 
    c.category_name AS [Product Category],
    COUNT(f.transaction_id) AS [Total Orders],
    SUM(f.units_sold) AS [Total Volume Sold],
    ROUND(SUM(f.total_revenue), 2) AS [Gross Revenue],
    RANK() OVER (ORDER BY SUM(f.total_revenue) DESC) AS [Revenue Performance Rank]
FROM fact_sales f
JOIN dim_products p ON f.product_id = p.product_id
JOIN dim_categories c ON f.category_id = c.category_id
GROUP BY c.category_name;
4. Output SummaryProduct CategoryTotal OrdersTotal Volume SoldGross RevenueRevenue Performance Rankmen's clothing420$744.781jewelry11$695.002📊 Business Intelligence Layer (Power BI Semantic Blueprint)To close the loop between data engineering and executive decision-making, the normalized Star Schema is mapped directly to a business intelligence semantic layer using a classic Star Schema configuration (One-to-Many 1:N unidirectional filtering).1. Core DAX Measures (Calculated Metrics)To ensure dynamic, high-performance calculations across executive dashboard filter contexts, the following enterprise DAX expressions were modeled:Total Revenue Invoiced:Code snippetTotal Revenue = SUM(fact_sales[total_revenue])
Average Order Value (AOV):Code snippetAverage Order Value = DIVIDE([Total Revenue], COUNT(fact_sales[transaction_id]), 0)
Total Units Distributed:Code snippetUnits Sold = SUM(fact_sales[units_sold])
2. Executive Dashboard Interface Design BlueprintThe UI/UX matrix is architected to prioritize high-level operational performance and systemic data health for corporate leadership teams:High-Impact KPIs (Top Ribbon Cards):Gross Revenue Generated: Dynamically reflecting total transactional values ($1,439.78 across current ingestion batches).Average Basket Value: Tracking customer purchasing strength.Total Order Volume: Real-time throughput monitoring of completed sales transactions.Core Visualizations:Category Performance Matrix (Bar Chart): Plots dim_categories[category_name] against [Total Revenue] to instantly pinpoint high-value verticals vs. lagging product categories.Data Ingestion Audit Trail (Grid View): Displays fact_sales[ingestion_timestamp] to allow system administrators to audit exact system processing times and ensure pipeline latency remains low.🛠️ Future Production Scalability & Enterprise RoadblocksBecause this platform was built as an architectural prototype using a lightweight public mock API (fakestoreapi.com), scaling this system to a Tier-1 production environment (such as an SAP ERP or Oracle Data Cloud at an enterprise level) would require addressing the following system constraints:Enterprise Authentication: Transitioning from an open API endpoint to secure OAuth 2.0 protocols or Mutual TLS (mTLS), managing access keys securely through cloud key vaults rather than clear-text configuration parameters.Idempotency & Data Lineage: Implementing unique transaction IDs and logging checkpoints to guarantee that if the pipeline network crashes midway and restarts, it does not write duplicate duplicate transactions into the data warehouse layer.Robust Error Handling: Replacing basic status check constraints with comprehensive conditional validation blocks, data type exception catching, and automated webhook alerts routed to IT operations management tools.
