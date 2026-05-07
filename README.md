# ETL Pipeline using Databricks, Delta Lake & AWS S3

## Project Overview

This project implements an end-to-end ETL Lakehouse pipeline using Databricks, Delta Lake, and AWS S3 following the Bronze-Silver-Gold Medallion Architecture.

The pipeline processes retail sales data consisting of:

* Customers
* Products
* Stores
* Sales Transactions

The project also includes:

* Incremental Load Processing
* SCD Type 2 Implementation
* CDC Demonstration
* ETL Validation Testing
* Archival Management
* Workflow Automation using Databricks Jobs

---

# Architecture

```text
AWS S3 Source Files
        ↓
Bronze Layer (Raw Delta Tables)
        ↓
Silver Layer (Cleaned & Deduplicated Data)
        ↓
Gold Layer (Star Schema Warehouse)
        ↓
Incremental MERGE Processing
        ↓
SCD Type 2 Historical Tracking
        ↓
Validation Testing
        ↓
Archival Process & Validation
```

---

# Technologies Used

* Databricks
* Delta Lake
* AWS S3
* SQL
* Python
* Databricks Workflows

---

# Medallion Architecture

## Bronze Layer

Purpose:

* Raw data ingestion from AWS S3
* Store source CSV data as Delta tables

Tables:

* bronze.customers
* bronze.products
* bronze.stores
* bronze.sales

---

## Silver Layer

Purpose:

* Data cleaning
* Standardization
* Deduplication
* Data quality validation

Transformations:

* TRIM
* LOWER
* INITCAP
* CAST
* TO_DATE
* ROW_NUMBER based deduplication

Business Key Deduplication:

* CustomerID
* ProductID
* StoreID
* TransactionID

---

## Gold Layer

Purpose:

* Create dimensional warehouse model
* Build analytical reporting tables

Tables:

* dim_customer (SCD Type 2)
* dim_product
* dim_store
* fact_sales

Features:

* Surrogate keys
* Fact-Dimension relationships
* Derived Amount calculation

---

# Incremental Load

Incremental processing was implemented using Delta MERGE.

Features:

* Insert new records
* Update existing records
* Prevent duplicate processing
* Rerunnable workflow execution

---

# SCD Type 2

Implemented on:

* dim_customer

Features:

* Historical tracking
* Active/Inactive records
* StartDate and EndDate management

---

# CDC Demonstration

Delta Change Data Feed (CDF) was implemented to demonstrate:

* Insert tracking
* Update tracking
* Delete tracking

CDC was validated separately using:

```sql
table_changes()
```

---

# ETL Validation Testing

Validation testing includes:

## Source-to-Target Testing

* Row count validation
* Column mapping validation
* Data type validation

## Data Transformation Testing

* Proper case validation
* Lowercase email validation
* Amount calculation validation

## Data Quality Testing

* Duplicate checks
* Null checks
* Invalid data rejection
* Referential integrity validation

## Incremental Load Testing

* Incremental inserts
* Incremental updates
* Duplicate prevention

## SCD Type 2 Testing

* Active vs inactive records
* Historical record validation
* EndDate validation

---

# Archival Process

Implemented automated archival logic for source files.

Features:

* Retain latest timestamped file
* Archive previous files
* Validate naming conventions
* Multi-zone archival validation

File Naming Convention:

```text
<filename>_DDMMYYYYHHMMSS.csv
```

---

# Workflow Automation

Databricks Workflows were used to orchestrate the pipeline.

Pipeline Flow:

```text
01_initial_setup
      ↓
02_bronze_layer
      ↓
03_silver_layer
      ↓
04_gold_layer
      ↓
05_incremental_load
      ↓
06_scd2_processing
      ↓
07_validation_testing
      ↓
08_archival_process
      ↓
09_archival_validation
```

---

# Project Structure

```text
ETL-pipeline-databricks/
│
├── notebooks/
│   ├── 01_initial_setup.sql
│   ├── 02_bronze_layer.sql
│   ├── 03_silver_layer.sql
│   ├── 04_gold_layer.sql
│   ├── 05_incremental_load.sql
│   ├── 06_scd2_processing.sql
│   ├── 07_validation_testing.sql
│   ├── 08_archival_process.py
│   ├── 09_archival_validation.py
│   └── 10_cdc_demo.sql
│
├── datasets/
│
├── screenshots/
│
└── README.md
```

---

# Key Features

* End-to-end ETL Pipeline
* Delta Lakehouse Architecture
* Incremental MERGE Processing
* SCD Type 2
* CDC Demonstration
* Data Quality Validation
* Referential Integrity Checks
* Deduplication Logic
* Archival Framework
* Workflow Automation

---

# Future Enhancements

* Audit Logging Framework
* Pipeline Monitoring Dashboard
* Data Quality Scorecards
* Rejection Tables
* Alerting & Notifications
* Performance Optimization
* CI/CD Integration

---

# Conclusion

This project demonstrates a production-style ETL Lakehouse pipeline using Databricks and Delta Lake with incremental processing, historical tracking, validation testing, and workflow orchestration.
