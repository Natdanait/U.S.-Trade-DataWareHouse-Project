# 🧱 U.S. Trade Data Warehouse Architecture

## 1️⃣ Executive Summary & Core Objectives

The **U.S. Trade Data Warehouse Project** establishes a unified, scalable platform for advanced analysis of U.S. international trade flows, leveraging official monthly data from the **U.S. Census Bureau **.  
The system is hosted on **Google BigQuery**, with pipeline orchestration managed by **Google Colaboratory** scripts.

### Core Objectives
- **Scalable Consolidation:** Utilizes BigQuery native SQL to efficiently consolidate multiple years and months of raw Parquet data stored in GCS.  
- **Data Traceability:** Ensures immutability of the Raw Layer and full lineage from source files to final Fact Tables.  
- **Policy Segmentation:** Implements modular Policy Dimension Tables to support flexible, non-exclusive filtering of trade data.  
- **Performance:** Optimizes the final Fact Table using BigQuery partitioning and clustering for high-speed BI and analytical queries.

---

## 2️⃣ Layered Data Management & ELT Workflow

The process follows an **ELT (Extract, Load, Transform)** pattern, optimized by shifting heavy consolidation and transformation (T) to the BigQuery engine.

| **Layer** | **Storage Location** | **Key Process (Engine Used)** | **Data Granularity & Purpose** |
|:-----------|:--------------------|:------------------------------|:-------------------------------|
| 🟢 **Raw (Landing)** | ☁️ GCS (Parquet) | **Load (Colab / PyArrow):** Colab script pulls Census data and writes raw, uncleaned monthly files directly to GCS. | **Immutability:** Serves as the permanent, auditable source backup. |
| 🟡 **Staging** | 🗄️ BigQuery (External Table) | **Consolidation (BigQuery SQL):** Creates a single virtual table spanning multiple years by referencing all monthly Parquet files in GCS. No transformations are applied. | **Unified Query Scope:** Provides one consolidated endpoint for all historical raw data. |
| 🔵 **Warehouse** | 🗂️ BigQuery (Fact Table) | **Transformation (BigQuery SQL):** Performs all cleaning, type casting, date construction, and dimensional lookups. | **Optimized for BI queries:** Final Star Schema model. |

---

## 3️⃣ Raw Data Structure & Transformation Requirements

The data ingested from the Census Bureau is stored in the Raw Layer with structural issues that must be resolved during the Warehouse Load phase.

| **Field Name** | **Type (Raw Schema)** | **Key Role** | **Required Transformation in Warehouse Load** |
|:----------------|:----------------------|:--------------|:----------------------------------------------|
| `GEN_VAL_MO` | STRING | Primary Measure (Fact) | **CAST** to `BIGNUMERIC` (data arrives as strings; numeric precision required). |
| `YEAR`, `MONTH` | STRING | Time Key Components | **CAST** to `INT64` and combine into a single field (`reporting_date`). |
| `I_COMMODITY` | STRING | HTS10 Product Key | **TRIM()** to ensure clean join key against dimensions. |
| `CTY_CODE` | STRING | Country Key | **TRIM()** to ensure clean join key against dimensions. |

---

## 4️⃣ Star Schema Design

### 🔹 HTS Dictionary Dimension (`dim_hts_all`)
This dimension is the central conformed bridge that maps granular `HTS10` codes from the Fact Table to higher-level classifications (8-, 6-, 4-, and 2-digit).  
It also acts as the point of connection for policy segmentation.

**Role:** Enables flexible product roll-up and serves as the key link to all policy definitions.

---

### 🟢 Policy Dimension Tables
Policy tables are external, modular lookup lists used for analytical filtering based on specific U.S. trade measures (e.g., *Section 232*, *Critical Minerals*).

**Separation Principle:**  
Each major policy resides in its own dimension table (e.g., `dim_sec232_iron_steel`) because one HTS10 code can link to multiple, non-exclusive policy measures simultaneously.

**Optimization:**  
All policy dimensions are **clustered by `HTS10`** in BigQuery to maximize join and filtering efficiency.

---

### 🎯 Warehouse Fact Table (`import.fact_imp_val2`)

The final, permanent analytical table populated only after all cleaning and dimensional lookups are complete.

- **Partitioning:** by time (`reporting_date`) to optimize historical range queries.  
- **Clustering:** by main dimension keys (`HTS_Key`, `Country_Key`) to accelerate BI joins.  
- **Structure:** conforms to the Star Schema — joined with `dim_hts_all`, `dim_country`, and relevant policy dimensions.

---

### 🔁 Relationship Overview

```mermaid
flowchart LR
    F["Fact Table (import.fact_imp_val2)"]
    D["HTS Dictionary (dim_hts_all)"]
    P["Policy Dimensions (e.g., dim_sec232_iron_steel, dim_critical_minerals)"]

    F --> D
    D --> P
