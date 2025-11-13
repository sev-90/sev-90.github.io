---
title: "CoffeeCo Sales Analytics — AWS Data Engineering Mini-Project"
excerpt: ""
collection: portfolio
abstract: "Build a small but realistic batch analytics pipeline on AWS, ending with a Redshift-backed dashboard 
(QuickSight and Quick Suite) and a clean star schema."

---

# CoffeeCo Sales Analytics — AWS Data Engineering Mini‑Project

> **Batch pipeline, Redshift analytics, and QuickSight dashboard**

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

# 📦 Data Model

- **Grain:** `order line`
- **Columns (curated):**  
  `order_id, event_ts, customer_id, state, product_name, unit_price, quantity, order_total, status, order_date (partition)`
- **Partitions:** `order_date` (daily)

---

# What I Implemented

## 1) Land & Curate

- Generated fake **daily orders (CSV)** → `s3://.../bronze/`
- Created **curated Parquet** with strict types and **daily partitions** (`order_date`) → `s3://.../curated/`

```bash
# Example S3 layout
s3://mybucket/
├── bronze/
│   └── orders/ingest_date=2025-11-10/orders_2025-11-10.csv
└── curated/
    └── orders/order_date=2025-11-10/part-0000.parquet

