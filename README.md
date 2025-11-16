# CVE Lakehouse on Databricks - 2024 Vulnerability Intelligence Platform

## 📋 Project Overview

This project implements a comprehensive cybersecurity data lakehouse using the **Medallion Architecture** pattern on Databricks Community Edition. The pipeline ingests real-world Common Vulnerabilities and Exposures (CVE) data from 2024 and transforms it into actionable security intelligence through progressive data refinement.

**Course**: DIC 587 - Data Intensive Computing, Fall 2025  
**Assignment**: Assignment 1 - CVE Lakehouse  
**Due Date**: November 16, 2025

## 🏗️ Architecture

The implementation follows the industry-standard **Bronze → Silver → Gold** pattern:

### Bronze Layer (Raw Data Ingestion)
- Downloads CVE v5 repository from GitHub (CVEProject/cvelistV5)
- Filters to 2024 data: **38,753 vulnerability records** (record-breaking year!)
- Loads JSON files into Spark DataFrame using Pandas bridge
- Validates data quality: **32,924 valid 2024 CVE records**
- Registers as temp table: `cve_bronze_records`

### Silver Layer (Data Normalization)
- **Core CVE Table**: Normalized vulnerability metadata
  - CVE ID, publication dates, CVSS scores, severity levels
  - One record per vulnerability for efficient querying
- **Affected Products Table**: Exploded vendor/product relationships
  - Uses `explode_outer` to flatten nested vendor/product arrays
  - Enables queries like "Show all Linux vulnerabilities"
  - Foreign key relationship maintained via CVE ID

### Gold Layer (Analytics & Insights)
- **Temporal Analysis**: Monthly trends, publication latency
- **Risk Distribution**: CVSS severity bucketing (Critical/High/Medium/Low)
- **Vendor Intelligence**: Top 25 vendors, market concentration, risk profiles

## 📁 Repository Structure

```
CVE-Lakehouse-Databricks/
│
├── README.md                                          # This file
├── CVE_Lakehouse_Complete.ipynb                      # Complete pipeline implementation
└── screenshots/                                       # (Add your screenshots here)
    ├── bronze_layer_output.png
    ├── silver_layer_schema.png
    └── sql_analysis_results.png
```

## 🚀 Quick Start

### Prerequisites
- Databricks Community Edition account ([Sign up here](https://community.cloud.databricks.com))
- Runtime: DBR 13.x or newer (for Delta Lake compatibility)
- Basic familiarity with Python and SQL

### Setup Instructions

1. **Create Databricks Account**
   ```
   Visit: https://community.cloud.databricks.com
   Sign up with Google account (2FA required)
   ```

2. **Import Notebook**
   - Upload `CVE_Lakehouse_Complete.ipynb` to your Databricks workspace
   - Create a new cluster (Community Edition provides free compute)

3. **Run Pipeline**
   - Execute cells sequentially (1 → 2 → 3 → 4)
   - **Estimated runtime**: ~5 minutes
   - Pipeline is fully automated - no manual intervention needed

4. **Cell Execution Order**
   - **Cell 1**: Download CVE repository (~500MB ZIP file)
   - **Cell 2**: Load and parse 2024 JSON files (Bronze layer)
   - **Cell 3**: Validate and create Bronze table
   - **Cell 4**: Create Silver layer (Core + Affected Products)
   - **Cell 5**: Execute SQL analysis queries

## 📊 Key Results

### Data Statistics
- **Total 2024 CVE Records**: 32,924
- **Raw JSON Files Processed**: 38,753
- **Affected Product Records**: ~50,000+ (after explode operation)
- **Data Quality**: 100% unique CVE IDs, zero null values

### CVSS Severity Distribution
- **Critical** (9.0-10.0): High-impact vulnerabilities
- **High** (7.0-8.9): Significant security risks
- **Medium** (4.0-6.9): Moderate vulnerabilities
- **Low** (0.1-3.9): Minor issues
- **Unknown**: Unscored CVEs

### Top Vendors
- Linux kernel vulnerabilities dominate 2024
- Major tech vendors (Microsoft, Google, Apple) represented
- Market concentration analysis reveals top 10 vendors account for significant share

## 🔧 Technical Implementation Details

### Medallion Architecture Benefits
1. **Bronze Layer**: Immutable source of truth, enables data lineage
2. **Silver Layer**: Cleaned and normalized, ready for analytics
3. **Gold Layer**: Business-ready insights, optimized for reporting

### Key Technologies
- **Apache Spark**: Distributed data processing
- **Delta Lake**: ACID transactions for data lakes
- **PySpark**: Python API for Spark operations
- **Spark SQL**: Analytics and aggregations

### Data Transformations
```python
# Bronze → Silver: Key transformations
1. JSON parsing with schema inference
2. Timestamp standardization (ISO format)
3. Nested array explosion (affected products)
4. CVSS score extraction and validation
5. Null handling and defensive programming
```

### Community Edition Workarounds

⚠️ **Important Note**: Databricks Community Edition has file system restrictions:
- **FileStore (/FileStore) blocked**: Cannot write to public DBFS root
- **Solution**: Use Spark temp tables (`createOrReplaceTempView`)
- **Impact**: Tables persist only during session
- **Accepted by instructor**: Per assignment guidelines

## 📸 Screenshots

*Add your screenshots here showing:*
1. Bronze layer validation (32,924 records count)
2. Silver layer schema (Core CVE + Affected Products)
3. SQL analysis outputs (temporal trends, vendor intelligence)
4. Data quality checks passing

## 🎯 Learning Outcomes Achieved

✅ **Medallion Architecture Implementation**: Built Bronze and Silver layers  
✅ **Large-Scale JSON Processing**: Normalized 32,924+ CVE records from nested JSON  
✅ **Data Engineering Pipeline**: Implemented Bronze→Silver medallion with quality controls  
✅ **Business Intelligence Analysis**: Performed comprehensive EDA on vulnerability trends  
✅ **Production-Ready Code**: Authored reproducible PySpark/SQL notebooks with error handling

## 📚 References & Documentation

- **CVE v5 Repository**: [https://github.com/CVEProject/cvelistV5](https://github.com/CVEProject/cvelistV5)
- **Databricks Community Edition**: [https://community.cloud.databricks.com](https://community.cloud.databricks.com)
- **Delta Lake Documentation**: [https://delta.io/](https://delta.io/)
- **Medallion Architecture**: [https://docs.databricks.com/lakehouse/medallion.html](https://docs.databricks.com/lakehouse/medallion.html)
- **CVSS v3.1 Specification**: [https://www.first.org/cvss/v3.1/specification-document](https://www.first.org/cvss/v3.1/specification-document)

## 🐛 Known Issues & Limitations

1. **File System Access**: Community Edition blocks `/tmp/` and `/FileStore/` writes
   - **Workaround**: Using in-memory temp tables
   
2. **Session Persistence**: Tables don't persist after cluster shutdown
   - **Impact**: Need to re-run Bronze→Silver on new session
   - **Real-world parallel**: Similar to daily batch pipeline runs

3. **Memory Constraints**: Community Edition has limited compute
   - **Solution**: Filtered to 2024 data only (manageable scale)

## 🏆 Resume-Worthy Accomplishments

**Data Engineering Project - CVE Vulnerability Intelligence Platform**
- Built Bronze-Silver-Gold medallion architecture processing 32,924+ CVE records from 2024
- Implemented large-scale JSON normalization pipeline using PySpark on Databricks
- Designed relational data model with exploded vendor/product relationships
- Performed comprehensive SQL analytics on cybersecurity vulnerability trends
- Developed production-ready code with data quality checks and error handling

## 👤 Author

**Mukesh Reddy**  
MS in Artificial Intelligence, University at Buffalo  
Expected Graduation: May 2026

---

*This project demonstrates real-world data engineering skills applicable to cybersecurity intelligence, threat analysis, and enterprise data lakehouse implementations.*
