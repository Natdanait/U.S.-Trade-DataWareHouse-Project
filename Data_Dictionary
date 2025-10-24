📘 Data Dictionary: Fact Tables

The Fact Tables contain the core quantitative measures of U.S. merchandise trade, classified by country and HTS product code.

🔹 fact_imp_val — U.S. Monthly Import Value

This table records monthly import values of U.S. goods, classified by partner country and HTS10 product code.

🧱 Schema Definition
Field Name	Type	Description
CTY_CODE	STRING	Country numeric code (4-digit) representing the trading partner country.
CTY_NAME	STRING	Full name of the trading partner country.
I_COMMODITY	STRING	10-digit Harmonized Tariff Schedule (HTS) code identifying the imported product.
I_COMMODITY_LDESC	STRING	Long product description.
GEN_VAL_MO	INTEGER	Monthly import value (USD) — primary measure field.
YEAR	INTEGER	Reporting year of trade.
MONTH	INTEGER	Reporting month (1–12).
TIME	DATE	Combined date constructed from YEAR + MONTH; used for partitioning.
TradeType	STRING	Trade type indicator — always 'Import'.
load_timestamp	TIMESTAMP	Timestamp of ETL load event into BigQuery.
⚙️ Metadata Notes

Partitioning: TIME (monthly)

Clustering: CTY_CODE, I_COMMODITY (uses natural string keys for clustering)

Source: U.S. Census Bureau official trade datasets

Purpose: Import-value analysis, partner performance, and policy segmentation

🔹 fact_imp_qty — U.S. Monthly Import Quantity

This table tracks monthly import quantities by partner country and HTS10 product.

🧱 Schema Definition
Field Name	Type	Description
CTY_CODE	STRING	Country numeric code.
CTY_NAME	STRING	Partner country name.
I_COMMODITY	STRING	10-digit HTS product code.
UNIT_QY1	STRING	Unit of measurement for Quantity 1 (e.g., KG, NO, L).
UNIT_QY2	STRING	Unit of measurement for Quantity 2, if applicable.
GEN_QY1_MO	INTEGER	Monthly quantity value (unit 1).
GEN_QY2_MO	INTEGER	Monthly quantity value (unit 2).
YEAR	INTEGER	Calendar year.
MONTH	INTEGER	Calendar month.
TIME	DATE	Combined YEAR + MONTH; used for partitioning.
TradeType	STRING	Trade type — 'Import'.
load_timestamp	TIMESTAMP	Timestamp of ETL load into BigQuery.
⚙️ Metadata Notes

Partitioning: TIME (monthly)

Clustering: CTY_CODE, I_COMMODITY

Source: U.S. Census Bureau official trade datasets

Purpose: Provides volume metrics complementing import value analysis

🔹 fact_imp_val2 — Consolidated Import Value & Quantity

This table unifies value and quantity metrics into one structure for streamlined analysis.

🧱 Schema Definition
Field Name	Type	Description
CTY_CODE	STRING	Country numeric code.
I_COMMODITY	STRING	10-digit HTS product code.
total_imp	INTEGER	Monthly import value (USD).
total_qty	INTEGER	Monthly import quantity (in product-specific units).
Description	STRING	Measurement unit (e.g., Kilogram, Number).
YEAR	INTEGER	Reporting year.
MONTH	INTEGER	Reporting month.
TIME	DATE	Combined YEAR + MONTH; used for partitioning.
⚙️ Metadata Notes

Partitioning: TIME (monthly)

Clustering: CTY_CODE, I_COMMODITY

Source: U.S. Census Bureau official trade datasets (merged value + quantity)

Purpose: Unified dataset for import analytics and policy impact evaluation

🔹 fact_exp_val — U.S. Monthly Export Value

This table stores monthly export values of goods shipped from the U.S., by partner country and HTS10 product.

🧱 Schema Definition
Field Name	Type	Description
CTY_NAME	STRING	Partner country name.
E_COMMODITY	STRING	10-digit HTS code identifying exported product.
E_COMMODITY_LDESC	STRING	Long product description.
E_COMMODITY_SDESC	STRING	Short product description for compact labeling.
ALL_VAL_MO	INTEGER	Monthly export value (USD).
YEAR	INTEGER	Reporting year.
MONTH	INTEGER	Reporting month.
COMM_LVL	STRING	Commodity classification level (e.g., HS10).
TIME	DATE	Combined YEAR + MONTH for partitioning.
TradeType	STRING	Trade flow indicator — 'Export'.
⚙️ Metadata Notes

Partitioning: TIME (monthly)

Clustering: E_COMMODITY, CTY_NAME

Source: U.S. Census Bureau official trade datasets (Exports)

Purpose: Analyzes export performance, destinations, and trade balance

✅ Status: Finalized — ready to commit under
docs/04_data_dictionary.md → Fact Tables Section
