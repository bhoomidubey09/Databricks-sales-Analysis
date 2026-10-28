# 🛍️ Retail Sales Analysis | Databricks Medallion Architecture

<p align="center">
  <img src="https://img.shields.io/badge/Databricks-Data_Engineering-EF3E2C?style=for-the-badge&logo=databricks&logoColor=white" alt="Databricks"/>
  <img src="https://img.shields.io/badge/PySpark-Transformations-FDEE21?style=for-the-badge&logo=apachespark&logoColor=black" alt="PySpark"/>
  <img src="https://img.shields.io/badge/Delta_Lake-Data_Storage-00A1E0?style=for-the-badge" alt="Delta Lake"/>
  <img src="https://img.shields.io/badge/GitHub-Version_Control-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"/>
</p>

<p align="center">
  <b>An end-to-end retail data engineering project built with Databricks and PySpark.</b>
  <br/>
  Transforming messy retail orders and customer data into structured, analytics-ready datasets.
</p>

---

## 📌 Project Overview

This project implements the **Medallion Architecture in Databricks** to process retail orders and customer datasets containing inconsistent values, duplicate records, mixed date formats, and data quality issues.

Using PySpark and Delta tables, I built a data pipeline that ingests raw CSV files, cleans and standardizes the data, integrates customer and order information, and prepares aggregated datasets for retail sales analysis.

The project also includes a retail sales dashboard that explores category revenue, daily revenue trends, customer gender distribution, and top products by revenue.

## 📊 My Retail Sales Dashboard

### Dashboard 1 — Category Revenue & Daily Sales Trends

<img width="1896" height="850" alt="Retail sales Dashboard1" src="https://github.com/user-attachments/assets/6e3f6eae-abda-4be5-a2b4-18e68fe4dd0c" />


### Dashboard 2 — Customer Analysis & Top 10 Products

<img width="1885" height="893" alt="Retail sales Dashboard2" src="https://github.com/user-attachments/assets/b7f5a6e2-9b59-441e-853e-be84c9b8ebf4" />


---

## 🏗️ Medallion Architecture

The project follows three data processing layers:

**Bronze → Silver → Gold**

### 🥉 Bronze Layer — Raw Data Ingestion

**Objective:** Ingest the original datasets into Databricks and preserve the source data as the starting point for processing.

**What I implemented:**
- Loaded the retail orders and customer CSV files from Databricks Volumes.
- Defined explicit PySpark schemas for both datasets.
- Read source data using PySpark DataFrames.
- Added an `ingest_time` timestamp to track data ingestion.
- Stored the datasets as Delta tables.

**Tables created:**
- `retail_project.bronze.bronze_order`
- `retail_project.bronze.bronze_customer`

### 🥈 Silver Layer — Data Cleaning & Transformation

**Objective:** Improve data quality and standardize inconsistent records before analysis.

**Orders data transformations:**
- Parsed order dates from multiple formats.
- Standardized category names, including `ELECTRONIC` to `ELECTRONICS`, `FASHON` to `FASHION`, and `HOME AND LIVING` to `HOME & LIVING`.
- Checked duplicate order IDs and removed duplicates.
- Handled missing discount values and the text value `ten`.
- Converted percentage discounts into decimal values.
- Handled negative quantities.
- Converted unit prices into decimal data types.
- Recalculated `total_amount` using quantity, unit price, and discount.

**Customer data transformations:**
- Trimmed unwanted spaces from customer fields.
- Removed duplicate customers using a window function and retained the most recently ingested record per customer ID.
- Standardized customer status to `ACTIVE`, `INACTIVE`, or `UNKNOWN`.
- Cleaned loyalty points and converted them into integer values.
- Validated email formats using a regular expression.
- Parsed date of birth and registration dates from multiple formats.
- Standardized gender values to `M`, `F`, or `U`.

**Tables created:**
- `retail_project.silver.silver_order`
- `retail_project.silver.silver_customer`

### 🥇 Gold Layer — Business-Ready Data

**Objective:** Combine the cleaned data and prepare it for reporting and business analysis.

**What I implemented:**
- Created a customer dimension containing customer ID, name, email, city, state, loyalty points, and status.
- Joined order data with customer data using `customer_id` and a left join.
- Enriched orders with customer and geographic attributes.
- Extracted `Year`, `Month`, and `Day` from the order date.
- Aggregated sales metrics using PySpark functions.

**Tables created:**
- `retail_project.gold.gold_dim_customer`
- `retail_project.gold.gold_fact_sales`

**Metrics calculated in the Gold dataset:**
- Total revenue
- Total quantity
- Distinct order count
- Average revenue
- Average quantity
- Average loyalty points
- Minimum unit price
- Maximum unit price

---

## 🔄 Data Pipeline

```mermaid
flowchart TD
    A["📁 Retail Orders CSV<br/>Retail Customers CSV"]
    B["🥉 Bronze Layer<br/>Raw Delta Tables"]
    C["🥈 Silver Layer<br/>Cleaning & Validation"]
    D["🥇 Gold Layer<br/>Customer Dimension & Sales Fact"]
    E["📊 Retail Sales Dashboard"]

    A --> B --> C --> D --> E

    style A fill:#E8F1FF,stroke:#3973AC,color:#16324F
    style B fill:#FCE8D5,stroke:#D97706,color:#7C2D12
    style C fill:#DDEBFF,stroke:#2563EB,color:#1E3A8A
    style D fill:#DDF7E8,stroke:#16A34A,color:#14532D
    style E fill:#EDE9FE,stroke:#7C3AED,color:#4C1D95
```

## 📈 Dashboard Insights & Visualizations

The dashboard screenshots show the following analyses:

| Visualization | What it shows |
|---|---|
| Revenue by Category | Compares total revenue across Electronics, Fashion, and Home & Living |
| Daily Revenue Trend | Tracks total revenue across order dates |
| Customer Data Table | Displays city-level customer and sales metrics |
| Total Orders by Gender | Shows the distribution of orders across gender categories, including unknown values |
| Top 10 Products by Revenue | Ranks products by their total revenue |

The dashboard also includes filters for **Year, Gender, and Category** to explore the data interactively.

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Databricks | Data engineering workspace |
| PySpark | Data cleaning, transformation, joins, and aggregation |
| Delta Lake | Structured data storage |
| Python | Data processing logic |
| GitHub | Source control and project documentation |

## 📂 Repository Structure

```text
Databricks-sales-Analysis/
│
├── Retail_sales_project_databricks.ipynb
├── dashboard.json
├── README.md
└── images/
    ├── retail-sales-dashboard-1.png
    └── retail-sales-dashboard-2.png
```

*The image filenames are used by this README. Upload your actual dashboard screenshots using these exact names. Keep `dashboard.json` if it is your exported dashboard definition.*

## 🚀 How to Run the Project

1. Clone the repository or connect it to Databricks using Git folders.
2. Upload the source CSV files to an accessible Databricks Volume.
3. Verify the catalog, schema, permissions, and file paths.
4. Run the notebook cells in order, starting with the Bronze layer.
5. Review the Silver tables and data quality transformations.
6. Generate the Gold customer dimension and sales fact table.
7. Open the dashboard in Databricks or import the dashboard definition into a compatible dashboard interface.

## 🎯 Key Learnings

- Implementing the Medallion Architecture in Databricks.
- Processing messy data with PySpark.
- Cleaning and standardizing real-world data.
- Using window functions for deduplication.
- Working with Delta tables and schemas.
- Joining customer and order datasets.
- Aggregating sales metrics for business intelligence.
- Building dashboard visualizations for retail analysis.

---

<p align="center">
  <b>Created by Bhoomi Dubey</b>
  <br/>
  <i>Data Science | Data Engineering | Retail Analytics</i>
  <br/><br/>
  <b>Built with ❤️ using Databricks and PySpark</b>
</p>
