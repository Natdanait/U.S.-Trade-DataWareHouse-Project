# 🇺🇸 U.S. Trade Data Architecture & Analytics

This repository presents the **data foundation** for analyzing the international trade of the **United States**.  
It integrates data from multiple official sources into a unified, policy-aware analytics environment — designed for trade analysts, researchers, and policymakers who need to explore trade flows, assess policy impacts, and visualize trends clearly.

---

## 🌍 Project Overview

The system consolidates **monthly U.S. import and export data** with countries worldwide.  
Each record is identified by a **10-digit Harmonized Tariff Schedule (HTS)** code — the most detailed product level — and can be grouped upward into broader product categories (8-, 6-, 4-, and 2-digit levels).

This database serves as the analytical backbone for evaluating U.S. trade policies such as:

- **Section 232 Tariffs** (Iron, Steel, Aluminum, Vehicles, etc.)  
- **Reciprocal Tariff Frameworks** (Annex II & III)  
- **Critical Minerals and Agricultural Commodities** (HS 01–23 & 40)

### Key Highlights
- Complete **HTS hierarchy** for flexible aggregation and drill-down  
- Supports **monthly and year-to-date (YTD)** time series analysis  
- Connects directly to **policy filter tables** (Section 232, Reciprocal, etc.)  
- Enables fast exploration of **trade trends, partner composition, and policy impacts**  
- Designed for **data-driven decision-making** in international trade analysis  

---

## 🧭 Data Journey

> 🔗 Detailed steps in [`docs/01_data_journey.md`](docs/01_data_journey.md)

| Stage | Description |
|-------|--------------|
| **1. Source** | Monthly import/export data from the **U.S. Census Bureau** |
| **2. Ingestion** | Extracted via **Google Colab** and stored in **Google Cloud Storage (GCS)** |
| **3. Storage** | Raw parquet files organized by year, month, and trade type |
| **4. Warehouse** | **BigQuery** transforms raw data into clean fact and dimension tables |
| **5. Analytics** | **Power BI** connects to BigQuery for visualization, DAX modeling, and policy dashboards |

---

### 🗺️ Data Pipeline Overview

![Data Pipeline](Diagrams/Data%20pipeline%20Diagram.drawio.png)

**Flow Summary**

1. **U.S. Census Data** is extracted through Google Colab and uploaded to **Google Cloud Storage (GCS)**.  
2. **GCS** acts as the central repository for raw monthly trade files in parquet format.  
3. **BigQuery** ingests and transforms data into structured **fact** and **dimension** tables (e.g., `fact_imp_val2`, `dim_hts10`, `bridge_sec232`).  
4. **Power BI** connects to BigQuery to power dashboards that track trade flows and evaluate the impact of policies like Section 232 and Reciprocal Tariffs.

---

## 🧱 Database Architecture

> 🔗 See [`docs/02_database_architecture.md`](docs/02_database_architecture.md)

**Core Components**
- **Google BigQuery** – Main cloud data warehouse  
- **MySQL (Local)** – Sandbox for quick testing  
- **Python ETL (Colab / Composer)** – Automated ingestion and transformation  
- **Power BI Service** – Visualization and dashboard layer  

**Key Datasets**
| Dataset | Description |
|----------|--------------|
| `import.fact_imp_val2` | Monthly import value |
| `export.fact_exp_val2` | Monthly export value |
| `Dimension_Table.dim_hts10` | HTS classification (10-digit) |
| `Dimension_sec232.bridge_sec232` | Section 232 product mapping |
| `Dimension_reciprocal.bridge_reciprocal` | Reciprocal tariff mapping |

---

## 🔐 Data Access & Governance

> 🔗 See [`docs/03_data_access.md`](docs/03_data_access.md)

- Managed through **Google Cloud IAM** roles (`Viewer`, `Editor`, `Owner`)  
- Separate **staging**, **production**, and **analytics** environments  
- Access controlled via **service accounts**, not personal credentials  
- Schema and version tracking maintained in this **GitHub repository**

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
| `Section232_Flag` | Product listed under Section 232 tariffs | BOOLEAN | Policy Dataset |
| `Reciprocal_Flag` | Product listed under Reciprocal tariff | BOOLEAN | Policy Dataset |

---

## 🔗 Data Relationships

> 🔗 See [`docs/05_data_relationship.md`](docs/05_data_relationship.md)

| Relationship | Description |
|---------------|--------------|
| `fact_imp_val2` → `dim_hts10` | Connects trade data to product classification |
| `dim_hts10` → `bridge_sec232` | Links products to Section 232 coverage |
| `bridge_sec232` → `dim_section232_category` | Groups products by policy category |
| `fact_imp_val2` → `bridge_reciprocal` | Enables analysis under Reciprocal Tariffs |

Full ER Diagram: [`diagrams/erd_bigquery_model.png`](diagrams/erd_bigquery_model.png)

---

## 🗓 Data Roadmap

> 🔗 See [`docs/06_data_roadmap.md`](docs/06_data_roadmap.md)

| Phase | Focus | Status |
|-------|--------|--------|
| **Phase 1** | Section 232 integration | ✅ Completed |
| **Phase 2** | Reciprocal Tariff (Annex II & III) | 🔄 In Progress |
| **Phase 3** | Critical Mineral Analysis (ASEAN scope) | 🚀 Planned |
| **Phase 4** | Predictive Trade Analytics & Automation | 🧠 Upcoming |

---

## 👤 Author

**Natdanai Tapanwong**  
Data Analyst / Visualization Developer  
Office of Commercial Affairs, Royal Thai Embassy (Washington D.C.)  
📧 natdanai.tapanwong@gmail.com  

---

## 🪪 License

This project is released under the [MIT License](LICENSE).
