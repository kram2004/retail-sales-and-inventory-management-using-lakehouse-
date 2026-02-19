#  Retail Sales & Inventory Lakehouse
**Databricks | Delta Lake | Unity Catalog | ADLS Gen2 | Power BI**

---

##  Project Overview

This project implements a production-style **Retail Lakehouse Data Platform** using **Databricks** and the **Medallion Architecture (Bronze → Silver → Gold)**.

A multi-channel retail enterprise generates continuous transactional data from physical stores and e-commerce platforms. The system ingests, cleans, governs, and transforms raw operational data into analytics-ready datasets powering business intelligence dashboards.

The project demonstrates real-world data engineering challenges including:

- Streaming ingestion
- Schema drift handling
- Data quality validation
- Late-arriving data processing
- Incremental MERGE pipelines
- Governance and lineage tracking
- BI-ready dimensional modeling

---

##  Business Objectives

The platform enables analytics for:

- Gross Sales & Net Revenue
- Discount utilization analysis
- Return rate monitoring
- Promotion effectiveness (Promo Lift)
- Inventory health & stockout risk
- Shrinkage detection
- Store and product performance tracking

---

##  Architecture

### Medallion Lakehouse Flow

ADLS Raw Landing
↓
Bronze Layer (Streaming Ingestion)
↓
Silver Layer (Data Cleansing & Quality)
↓
Gold Layer (Star Schema & KPIs)
↓
Databricks SQL Warehouse
↓
Power BI Dashboards


---

##  Technology Stack

| Technology | Purpose |
|------------|---------|
| Databricks | Data engineering platform |
| Delta Lake | ACID storage & incremental processing |
| Auto Loader | Streaming ingestion |
| Unity Catalog | Governance & access control |
| ADLS Gen2 | Cloud storage |
| Databricks Workflows | Pipeline orchestration |
| Databricks SQL | Analytics serving layer |
| Power BI | Visualization & dashboards |

---

##  Repository Structure

Retail-Lakehouse/
│
├── 00_Governance_Setup.ipynb
├── 01_Project_Config.ipynb
├── 02_Ingest_Bronze.ipynb
├── 03_Silver_Ingestion.ipynb
├── 04_Gold_Ingestion.ipynb
├── 05_Business_Insights.ipynb
│
└── README.md


---

##  Data Architecture

###  Bronze Layer — Raw Ingestion

- Streaming ingestion using Databricks Auto Loader
- Captures raw data exactly as received
- Supports schema drift via `_rescued_data`
- Adds ingestion metadata:
  - source_file
  - ingest_timestamp
  - batch_id

**Tables**
- `bronze.pos_sales_raw`
- `bronze.returns_raw`
- `bronze.inventory_raw`

---

###  Silver Layer — Data Cleansing & Quality

Data is standardized, validated, and deduplicated.

#### Data Quality Rules

- quantity > 0
- unit_price > 0
- discount between 0–100
- valid store and product references
- orphan returns routed to quarantine

#### Transformations

- String normalization
- Timestamp validation
- Deduplication using window functions
- Derived metric calculations

**Outputs**
- `silver.pos_sales_clean`
- `silver.returns_clean`
- `silver.inventory_clean`
- `quarantine.*` rejected datasets

---

###  Gold Layer — Analytics Model

A **Star Schema** optimized for analytics workloads.

#### Fact Tables
- `gold.fact_sales`
- `gold.fact_returns`
- `gold.fact_inventory_daily`

#### Dimension Tables
- `gold.dim_product`
- `gold.dim_store`
- `gold.dim_customer`
- `gold.dim_promotion`
- `gold.dim_date`

Features:
- Incremental MERGE upserts
- Late-arriving return handling
- Surrogate key generation
- Delta optimization (OPTIMIZE + ZORDER)

---

##  Key Engineering Features

- Structured Streaming ingestion
- Schema evolution handling
- Data Quality framework with quarantine layer
- Incremental MERGE pipelines
- Delta Lake time travel
- Query performance optimization
- Unity Catalog governance
- Workflow orchestration with monitoring

---

##  Data Governance

Implemented using Unity Catalog:

- Role-Based Access Control (RBAC)
- Data lineage tracking
- Controlled schema access
- Optional masking for customer PII
- Audit-ready access model

### Roles

- **Engineers** → Full pipeline access
- **Analysts** → Gold layer access
- **Finance** → KPI views only

---

##  Analytics & BI Layer

Databricks SQL views power business dashboards:

- Daily Revenue & Margin Trends
- Store Performance Leaderboard
- Product Performance Analysis
- Return Rate Hotspots
- Promotion Effectiveness
- Inventory Health Monitoring
- Retail Anomaly Detection

Dashboards are built using **Power BI**.

---

##  Anomaly Detection Engine

Retail anomaly rules identify operational risks:

- Price outliers by category
- Discount abuse detection
- Return spikes
- Shrinkage anomalies
- Sudden sales drops
- Stockout risk surges
- Promotion misuse

Output table:


---

## Pipeline Orchestration

Implemented using Databricks Workflows:

1. Bronze ingestion (Sales & Returns)
2. Silver cleansing & quarantine
3. Dimension builds
4. Fact MERGE operations
5. Inventory batch processing
6. Anomaly detection

Includes:
- Task dependencies
- Retry policies
- Monitoring metrics

---

##  Performance Optimization

- Partitioned fact tables by date
- OPTIMIZE & ZORDER indexing
- Query plan benchmarking
- Reduced BI query latency

---

##  Engineering Challenges Solved

- Late-arriving returns updating historical KPIs
- Duplicate transactions across batches
- Schema drift in master data
- Invalid pricing and discount anomalies
- Inventory inconsistencies and shrinkage spikes

---

##  How to Run the Project

1. Configure storage paths in `01_Project_Config.ipynb`
2. Execute notebooks in order:
00_Governance_Setup
01_Project_Config
02_Ingest_Bronze
03_Silver_Ingestion
04_Gold_Ingestion
05_Business_Insights


3. Start Databricks Workflows for automation.
4. Connect Power BI to Databricks SQL Warehouse.

---

##  Skills Demonstrated

- Data Engineering (Batch + Streaming)
- Lakehouse Architecture
- Delta Lake Optimization
- Dimensional Modeling
- Data Governance
- ETL/ELT Pipeline Design
- Analytics Engineering
- BI Integration

---

##  Author

**K Ramabayapu Reddy**  
Data Engineering Enthusiast

 ramreddyk2105@gmail.com  
 GitHub: (http://github.com/kram2004/retail-sales-and-inventory-management-using-lakehouse-)

---

##  Future Enhancements

- CDC ingestion support
- Real-time dashboard refresh
- ML-based anomaly detection
- Data observability metrics
- Automated data contract validation
