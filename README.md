# U.S. Trade Data Warehouse and Viuslaizations Project

This project is part of the **U.S. Trade Landscape Reports**, which requires a complete **ETL (Extract, Transform, Load)** process design and **data visualization pipeline** to support trade policy analysis and reporting.

The ETL pipeline is designed to **automate the end-to-end flow of U.S. trade data**—from data extraction, cleaning, transformation, and storage, to final visualization.  
Each stage ensures data accuracy, scalability, and efficiency in updating multi-year import and export datasets.

The dataset is retrieved from the [International Trade Data – U.S. Census Bureau](https://www.census.gov/foreign-trade/data/index.html), covering monthly **import and export statistics** (value and quantity) under the **Harmonized Tariff Schedule (HTS)** classification system.

---

## 🏗️ Project Architecture Overview

The pipeline integrates multiple technologies within the Google Cloud ecosystem:

1. **Extract** → Retrieve raw monthly data using Python scripts in Google Colab (via U.S. Census API).  
2. **Transform** → Clean, structure, and join data in **BigQuery**, combining fact and dimension tables.  
3. **Load** → Store processed datasets in optimized tables ready for analysis.  
4. **Visualize** → Build interactive dashboards in **Power BI** with dynamic filters (HS2–HS10, country, and period selection).

This workflow allows analysts to quickly identify trends, track YTD performance, and evaluate the impact of tariff policies such as Section 232 and Reciprocal Tariffs.

---

## 📊 Data Pipeline Diagram
![Data Pipeline](Diagrams/Data%20pipeline%20Diagram.drawio.png)
*Figure 1: End-to-End U.S. Trade Data Pipeline*

---

## 🎯 Dashboard Objectives & Insights

The Power BI dashboard is designed to help analysts and policymakers explore both **fundamental trade patterns** and **policy-specific insights** using U.S. import and export data from the U.S. Census Bureau.  
It supports two levels of analysis: **basic trade monitoring** and **advanced policy evaluation**.

### 🧩 1. Basic Trade Analysis
- **Overall Trade Trends** – Track imports, exports, and trade balance by month or YTD.  
- **Top Trading Partners** – Identify leading source and destination countries.  
- **Commodity Composition** – Analyze trade by HS2/HS4 product groups.  
- **Country–Product Matrix** – Explore major traded products by partner country.  

> 📊 *These insights establish a clear picture of the U.S. trade structure and performance.*

### 🌍 2. Advanced Policy & Segmented Analysis
With additional segmentation—**agricultural commodities (HS 01–23 & 40)**, **reciprocal tariff lists**, and **Section 232 coverage**—the dashboard provides deeper policy insights:
- **Agricultural Trade Focus** – Compare agricultural vs non-agricultural sectors.  
- **Reciprocal Tariff Evaluation** – Track trade of products under reciprocal tariff lists (Annex II & III).  
- **Section 232 Monitoring** – Measure import exposure and country-level impacts.  
- **Negotiation Scenarios** – Analyze products moved from exclusion to negotiation lists.  

> 🧠 *These advanced insights support data-driven decision-making and trade policy evaluation.*

---

## 🧱 Data Model & Table Relationship

The data warehouse follows a **star-schema** model built in **Google BigQuery**, combining fact and dimension tables to support both detailed and aggregated analysis.

### 🗂️ Core Fact Tables
| Table | Description | Granularity |
|--------|--------------|--------------|
| `import.fact_imp_val2` | Monthly import value | Year–Month–Country–HTS10 |
| `import.fact_imp_qty` | Monthly import quantity | Year–Month–Country–HTS10 |
| `export.fact_exp_val` *(optional)* | Monthly export value | Year–Month–Country–HTS10 |

### 🧭 Dimension & Bridge Tables
| Table | Description |
|--------|--------------|
| `Dimension_Table.dim_hts10` | HTS 10-digit reference (with hierarchy 2–4–6–8–10). |
| `Dimension_Table.dim_hts_all` | Aggregated HTS levels for Power BI slicers. |
| `Dimension_Table.dim_country` | Country reference (name, ISO, region). |
| `Dimension_sec232.bridge_sec232` | Section 232 product–HTS linkage. |
| `Dimension_sec232.bridge_reciprocal` | Reciprocal tariff product linkage (Annex II & III). |

### 🔗 Relationship Overview

---

## 🔄 Data Refresh & Automation

| Step | Tool | Description |
|------|------|--------------|
| **1. Data Extraction** | Google Colab / Python | Fetch latest monthly import/export data via U.S. Census API. |
| **2. Upload to GCS** | Google Cloud Storage | Store raw `.parquet` files organized by year. |
| **3. Load to BigQuery** | BigQuery Staging | Append new data to staging tables. |
| **4. Transform & Clean** | SQL Queries | Standardize fields, join with HTS dimensions. |
| **5. Visualization Update** | Power BI Service | Refresh datasets and update dashboards. |

> ⚙️ *Process runs monthly, ensuring near real-time trade insights.*

---

## 🧩 Core Technologies

| Layer | Tool | Function |
|--------|------|-----------|
| **Extraction** | Python (Colab) | Retrieve trade data |
| **Storage** | Google Cloud Storage | Store raw Parquet files |
| **Warehouse** | BigQuery | Transform & host clean datasets |
| **Visualization** | Power BI | Interactive dashboards |

---

## 👤 Author
**Natdanai Tapanwong**  
Data Analyst & BI Developer  
📧 t.natdanai@gmail.com  
🌐 [LinkedIn](https://www.linkedin.com/in/natdanai-tapanwong-a650b077/)

---

© 2025 Natdanai Tapanwong | MIT License
