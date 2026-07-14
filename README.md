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

# Medallion Architecture

The project follows the **Medallion Architecture**, progressively refining data from raw ingestion to business-ready analytical models.

---

## Bronze Layer

**Purpose**

The Bronze layer stores raw source data exactly as received from the operational database.

**Key Characteristics**

- Raw immutable data
- Source fidelity preserved
- Supports replay and reprocessing
- Loaded directly using Azure Data Factory
- Stored in **Parquet** format
- Organized by table

---

## Silver Layer

**Purpose**

The Silver layer transforms raw data into trusted, analytics-ready datasets.

**Key Characteristics**

- Duplicate removal
- Data quality validation
- Schema drift detection
- Controlled schema evolution
- Metadata-driven transformations
- SCD Type 2 implementation
- Delta Lake MERGE
- Business rule enforcement

---

## Gold Layer

**Purpose**

The Gold layer transforms curated Silver tables into a dimensional model optimized for analytical workloads.

**Key Characteristics**

- Star Schema Modeling
- Surrogate Key generation
- Business-ready datasets
- Optimized for BI tools
- Incremental publishing
- Delta Lake storage

---

# Key Architectural Decisions

---

## 1. Metadata-Driven Pipeline

### Problem

Hardcoding ingestion and transformation logic for every table creates duplicated code, increases maintenance effort, and makes onboarding new tables difficult.

### Design Decision

Implemented a metadata-driven framework where Azure SQL stores processing configuration in **PipelineConfig**, **TransformationConfig**, and **Watermark** tables.

The framework dynamically controls:

- Load Strategy
- Business Keys
- Compare Columns
- Required Columns
- Merge Strategy
- SCD Type
- Watermark Column

Azure Data Factory reads the metadata and invokes reusable Databricks notebooks instead of maintaining separate pipelines per table.

### Benefits

- Eliminates duplicated ETL logic
- New tables require metadata only
- Centralized business rules
- Highly reusable architecture

---

## 2. Full & Incremental Loading Strategy

### Problem

Different tables have different growth patterns and update frequencies.

Applying one ingestion strategy to every table either wastes compute resources or increases pipeline complexity.

### Design Decision

Implemented both Full and Incremental loading strategies.

**Full Load**

- Categories
- Products
- Branches

**Incremental Load**

- Customers
- Orders
- OrderItems

Incremental tables use a metadata-driven watermark maintained in Azure SQL.

### Benefits

- Reduced processing time
- Lower storage consumption
- Scalable ingestion framework

---

## 3. Late-arriving Data Handling

### Problem

Strict watermark-based extraction can miss records arriving after the previous pipeline execution.

### Design Decision

Implemented a configurable lookback window that reprocesses recent records during every execution.

### Trade-off

Reprocessing historical data intentionally introduces duplicate candidate records.

Duplicates are safely eliminated downstream through Delta MERGE and business-key comparison, ensuring idempotent processing while improving data completeness.

### Alternatives Considered

Source-side Change Data Capture (CDC) was considered but requires enabling CDC on the operational database and increases operational complexity.

---

## 4. Reusable SCD Type 2 Framework

### Problem

Master data changes must preserve historical versions without implementing custom logic for every dimension.

### Design Decision

Implemented a reusable metadata-driven SCD Type 2 framework using configurable:

- Business Keys
- Compare Columns

Historical versions are maintained using:

- EffectiveStartDate
- EffectiveEndDate
- IsCurrent

Incoming records are staged before executing a single Delta MERGE.

### Benefits

- Complete historical tracking
- Reusable across multiple dimensions
- Centralized implementation

---

## 5. Controlled Schema Evolution

### Problem

Unexpected source schema changes can silently corrupt downstream datasets.

### Design Decision

Implemented schema drift detection during Bronze → Silver processing.

New columns are validated before explicitly updating the Silver schema using ALTER TABLE.

### Trade-off

Adds a small validation step but provides significantly stronger governance than automatic schema evolution.

### Alternatives Considered

Automatic `mergeSchema=true` was evaluated but rejected to maintain explicit control over structural changes.

---

## 6. Idempotent Data Processing

### Problem

Pipeline retries caused by transient failures should never produce duplicate or inconsistent datasets.

### Design Decision

Combined:

- Duplicate removal
- Business-key comparison
- Delta Lake MERGE

to ensure repeated executions always converge to the same final state.

### Benefits

- Safe reruns
- Atomic writes
- Reliable recovery

---

## 7. Data Governance & Security

### Problem

Enterprise data platforms require centralized governance while avoiding hardcoded credentials.

### Design Decision

Implemented:

- Unity Catalog
- Azure Key Vault

Unity Catalog manages catalogs, schemas, external locations, and Delta tables.

Azure Key Vault securely stores database credentials consumed by Azure Data Factory and Databricks.

### Benefits

- Centralized governance
- Secure credential management
- Reduced operational risk

---

## 8. Star Schema Modeling

### Problem

Normalized OLTP schemas require multiple joins and are inefficient for analytical workloads.

### Design Decision

Transformed Silver tables into a Star Schema consisting of:

### Dimensions

- DimCustomer
- DimBranch
- DimProduct

### Fact

- FactSales

Dimension tables use surrogate keys while the fact table references these keys to simplify reporting and improve query performance.

### Benefits

- Faster analytical queries
- Simplified reporting
- BI-friendly semantic model

---

## 9. Layered Medallion Architecture

### Problem

Mixing raw, validated, and analytical data tightly couples ingestion with reporting and makes troubleshooting difficult.

### Design Decision

Separated responsibilities into three independent layers.

**Bronze**

- Raw immutable ingestion

**Silver**

- Cleansed
- Validated
- Historized

**Gold**

- Business-ready dimensional models

### Benefits

- Clear separation of concerns
- Easier debugging
- Independent evolution
- Improved maintainability

---

# Repository Structure

```text
CoffeeBean-Azure-Data-Engineering/
│
├── README.md
│
├── adf/
│   ├── pipelines/
│   ├── datasets/
│   ├── linkedServices/
│   └── triggers/
│
├── databricks/
│   ├── bronze_to_silver.py
│   └── silver_to_gold.py
│
├── sql/
│   ├── metadata/
│   ├── source/
│   └── procedures/
│
└── docs/
    └── architecture/
        ├── architecture-diagram.png
        ├── source-er-diagram.png
        ├── star-schema-diagram.png
        └── adf-pipeline.png
```

---

# Future Improvements

Although the project demonstrates a production-oriented architecture, several enhancements could further improve scalability and operational maturity.

- Change Data Capture (CDC)
- CI/CD using Azure DevOps
- Audit & Monitoring Framework
- Delta Lake OPTIMIZE & ZORDER
- Automated Data Quality Dashboard
- Unit & Integration Testing
- Partition Pruning Strategy
- Data Lineage & Catalog Documentation

---

# Lessons Learned

Building this project provided practical experience in designing reusable Azure Data Engineering solutions rather than table-specific ETL pipelines.

Key learnings include:

- Metadata-driven frameworks significantly reduce duplicated ETL logic.
- Different tables require different ingestion strategies based on business characteristics.
- Explicit schema evolution provides stronger governance than automatic schema merging.
- Separating change detection from persistence simplifies reusable SCD Type 2 implementations.
- Delta Lake MERGE enables reliable, idempotent processing.
- Handling late-arriving data is essential for production-grade incremental pipelines.
- Proper separation of Bronze, Silver, and Gold improves maintainability and simplifies debugging.

---

# License

This project is intended for **learning, portfolio, and demonstration purposes**.

