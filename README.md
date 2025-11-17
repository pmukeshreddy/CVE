# CVE Lakehouse on Databricks - 2024 Vulnerability Intelligence Platform

**Data Intensive Computing (DIC 587) - Fall 2025**

A production-grade data engineering pipeline implementing the Medallion Architecture to process and analyze 32,924+ cybersecurity vulnerabilities from 2024.

## 🎯 Project Overview

Built a comprehensive vulnerability intelligence platform using Databricks Community Edition, processing real-world CVE (Common Vulnerabilities and Exposures) data through Bronze → Silver → Gold layers to deliver actionable security insights.

## 📊 Key Results

- **32,924 CVE records** processed from 2024 (record-breaking year)
- **6,683 vulnerabilities** with CVSS severity scores
- **Average CVSS score**: 7.02 (High severity)
- **810 Critical vulnerabilities** (9.0+ CVSS score)
- **3,644+ affected vendors** tracked across products

## 🏗️ Architecture

### Medallion Architecture Implementation
```
Bronze Layer (Raw)          Silver Layer (Normalized)       Gold Layer (Analytics)
├── cve_bronze.records  →   ├── cve_silver_core         →   ├── Temporal Analysis
│   - Raw JSON              │   - One row per CVE           │   - Risk Distribution  
│   - 32,924 records        ├── cve_silver_affected_products│   - Vendor Intelligence
│   - 2024 filtered         │   - Exploded vendor/product   └── - Market Concentration
└── - Full metadata         └── - Foreign key relationships
```

## 🛠️ Tech Stack

- **Platform**: Databricks Community Edition (DBR 13.x+)
- **Storage**: Delta Lake with ACID transactions
- **Processing**: Apache Spark (PySpark + SparkSQL)
- **Data Source**: CVEProject/cvelistV5 GitHub repository
- **Language**: Python 3.x, SQL

## 🔑 Key Features

### Bronze Layer
- Downloaded and ingested CVE v5 JSON repository
- Filtered to 2024 data by `cveMetadata.datePublished`
- Created Delta table with column mapping
- Implemented data quality assertions (count, nulls, uniqueness)

### Silver Layer
- **Core CVE Table**: Normalized vulnerability records with CVSS scores, dates, descriptions
- **Affected Products Table**: Exploded vendor/product relationships using `explode_outer()`
- Standardized timestamps and handled null values
- Used `saveAsTable()` for proper catalog registration

### SQL Analytics
- **Temporal Analysis**: Monthly vulnerability trends, publication latency
- **Risk Distribution**: CVSS severity bucketing (Critical/High/Medium/Low)
- **Vendor Intelligence**: Top 25 vendors, market concentration, vendor-specific risk profiles

## 📈 Notable Insights

### Top Vulnerability Vendors (2024)
1. **n/a** (unattributed): 3,644 CVEs, 7.41 avg CVSS
2. **Google**: 411 CVEs, 7.38 avg CVSS
3. **Apple**: 369 CVEs, 6.47 avg CVSS
4. **Mozilla**: 156 CVEs, 7.44 avg CVSS

### Highest Risk Vendors (≥10 CVEs)
1. **NEC Corporation**: 7.81 avg CVSS, 295 critical
2. **HP Inc.**: 7.65 avg CVSS
3. **Mozilla**: 7.44 avg CVSS, 94 critical

### Severity Distribution
- **Critical** (≥9.0): 12.1% of scored CVEs
- **High** (7.0-8.9): 54.8%
- **Medium/Low**: 33.1%

## 🚀 Setup Instructions

### Prerequisites
1. Databricks Community Edition account: https://community.cloud.databricks.com
2. Create Unity Catalog volume: `/Volumes/workspace/default/assignment1`
3. Runtime: DBR 13.x or newer

### Running the Pipeline

**Cell 1: Download CVE Repository**
```python
# Downloads CVEProject/cvelistV5 from GitHub
# Extracts to /tmp/cve/cvelistV5-main/cves
```

**Cell 2: Load Raw JSON**
```python
# Reads 2024 JSON files using os.walk()
# Creates Spark DataFrame with schema inference
```

**Cell 3: Bronze Layer**
```python
# Filters to 2024 by datePublished
# Creates cve_bronze.records Delta table
# Runs data quality assertions
```

**Cell 4: Silver Layer**
```python
# Creates cve_silver_core (normalized CVE records)
# Creates cve_silver_affected_products (exploded relationships)
# Uses saveAsTable() for catalog registration
```

**Cell 5: SQL Analytics**
```python
# Executes 8 analytical queries
# Temporal, risk, and vendor intelligence analysis
```

## 📁 Project Structure
```
├── Untitled_Notebook_2025-11-16.ipynb    # Complete pipeline implementation
├── README.md                              # This file
└── screenshots/                           # Execution results (optional)
    ├── bronze_layer_output.png
    ├── silver_layer_tables.png
    └── sql_analysis_results.png
```

## 🎓 Key Learnings

### Technical Skills
- **Medallion Architecture**: Implemented Bronze→Silver→Gold pattern with progressive data refinement
- **Large-Scale JSON Processing**: Normalized 32,924 nested JSON records to relational schema
- **Delta Lake Operations**: ACID transactions, schema evolution, table versioning
- **Data Normalization**: Used `explode_outer()` to flatten one-to-many relationships
- **Data Quality**: Implemented assertion-based validation (count thresholds, uniqueness, nulls)

### Big Data Concepts
- Schema-on-read for semi-structured data
- Efficient filtering (filter early, not after loading all data)
- Unity Catalog volume operations with `dbutils.fs`
- Reproducible pipeline design for Community Edition limitations

### Cybersecurity Domain
- CVE data structure and CVSS scoring system
- Vulnerability disclosure patterns and market concentration
- Real-world threat intelligence analysis

## 📊 Performance Metrics

- **Bronze Table**: 8 Delta files, 10.1 MB compressed
- **Data Quality**: 100% unique CVE IDs, 0 null values
- **Silver Explosion**: ~40K core records → ~45K+ vendor/product relationships



## 📝 Resume Bullet Points

- Built Bronze and Silver data layers using Delta Lake on Databricks implementing Medallion Architecture
- Normalized 32,924 CVE v5 vulnerability records from nested JSON to relational schema using PySpark
- Implemented production-ready data pipeline with ACID transactions, schema evolution, and quality assertions
- Performed comprehensive exploratory analysis on cybersecurity vulnerability trends using SparkSQL
- Designed reproducible ETL pipeline with error handling for distributed computing environment

