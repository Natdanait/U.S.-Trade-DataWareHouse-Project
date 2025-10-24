# U.S. Trade Data Warehouse Project

This repository presents the **data foundation** for analyzing the international trade of the **United States**.
It integrates official U.S. trade data and policy references into a single analytical environment — designed for trade analysts, researchers, and policymakers who seek to understand trade trends, performance, and policy impacts clearly and efficiently.

-----

## 🌍 Project Overview

The **U.S. Trade Data Architecture** is built to answer the most fundamental questions about U.S. international trade, such as:

  - Has U.S. **import or export** increased or decreased this year?
  - What are the **top imported or exported products**?
  - Which countries are the **main trading partners**?
  - How have trade **trends changed over time**?

These represent the system’s **core analytical capabilities**, providing accurate, consistent, and ready-to-use insights on the structure and evolution of U.S. trade at both the **product** and **country** levels.

-----

Over time, the database has evolved beyond basic statistics.
By integrating **additional features** and **policy-specific dimension tables**, the system now supports **deeper, policy-driven analysis**, including:

  - **Agricultural trade focus** (HS Chapters 01–23 & 40)
  - **Section 232 Tariff coverage** (Iron, Steel, Aluminum, Vehicles, etc.)
  - **Reciprocal Tariff framework** (Annex II & III lists)
  - **Country and product-level impact evaluations** under specific trade measures

These enhancements make it possible to perform advanced analysis more **accurately**, **rapidly**, and **consistently** — for example:

> *"How have Section 232 tariffs affected imports of steel from ASEAN countries?”*
> *“Which agricultural products are showing sustained import growth?”*

Together, these features form a **scalable and modular architecture** that grows with new data sources, trade policies, and analytical needs.

### Key Highlights

  - Multi-level **HTS hierarchy** (10 → 8 → 6 → 4 → 2 digits)
  - Combines **monthly and year-to-date (YTD)** trade trends
  - Integrated **policy filter tables** (Section 232, Reciprocal, etc.)
  - Enables both **high-level monitoring** and **deep-dive policy evaluation**
  - Designed for **international trade analysts** and **economic policymakers**

-----

## 🧭 Data Journey

| Stage | Description |
| :--- | :--- |
| **1. Data Source** | Monthly import/export data from the **U.S. Census Bureau official trade datasets**. |
| **2. Ingestion** | Extracted via **Google Colab** and uploaded to **Google Cloud Storage (GCS)**. |
| **3. Storage** | Raw parquet files organized by year, month, and trade type (GCS is the **Immutable Raw Layer**). |
| **4. Data Warehouse** | **BigQuery** consolidates raw files (Staging) and transforms them into clean fact and dimension tables (Warehouse). |
| **5. Analytics** | **Power BI** connects to BigQuery for visualization, DAX modeling, and policy dashboards. |

-----

### 🗺️ Data Pipeline Overview

\![Diagrams/Data%20pipeline%20Diagram.drawio.png]

**Flow Summary**

1.  **U.S. Census Data** is extracted through Google Colab and uploaded to **Google Cloud Storage (GCS)**.
2.  **GCS** acts as the central repository for raw monthly trade files (parquet format).
3.  **BigQuery** ingests and transforms data into structured **fact** and **dimension** tables (e.g., `fact_imp_val2`, `dim_hts_all`, `bridge_sec232`).
4.  **Power BI** connects directly to BigQuery for visual exploration and policy dashboards.

-----

## 🧱 Database Architecture

> 🔗 See [**DataWarehouse\_Architecture.md**](https://www.google.com/search?q=DataWarehouse_Architecture.md)

**Core Components**

  - **Google BigQuery** – Main data warehouse for large-scale analytics
  - **MySQL (Local)** – Sandbox environment for quick queries and local testing
  - **Python ETL (Colab / Composer)** – Automated ingestion and transformation
  - **Power BI Service** – Interactive dashboard and reporting layer

**Key Datasets (Aligned with Data Dictionary)**
| Dataset | Table Name | Description |
| :--- | :--- | :--- |
| `import` | `fact_imp_val2` | Consolidated Monthly Import Value and Quantity. |
| `import` | `fact_imp_val` | Raw Monthly Import Value (Non-Consolidated). |
| `export` | `fact_exp_val` | Monthly Export Value. |
| `Dimension_Table` | `dim_hts_all` | Central HTS Product Hierarchy (10-digit to 2-digit). |
| `Dimension_sec232` | `dim_sec232_...` | Product mapping for specific Section 232 measures. |

-----

## 🔐 Data Access & Governance

> 🔗 See [`docs/03_data_access.md`](https://www.google.com/search?q=docs/03_data_access.md)

  - Managed through **Google Cloud IAM** roles (`Viewer`, `Editor`, `Owner`)
  - Separate environments for **Staging**, **Production**, and **Analytics**
  - Access controlled via **service accounts** (no personal credentials)
  - Schema and version tracking maintained in this **GitHub repository**

-----

## 🧾 Data Dictionary (Aligned with Fact Tables)

> 🔗 See [**Data\_Dictionary.md**](https://www.google.com/search?q=Data_Dictionary.md)

| Field | Description | Type | Fact Table Usage |
| :--- | :--- | :--- | :--- |
| **I/E\_COMMODITY** | 10-digit HTS code (Import/Export product key) | `STRING` | `fact_imp_val`, `fact_exp_val` |
| **CTY\_CODE** | Partner Country Numeric Code (4-digit) | `STRING` | `fact_imp_val`, `fact_imp_qty` |
| **GEN\_VAL\_MO** | Monthly Import Value (Primary Measure) | `INTEGER` | `fact_imp_val` |
| **ALL\_VAL\_MO** | Monthly Export Value (Primary Measure) | `INTEGER` | `fact_exp_val` |
| **TIME** | Trade period (Combined YEAR + MONTH) | `DATE` | All Fact Tables (Partitioning Key) |
| **total\_imp** | Consolidated Monthly Import Value (in `fact_imp_val2`) | `INTEGER` | `fact_imp_val2` |
| **UNIT\_QY1** | Unit of Measurement (e.g., KG, NO) | `STRING` | `fact_imp_qty` |

-----

## 🔗 Data Relationships

> 🔗 See [`docs/05_data_relationship.md`](https://www.google.com/search?q=docs/05_data_relationship.md)

| Relationship | Description |
| :--- | :--- |
| `fact_imp_val2` → `dim_hts_all` | Connects trade data to product classification. |
| `dim_hts_all` → `bridge_sec232` | Links products to Section 232 coverage. |
| `bridge_sec232` → `dim_section232_category` | Groups products by policy category. |
| `fact_imp_val2` → `bridge_reciprocal` | Enables analysis under Reciprocal Tariffs. |

Full ER Diagram: [`diagrams/erd_bigquery_model.png`](https://www.google.com/search?q=diagrams/erd_bigquery_model.png)

-----

## 🗓 Data Roadmap

> 🔗 See [`docs/06_data_roadmap.md`](https://www.google.com/search?q=docs/06_data_roadmap.md)

| Phase | Focus | Status |
| :--- | :--- | :--- |
| **Phase 1** | Section 232 Integration | 🔄 In Progress |
| **Phase 2** | Reciprocal Tariff (Annex II & III) | 🔄 In Progress |

-----

## 👤 Author

**Natdanai Tapanwong**
Data Analyst / Visualization Developer
Office of Commercial Affairs, Royal Thai Embassy (Washington D.C.)
📧 natdanai.tapanwong@gmail.com

-----

## 🪪 License
