# 🏨 Hotel Booking Analysis – Data Warehouse & Business Intelligence

End-to-end **Data Warehousing and Business Intelligence** project built on a Hotel Booking
Analytics dataset (~50,000 review transactions, 2020–2025). The project takes raw OLTP-style
data all the way through ETL, dimensional modeling, OLAP cube design, and interactive BI
reporting.

> Built for **IT3021 – Data Warehousing and Business Intelligence**, SLIIT (Assignments 1 & 2).

---

## 📌 Project Overview

| | |
|---|---|
| **Dataset** | Hotel Booking Analytics — ~50,000 reviews, 25 hotels, 25 cities, 25 countries (2020–2025) |
| **Source formats** | CSV (Hotels, Reviews), Excel (Users) |
| **Schema** | Star Schema — `Fact_Reviews` + `Dim_Hotel`, `Dim_User` (SCD Type 2), `Dim_Date`, `Dim_Location` |
| **ETL** | SQL Server Integration Services (SSIS) |
| **OLAP Cube** | SQL Server Analysis Services (SSAS Multidimensional) |
| **Reporting** | Excel PivotTables (OLAP ops) + Power BI (Live Connection & Import) |

---

## 🧱 Architecture

```
hotel_v1.csv ─┐
reviews_v1.csv├──► Staging Tables ──► SSIS ETL ──► Data Warehouse (Star Schema) ──┬──► SSAS Cube ──► Excel (OLAP)
users_v1.xlsx ┘                                                                   └──► Power BI Reports
```

1. **Data Sources** – CSV (Hotels, Reviews) + Excel (Users), simulating heterogeneous OLTP feeds
2. **Staging Area** – `Staging_Hotel`, `Staging_Users`, `Staging_Reviews`
3. **ETL Layer** – SSIS packages: `Load_Staging.dtsx`, `Load_DW.dtsx`, `Update_Fact_Completion.dtsx`
4. **Data Warehouse (Hotel_DW)** – Star schema with surrogate keys
5. **BI Layer** – SSAS cube, Excel OLAP, Power BI dashboards

---

## 📂 Assignment 1 — Data Warehouse Design & ETL

- Identified the dataset as an **OLTP** source and justified its suitability for warehousing
- Designed a **Star Schema**:
  - `Fact_Reviews` (review scores: overall, cleanliness, comfort, facilities, location, staff, value-for-money)
  - `Dim_Hotel`, `Dim_User` (SCD Type 2 with `start_date`/`end_date`/`is_current`), `Dim_Date`, `Dim_Location`
- Implemented schema with SQL Server `CREATE TABLE` scripts (PKs, FKs, surrogate keys)
- Built **SSIS ETL packages**:
  - Load raw CSV/Excel data into staging tables
  - Transform & load dimension tables (with lookups, data conversion, derived columns)
  - Load the fact table with referential integrity via Lookup transformations
- Extended `Fact_Reviews` into an **Accumulating Snapshot Fact Table**:
  - `accm_txn_create_time`, `accm_txn_complete_time`, `txn_process_time_hours`
  - Simulated a separate "completion dataset" and updated the fact table via a dedicated SSIS package
  - Calculated processing duration using `DATEDIFF`
- Row-count and integrity validation performed at each ETL stage

---

## 📊 Assignment 2 — SSAS Cube & Power BI Reports

### SSAS Multidimensional Cube (`HotelReviews_Cube`)
- Data Source & Data Source View (DSV) built on `Hotel_DW`
- Dimensions with hierarchies:
  - **Dim_Date**: Year → Month → Day
  - **Dim_Hotel**: Country → City → Hotel Name
  - **Dim_Location**: Continent → Region → Country → City
  - **Dim_User**: Gender, Age Group, Traveller Type
- 7 score measures + Review Count + calculated measure **Avg Overall Score** (MDX)
- Deployed and processed (Process Full) for querying

### OLAP Operations (Excel PivotTables via SSAS connection)
- **Roll-Up** — monthly → yearly aggregation
- **Drill-Down** — Year → Month → Day
- **Slice** — single-dimension filter (e.g., Country = Japan)
- **Dice** — multi-dimension filter (Country + Traveller Type + Year)
- **Pivot** — swapping row/column axes

### Power BI Reports (Live Connection / Import + DAX)
- **Report 1 – Matrix Visual**: hierarchical Country → Hotel rows, Year columns, conditional formatting
- **Report 2 – Cascading Slicers**: Country → Hotel cascading filters + bar/donut/line charts
- **Report 3 – Drill-Down Hierarchy**: Year → Month → Day drill-down with geography comparison
- **Report 4 – Drill-Through**: Summary table → per-hotel detail page with KPI cards and trend charts

DAX measures: `Total Reviews`, `Avg Overall Score`, `Avg Cleanliness`, `Avg Comfort`, `Avg Staff Score`, `Avg Value`, `Avg Location`

---

## 🗂️ Repository Structure

```
├── DataWarehouse/        # SQL scripts: staging, dimension & fact table DDL
├── ETL/                  # SSIS project (.dtsx packages, connection managers)
├── CubeProject/          # SSAS Multidimensional project (.dim, .cube, MDX calculations)
├── Excel/                # Excel PivotTable workbook demonstrating OLAP operations
├── PowerBIReports/        # .pbix file with Matrix, Slicers, Drill-Down & Drill-Through reports
├── Docs/                 # Assignment 1 & 2 submission reports (PDF)
└── README.md
```

---

## 🛠️ Tools & Technologies

`SQL Server` · `SSIS (Visual Studio / SSDT)` · `SSAS Multidimensional` · `MDX` · `Microsoft Excel` · `Power BI Desktop` · `DAX`

---

## 👤 Author

**R A S R Harischandra**
Student ID: IT23194908 · Batch: Y3.S2.DS.WE.02.02
Sri Lanka Institute of Information Technology (SLIIT)
