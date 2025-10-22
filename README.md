# U.S. Trade Data Architecture & Analytics

This repository presents the **data foundation** for analyzing the international trade of the **United States**.  
It integrates official U.S. trade data and policy references into a single analytical environment — designed for trade analysts, researchers, and policymakers who seek to understand trade trends, performance, and policy impacts clearly and efficiently.

---

## 🌍 Project Overview

The **U.S. Trade Data Architecture** is built to answer the most fundamental questions about U.S. international trade, such as:

- Has U.S. **import or export** increased or decreased this year?  
- What are the **top imported or exported products**?  
- Which countries are the **main trading partners**?  
- How have trade **trends changed over time**?

These represent the system’s **core analytical capabilities**, providing accurate, consistent, and ready-to-use insights on the structure and evolution of U.S. trade at both the **product** and **country** levels.

---

Over time, the database has evolved beyond basic statistics.  
By integrating **additional features** and **policy-specific dimension tables**, the system now supports **deeper, policy-driven analysis**, including:

- **Agricultural trade focus** (HS Chapters 01–23 & 40)  
- **Section 232 Tariff coverage** (Iron, Steel, Aluminum, Vehicles, etc.)  
- **Reciprocal Tariff framework** (Annex II & III lists)  
- **Country and product-level impact evaluations** under specific trade measures  

These enhancements make it possible to perform advanced analysis more **accurately**, **rapidly**, and **consistently** — for example:

> *“How have Section 232 tariffs affected imports of steel from ASEAN countries?”*  
> *“Which agricultural products are showing sustained import growth?”*

Together, these features form a **scalable and modular architecture** that grows with new data sources, trade policies, and analytical needs.

---

### Key Highlights
- Multi-level **HTS hierarchy** (10 → 8 → 6 → 4 → 2 digits)  
- Combines **monthly and year-to-date (YTD)** trade trends  
- Integrated **policy filter tables** (Section 232, Reciprocal, etc.)  
- Enables both **high-level monitoring** and **deep-dive policy evaluation**  
- Designed for **international trade analysts** and **economic policymakers**

---

## 🧭 Data Journey

> 🔗 See [`docs/01_data_journey.md`](docs/01_data_journey.md)

| Stage | Description |
|-------|--------------|
| **1. Data Source** | Monthly import/export data from the **U.S. Census Bureau** |
| **2. Ingestion** | Extracted via **Google Colab** and uploaded to **Google Cloud Storage (GCS)** |
| **3. Storage** | Raw parquet files organized by year, month, and trade type |
| **4. Data Warehouse** | **BigQuery** transforms raw data into clean fact and dimension tables |
| **5. Analytics** | **Power BI** connects to BigQuery for visualization, DAX modeling, and policy dashboards |

---

### 🗺️ Data Pipeline Overview

![Data Pipeline](diagrams/Data%20pipeline%20Diagram.drawio.png)

**Flow Summary**

1. **U.S. Census Data** is extracted through Google Colab and uploaded to **Google Cloud Storage (GCS)**.  
2. **GCS** acts as the central repository for raw monthly trade files (parquet format).  
3. **BigQuery** ingests and transforms data into structured **fact** and **dimension** tables (e.g., `fact_imp_val2`, `dim_hts10`, `bridge_sec232`).  
4. **Power BI** connects directly to BigQuery for visual exploration and policy dashboards.

---

## 🧱 Database Architecture

> 🔗 See [`docs/02_database_architecture.md`](docs/02_database_architecture.md)

**Core Components**
- **Google BigQuery** – Main data warehouse for large-scale analytics  
- **MySQL (Local)** – Sandbox environment for quick queries and local testing  
- **Python ETL (Colab / Composer)** – Automated ingestion and transformation  
- **Power BI Service** – Interactive dashboard and reporting layer  

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
- Separate environments for **Staging**, **Production**, and **Analytics**  
- Access controlled via **service accounts** (no personal credentials)  
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
| `Reciprocal_Flag` | Product listed under Reciprocal Tariff | BOOLEAN | Policy Dataset |

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
| **Phase 1** | Section 232 Integration | ✅ Completed |
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
# U.S. Trade Data Architecture & Analytics

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
