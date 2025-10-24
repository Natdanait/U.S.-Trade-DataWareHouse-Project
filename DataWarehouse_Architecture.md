# 🧱 U.S. Trade Data Warehouse Architecture

## 1️⃣ Executive Summary & Core Objectives

This architecture establishes a scalable platform for **U.S. trade analysis**, leveraging official monthly data from the **U.S. Census Bureau (FT900 series)**.  
The entire pipeline runs within the **Google Cloud Platform (GCP)**, orchestrated in **Google Colaboratory**, with data stored and analyzed in **Google BigQuery**.

### Core Objectives
- **Scalable Consolidation:** Leverages BigQuery’s native processing power to efficiently consolidate large monthly Census datasets into annual staging tables.  
- **Data Traceability:** Maintains full lineage from immutable Parquet files in GCS to final analytical outputs.  
- **Policy Segmentation:** Uses modular Policy Dimension Tables to support flexible, non-exclusive filtering of trade data.  
- **Performance:** Optimizes BigQuery Fact Tables with partitioning and clustering for high-speed BI and analytical queries.

---

## 2️⃣ Layered Data Management & ELT Workflow

The pipeline follows a **three-layered ELT structure**, with most transformation (T) logic executed directly in BigQuery.

| **Layer** | **Storage Location** | **Key Process (Engine)** | **Purpose / Granularity** |
|:-----------|:--------------------|:--------------------------|:---------------------------|
| 🟢 **Raw (Landing)** | ☁️ GCS (Parquet) | **Load (Colab / PyArrow):** Colab scripts pull monthly Census data and write uncleaned Parquet files directly to GCS. | Immutable source backup preserving the original Census file structure. |
| 🟡 **Staging** | 🗄️ BigQuery | **Consolidation & Cleaning (SQL):** BigQuery queries GCS via External Tables, type-casts, cleans, and consolidates all monthly data for a given year (e.g., `trade_import_2025`). | Clean, validated, year-consolidated data ready for dimensional joins. |
| 🔵 **Warehouse** | 🗂️ BigQuery | **Star Schema Creation (SQL):** Performs final JOINs and DML operations to populate Fact and Dimension tables. | Optimized for BI queries using a Star Schema model. |

**Key Advantage:**  
Using **PyArrow** and **Parquet** enables efficient columnar reads/writes in Colab, greatly accelerating Load and Transform phases.

---

## 3️⃣ Raw Data Structure & Staging Requirements

Raw data ingested from the Census Bureau (stored as Parquet in GCS) requires casting and cleaning in the Staging Layer before use.

| **Field Name** | **Raw Type** | **Key Role** | **Required Staging Transformation** |
|:----------------|:-------------|:--------------|:------------------------------------|
| `GEN_VAL_MO` | STRING | Primary Measure (Fact) | CAST to `BIGNUMERIC` for calculation accuracy. |
| `I_COMMODITY` | STRING | HTS10 Product Key | `TRIM()` to ensure clean join key. |
| `CTY_CODE` | STRING | Country Key | `TRIM()` to ensure clean join key. |
| `YEAR`, `MONTH` | STRING | Time Key Components | CAST to `INT64` and combine into `reporting_date`. |
| `CTY_NAME` | STRING | Descriptive Attribute | Pass-through. |
| `I_COMMODITY_LDESC` | STRING | Descriptive Attribute | Pass-through. |

---

## 4️⃣ Star Schema Design

The warehouse adopts a **Star Schema** model optimized for analytical queries in BigQuery and Power BI.

### 🔹 HTS Dictionary Dimension (`dim_hts_all`)
The **HTS Dictionary Table** is the central conformed dimension linking trade values to product and policy context.  
It maps granular 10-digit HTS codes to higher-level classifications (8-, 6-, 4-, and 2-digit) and stores corresponding descriptions.

**Role:** Acts as the bridge key allowing the Fact Table (containing HTS10 codes) to be segmented by multiple external Policy Dimension tables.

---

### 🟢 Policy Dimension Tables (The Filters)
Policy tables are static, external lookup lists that segment HTS codes according to U.S. trade measures.

| **Table Name** | **Policy Category** | **Relationship Type** | **Optimization** |
|:----------------|:--------------------|:-----------------------|:-----------------|
| `dim_sec232_iron_steel` | Section 232 – Iron & Steel | One-to-Many via `dim_hts_all` | Clustered by `HTS10` for semi-join efficiency |
| `dim_sec232_vehicle_bus` | Section 232 – Heavy & Medium-Duty Vehicles & Buses | One-to-Many via `dim_hts_all` | Clustered by `HTS10` |
| `dim_reciprocal_tariff` | Reciprocal Tariff (Annex II & III) | One-to-Many via `dim_hts_all` | Clustered by `HTS10` |
| `dim_critical_minerals` | Critical Minerals List | One-to-Many via `dim_hts_all` | Clustered by `HTS10` |

---

### 🔁 Data Flow Sequence

```mermaid
flowchart LR
    A["Census Monthly Files"] --> B["Colab Load → GCS (Parquet)"]
    B --> C["BigQuery External Table"]
    C --> D["Staging (Clean & Consolidate)"]
    D --> E["Warehouse Fact Table (import.fact_imp_val2)"]
    E --> F["Power BI / Policy Dashboards"]
