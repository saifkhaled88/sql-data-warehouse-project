# 🏗️ Oracle Data Warehouse Project  
### Bronze → Silver → Gold Medallion Architecture with Oracle SQL

A modern **Data Warehouse ETL project** built using **Oracle SQL**, following the **Medallion Architecture** pattern:

- 🥉 **Bronze Layer** → Raw source data ingestion  
- 🥈 **Silver Layer** → Data cleansing and transformation  
- 🥇 **Gold Layer** → Star schema for analytics and reporting  

This project demonstrates practical **data engineering** concepts including:

- ETL pipeline design
- Data cleansing & standardization
- Multi-layer warehouse architecture
- Surrogate key generation
- Dimension & Fact modeling
- Oracle SQL development for analytics-ready data

---

# 🥉 Bronze Layer (Raw Data)

The **Bronze layer** stores raw source data exactly as it arrives from the source systems, with little to no transformation.

## 📌 Tables
- `crm_cust_info`
- `crm_prd_info`
- `crm_sales_details`
- `erp_loc_a101`
- `erp_cust_az12`
- `erp_px_cat_g1v2`

## 🎯 Purpose
- Preserve original source data
- Support traceability and auditing
- Act as the landing zone for ingestion

---

# 🥈 Silver Layer (Cleansed & Transformed Data)

The **Silver layer** applies cleansing, validation, and transformation rules to make the data consistent, standardized, and analytics-ready.

## 🔄 Key Transformations Performed

### 👤 CRM Customer Data
- Trimmed whitespace from names
- Standardized marital status values:
  - `S` → `SINGLE`
  - `M` → `MARRIED`
- Standardized gender values:
  - `M` → `MALE`
  - `F` → `FEMALE`
- Replaced unknown values with `n/a`

---

### 📦 CRM Product Data
- Extracted `category_id` from product key
- Extracted normalized product key
- Converted zero product cost to `NULL`
- Standardized product line values:
  - `M` → `Mountain`
  - `R` → `Road`
  - `S` → `Other Sales`
  - `T` → `Touring`
- Calculated `prd_end_dt` using `LEAD()` for slowly changing product records

---

### 💰 CRM Sales Data
- Converted integer date fields (`YYYYMMDD`) into Oracle `DATE`
- Invalid dates converted to `NULL`
- Recalculated sales amount when invalid or inconsistent
- Recalculated unit price when missing or invalid

---

### 🧑 ERP Customer Data
- Removed `NAS` prefix from customer IDs when present
- Nullified future birthdates
- Standardized gender values

---

### 🌍 ERP Location Data
- Removed dashes from customer IDs
- Standardized country values:
  - `DE` → `Germany`
  - `US` / `USA` → `United States`
- Replaced missing values with `n/a`

---

### 🏷️ ERP Product Category Data
- Reformatted product category IDs into a standardized pattern

---

# 🥇 Gold Layer (Analytics / Star Schema)

The **Gold layer** transforms the cleansed data into a **Star Schema** optimized for business intelligence and reporting.

## 📌 Objects Created
- `dim_customers`
- `dim_products`
- `fact_sales`

---

# ⭐ Star Schema Design

## `dim_customers`
The **Customer Dimension** contains:

- `customer_key` *(Surrogate Key)*
- Customer business ID / number
- First name
- Last name
- Country
- Marital status
- Gender
- Birthdate
- Account creation date

---

## `dim_products`
The **Product Dimension** contains:

- `product_key` *(Surrogate Key)*
- Product business ID / number
- Product name
- Category
- Subcategory
- Maintenance classification
- Product cost
- Product line
- Product start date

---

## `fact_sales`
The **Sales Fact Table** contains:

- Order number
- `product_key` *(FK)*
- `customer_key` *(FK)*
- Order date
- Shipping date
- Due date
- Sales amount
- Quantity
- Price

---

# 🔄 ETL Flow Summary

## 1️⃣ Bronze
Load raw data from CRM and ERP source systems.

## 2️⃣ Silver
Apply cleansing and transformation rules:

- Standardize formats
- Fix invalid values
- Handle missing data
- Normalize keys
- Enrich source data

## 3️⃣ Gold
Build an analytics-ready dimensional model:

- Create dimensions
- Generate surrogate keys
- Build fact table
- Enable reporting and BI use cases

---

# 📂 Project Structure

```text
data-warehouse-oracle-sql/
│
├── 01_create_bronze_tables.sql
├── 02_build_silver_layer.sql
├── 03_build_gold_layer.sql
│
└── README.md

If using views instead of physical Gold tables:

├── 03_build_gold_views.sql
🛠️ Technologies Used

Oracle SQL

PL/SQL (for procedural table creation when needed)

Window Functions

ROW_NUMBER()

LEAD()

Data Warehouse Design

Star Schema Modeling

ETL / ELT Concepts

Medallion Architecture

💡 Key Data Engineering Concepts Demonstrated

This project showcases practical understanding of:

Layered data architecture

Data quality checks

Data standardization

Business rule implementation

Surrogate key generation

Dimension / Fact modeling

Joining multi-source systems

Preparing data for BI & analytics

🚀 How to Run
Step 1 — Create Bronze Layer

Run:

01_create_bronze_tables.sql
Step 2 — Build Silver Layer

Run:

02_build_silver_layer.sql
Step 3 — Build Gold Layer

Run:

03_build_gold_layer.sql

Or, if you are using views instead of physical Gold tables:

03_build_gold_views.sql
📊 Example Use Cases

This warehouse can support analytical questions such as:

Total sales by product category

Sales by country

Customer segmentation by gender and marital status

Product performance analysis

Sales trends over time

Active product catalog reporting

🔍 Example Analytical Query
SELECT 
    dp.category,
    SUM(fs.sales_amount) AS total_sales
FROM fact_sales fs
JOIN dim_products dp
    ON fs.product_key = dp.product_key
GROUP BY dp.category
ORDER BY total_sales DESC;
📈 Future Improvements

Potential next enhancements for this project:

Add incremental loading

Implement SCD Type 2 for dimensions

Add indexes for performance optimization

Create materialized views

Add data validation checks

Build Oracle Scheduler jobs

Connect to Power BI or Tableau

Add data lineage documentation

👨‍💻 Author
Seif Khaled

Computer Science Student (AI Major) | Aspiring Data Engineer / Backend Developer

This project was built to strengthen practical skills in:

Data Engineering

SQL Development

ETL Pipeline Design

Data Warehouse Modeling

🌟 Why This Project Matters

This project reflects a real-world approach to building a structured data warehouse from operational systems using Oracle SQL.

It demonstrates the ability to:

Work with messy source data

Apply business transformation rules

Design a layered warehouse architecture

Build an analytics-ready star schema

If you're interested in Data Engineering, ETL, or Analytics Engineering, this project provides a strong foundational implementation of core warehouse concepts.

📬 Feedback

If you have suggestions, improvements, or want to discuss data engineering ideas, feel free to connect or open an issue.

⭐ If you like this project, consider starring the repository!
