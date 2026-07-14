# ☕ Coffee Bean Analytics – Metadata-Driven Azure Data Engineering Pipeline

<p align="center">

![Azure](https://img.shields.io/badge/Microsoft%20Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Azure Data Factory](https://img.shields.io/badge/Azure%20Data%20Factory-0078D4?style=for-the-badge)
![Azure Databricks](https://img.shields.io/badge/Azure%20Databricks-EF3E42?style=for-the-badge)
![Azure SQL](https://img.shields.io/badge/Azure%20SQL-CC2927?style=for-the-badge)
![ADLS Gen2](https://img.shields.io/badge/ADLS%20Gen2-0178D4?style=for-the-badge)
![Delta Lake](https://img.shields.io/badge/Delta%20Lake-0A6ED1?style=for-the-badge)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-FDEE21?style=for-the-badge&logo=apachespark&logoColor=black)
![Unity Catalog](https://img.shields.io/badge/Unity%20Catalog-FF3621?style=for-the-badge)
![Azure Key Vault](https://img.shields.io/badge/Azure%20Key%20Vault-0078D4?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

</p>

---

## Overview

Coffee Bean Analytics demonstrates an **end-to-end metadata-driven Azure Data Engineering solution** built using the Medallion Architecture.

The pipeline ingests transactional data from **NeonDB (PostgreSQL)**, orchestrates ingestion through **Azure Data Factory**, performs scalable transformations in **Azure Databricks**, and publishes analytics-ready dimensional models into the Gold layer.

The solution emphasizes **reusability**, **maintainability**, and **production-ready engineering practices**, including::

- Metadata-driven pipeline orchestration
- Full & Incremental data ingestion
- Watermark-based incremental processing
- Late-arriving data handling
- Data Quality Validation
- Schema Drift Detection
- Controlled Schema Evolution
- Slowly Changing Dimension (Type 2)
- Delta Lake MERGE (Idempotent Processing)
- Unity Catalog Governance
- Azure Key Vault Integration
- Dimensional Modeling (Star Schema)

---

# Table of Contents

- [Overview](#overview)
- [Data Flow](#data-flow)
- [Architecture Diagram](#architecture-diagram)
- [Technology Stack](#technology-stack)
- [Project Statistics](#-project-statistics)
- [Repository Tour](#repository-tour)
- [Source ER Diagram](#source-er-diagram)
- [Gold Star Schema](#gold-star-schema)
- [Medallion Architecture](#medallion-architecture)
- [Key Architectural Decisions](#key-architectural-decisions)
- [Repository Structure](#repository-structure)
- [Future Improvements](#future-improvements)
- [Lessons Learned](#lessons-learned)
- [License](#license)

---

# Data Flow

```text
                    NeonDB (PostgreSQL)
                            │
                            ▼
                Azure Data Factory (ADF)
                            │
                            ▼
              Bronze Layer (Parquet - ADLS Gen2)
                            │
                            ▼
              Azure Databricks (Bronze → Silver)
                • Data Quality Validation
                • Duplicate Removal
                • Schema Drift Detection
                • Schema Evolution
                • SCD Type 2
                • Delta MERGE
                            │
                            ▼
              Silver Layer (Delta Lake)
                            │
                            ▼
              Azure Databricks (Silver → Gold)
                • Star Schema Modeling
                • Dimension Building
                • Fact Table Generation
                            │
                            ▼
               Gold Layer (Delta Lake)
                            │
                            ▼
          Power BI / Reporting / Analytics
```

---

# Architecture Diagram

The following diagram illustrates the overall system architecture, showing the flow of data from the operational source system to the analytical layer.


![Architecture Diagram](docs/architecture/architecture-diagram.png)

---

# Technology Stack

| Component | Technology |
|------------|------------|
| **Source Database** | NeonDB (PostgreSQL) |
| **Metadata Repository** | Azure SQL Database |
| **Orchestration** | Azure Data Factory |
| **Processing Engine** | Azure Databricks |
| **Storage** | Azure Data Lake Storage Gen2 |
| **Bronze Format** | Parquet |
| **Silver & Gold Format** | Delta Lake |
| **Governance** | Unity Catalog |
| **Secrets Management** | Azure Key Vault |
| **Programming Languages** | Python, PySpark, SQL |

---

# 📊 Project Statistics

| Metric | Value |
|--------|-------|
| **Source Tables** | 6 |
| **Silver Tables** | 6 |
| **Gold Tables** | 4 |
| **Azure Services** | 6 |
| **Databricks Notebooks** | 2 |
| **Metadata-Driven** | ✅ Yes |
| **SCD Type 2 Tables** | Customers, Products, Branches |
| **Incremental Load** | Customers, Orders, OrderItems |
| **Full Load** | Categories, Products, Branches |

---

# Repository Tour

The repository is organized by platform components to separate orchestration, processing logic, database objects, and documentation.

| Folder | Description |
|---------|-------------|
| **/adf** | Azure Data Factory pipelines, datasets, linked services, and triggers |
| **/databricks** | PySpark notebooks implementing Bronze → Silver and Silver → Gold transformations |
| **/sql** | Source schema, metadata tables, stored procedures, and initialization scripts |
| **/docs/architecture** | Architecture diagram, Source ER Diagram, Star Schema, and ADF pipeline screenshots |

---

# Source ER Diagram

The operational source database is normalized to support transactional workloads. Orders and OrderItems capture business transactions, while Customers, Products, Categories, and Branches represent master data.

> **Source ER Diagram Placeholder**

```text
docs/
└── architecture/
    └── source-er-diagram.png
```

![Source ER Diagram](docs/architecture/source-er-diagram.png)

---

# Gold Star Schema

The curated Gold layer transforms normalized Silver tables into a dimensional model optimized for analytical workloads.

The model consists of:

### Dimensions

- DimCustomer
- DimBranch
- DimProduct

### Fact

- FactSales

> **Gold Star Schema Placeholder**

```text
docs/
└── architecture/
    └── star-schema-diagram.png
```

![Gold Star Schema](docs/architecture/star-schema-diagram.png)

---
