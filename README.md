# 🇺🇸 U.S. Trade Data Architecture & Analytics

This repository documents the **data architecture**, **pipeline**, and **analytics framework** that serve as the foundation for analyzing **international trade of the United States**.  
The system integrates multiple data sources and tools to provide policy-driven trade insights for analysts, researchers, and policymakers.

---

## 🌍 Project Overview

The database consolidates **monthly U.S. trade data** (import and export values and quantities) with countries worldwide, sourced primarily from the **U.S. Census Bureau**.

It serves as the **core foundation for U.S. trade policy analysis**, covering:
- **Section 232 Tariffs** (Iron, Steel, Aluminum, Vehicles)
- **Reciprocal Tariff Frameworks** (Annex II & III)
- **Critical Minerals and Agricultural Commodities** (HS 01–23 & 40)

### Key Features
- Multi-level **HTS hierarchy** (10 → 8 → 6 → 4 → 2 digits)
- Monthly and Year-to-Date (YTD) analysis
- Integration with **policy filter tables** (Section 232, Reciprocal, etc.)
- Supports **trend, policy impact, and partner analysis**
- Built for **international trade analysts and economic researchers**

---

## 🧭 Data Journey

> 🔗 See [`docs/01_data_journey.md`](docs/01_data_journey.md)

| Stage | Description |
|-------|--------------|
| **1. Data Source** | Monthly import/export data from the **U.S. Census Bureau** |
| **2. Data Ingestion** | Data extracted via **Google Colab** and uploaded to **Google Cloud Storage (GCS)** |
| **3. Storage Layer** | Raw parquet files organized by year and trade type |
| **4. Data Warehouse (BigQuery)** | ETL process converts raw → clean → fact & dimension tables |
| **5. BI Layer (Power BI)** | Build semantic models, DAX measures, and dashboards for policy analysis |

---

### 🗺️ Data Pipeline Diagram

![Data Pipeline Overview](diagrams/Data%20pipeline%20Diagram.drawio.png)

**Flow Explanation:**
1. **U.S. Census Data** is extracted and uploaded via Google Colab to **Google Cloud Storage (GCS)**.  
2. **GCS** stores raw monthly files (parquet format) for imports and exports (value and quantity).  
3. **BigQuery** ingests data from GCS into staging datasets, performs ETL transformations, and creates fact/dimension tables.  
4. **Power BI** connects to BigQuery for visualization, using DAX and filters to analyze trade policies (Section 232, Reciprocal, etc.).

---

## 🧱 Database Architecture

> 🔗 See [`docs/02_database_architecture.md`](docs/02_database_architecture.md)

**Core Components**
- **Google BigQuery** – Main data warehouse for trade analysis  
- **MySQL (Local)** – Sandbox for testing and smaller datasets  
- **Python ETL (Colab/Composer)** – Automated extraction and transformation scripts  
- **Power BI Service** – Interactive dashboards and reporting layer  

**Key Datasets**
| Dataset | Description |
|----------|--------------|
| `import.fact_imp_val2` | Monthly import value |
| `export.fact_exp_val2` | Monthly export value |
| `Dimension_Table.dim_hts10` | HTS classification (10-digit) |
| `Dimension_sec232.bridge_sec232` | Bridge for Section 232 product coverage |
| `Dimension_reciprocal.bridge_reciprocal` | Bridge for Reciprocal tariff mapping |

---

## 🔐 Data Access & Governance

> 🔗 See [`docs/03_data_access.md`](docs/03_data_access.md)

- Managed via **Google Cloud IAM Roles** (`BigQuery Data Viewer`, `Data Editor`, etc.)  
- Access separated between **Staging**, **Production**, and **Analytics** environments  
- Credentials handled via **service accounts** (no personal tokens)  
- Version control and schema tracking through **GitHub repository commits**  

---

## 🧾 Data Dictionary

> 🔗 See [`docs/04_data_dictionary.md`](docs/04_data_dictionary.md)

| Field | Description | Type | Source |
|-------|--------------|------|--------|
| `HTS10` | Harmonized Tariff Schedule 10-digit code | STRING | USITC |
| `Country` | Partner country | STRING | U.S. Census |
| `Year` / `Month` | Trade period | INTEGER | U.S. Census |
| `Imp_Value` | Import value (USD) | NUMERIC | Census |
| `Exp_Value` | Export value (USD) | NUMERIC | Census |
| `Section232_Flag` | Indicates product under Section 232 | BOOLEAN | Policy Dataset |
| `Reciprocal_Flag` | Indicates product under Reciprocal Tariff | BOOLEAN | Policy Dataset |

---

## 🔗 Data Relationship

> 🔗 See [`docs/05_data_relationship.md`](docs/05_data_relationship.md)

| Relationship | Description |
|---------------|--------------|
| `fact_imp_val2` → `dim_hts10` | Connects trade facts to product classification |
| `dim_hts10` → `bridge_sec232` | Links products to Section 232 coverage |
| `bridge_sec232` → `dim_section232_category` | Categorizes products by policy segment |
| `fact_imp_val2` → `bridge_reciprocal` | Enables analysis under reciprocal tariffs |

See full ER diagram: [`diagrams/erd_bigquery_model.png`](diagrams/erd_bigquery_model.png)

---

## 🗓 Data Roadmap

> 🔗 See [`docs/06_data_roadmap.md`](docs/06_data_roadmap.md)

| Phase | Focus | Status |
|-------|--------|--------|
| **Phase 1** | Section 232 Integration | ✅ Completed |
| **Phase 2** | Reciprocal Tariff (Annex II & III) | 🔄 In Progress |
| **Phase 3** | Critical Mineral Analysis (ASEAN scope) | 🚀 Planned |
| **Phase 4** | Predictive Trade Analytics | 🧠 Upcoming |

---

## 👤 Author

**Natdanai Tapanwong**  
Data Analyst / Visualization Developer  
Office of Commercial Affairs, Royal Thai Embassy (Washington D.C.)  
📧 natdanai.tapanwong@gmail.com  

---

## 🪪 License

This project is open-source under the [MIT License](LICENSE).
