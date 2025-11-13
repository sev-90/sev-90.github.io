---
title: "CoffeeCo Sales Analytics — AWS Data Engineering Mini-Project"
excerpt: ""
collection: portfolio
abstract: "This project showcases an end-to-end AWS analytics pipeline using synthetically (fake) generated e-commerce orders. Raw CSVs land in Amazon S3 (bronze), are standardized into partitioned Parquet via Amazon Athena with metadata in AWS Glue (curated), then modeled in Amazon Redshift Serverless—validated through Spectrum and materialized into internal tables with a simple star schema and materialized views for fast reads. A QuickSight (SPICE) dashboard surfaces KPIs, trends, and cancellation insights.

The stack utilized: S3, Athena, Glue, Redshift Serverless, QuickSight, Quick Suite."

---

# CoffeeCo Sales Analytics — AWS Data Engineering Mini‑Project

> **Batch pipeline, Redshift analytics, and QuickSight dashboard**

<p align="center">
<img src="/images/Coffeeco_QuickSight_dashboard.png" width="1000" height="1000" />
</p>
---

## Goal

Build a small but realistic **batch analytics pipeline** on AWS, ending with a **Redshift‑backed dashboard** (QuickSight/ Quick Suite) and a clean star schema.

---
# Tech Stack

- **Storage:** Amazon S3 (_bronze_ + _curated_)
- **Metadata / Query:** AWS Glue Data Catalog, Amazon Athena (CTAS / INSERT)
- **Warehouse:** Amazon Redshift **Serverless** (Spectrum + internal tables, Materialized Views)
- **Visualization:** Amazon QuickSight (SPICE); Business insights from Quick Suite
- **Languages:** SQL (Athena / Redshift), a bit of Python / Spark
- **IAM & Networking:** IAM roles for Spectrum/COPY, public endpoint

---

# Data Model

- **Grain:** `order line`
- **Columns (curated):**  
  `order_id, event_ts, customer_id, state, product_name, unit_price, quantity, order_total, status, order_date (partition)`
- **Partitions:** `order_date` (daily)

---

# What I Implemented

## 1) Land & Curate

- Generated fake **daily orders (CSV)** → `s3://.../bronze/`
- Created **curated Parquet** with strict types and **daily partitions** (`order_date`) → `s3://.../curated/`

### Example S3 layout
s3://mybucket/
├── bronze/
│   └── orders/ingest_date=2025-11-10/orders_2025-11-10.csv
└── curated/
    └── orders/order_date=2025-11-10/part-0000.parquet

## 2) Wire Redshift (read S3 first, then load)

- Created Redshift external schema to Glue DB (Spectrum).

- Sanity checked counts & partitions from S3.

- Built materialized view for daily sales.

## 3) Create internal warehouse tables (fast analytics)

## 4) Star schema + repeatable daily rebuild

- Dimensions + fact
- Transactional daily refresh (idempotent pattern).

## 5) BI Surface & Dashboard (QuickSight)
- QuickSight:
-- Import from Redshift or Athena
-- Use SPICE for speed
-- Visuals
-- Add an Quick Suite narrative for business insights 
