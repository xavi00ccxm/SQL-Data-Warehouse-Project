# 🏷️ Naming Conventions

This guide defines the standard naming rules for **schemas**, **tables**, **views**, **columns**, and **stored procedures** used in the data warehouse.

---

## 🧭 Table of Contents
1. [General Standards](#-general-standards)  
2. [Table Naming Rules](#-table-naming-rules)  
   - [Bronze Layer](#-bronze-layer)  
   - [Silver Layer](#-silver-layer)  
   - [Gold Layer](#-gold-layer)  
3. [Column Naming Standards](#-column-naming-standards)  
   - [Surrogate Keys](#surrogate-keys)  
   - [Technical Columns](#technical-columns)  
4. [Stored Procedures](#%EF%B8%8F-stored-procedures)  

---

## 🪶 General Standards

> ### 🔹 Naming Format  
> Use `snake_case` — all lowercase with underscores separating words.  
>
> ### 🔹 Language  
> Use **English** for all object names.  
>
> ### 🔹 Reserved Words  
> Avoid SQL reserved words and system keywords.  
>
> ### 🔹 Consistency  
> Keep names short, clear, and consistent across all layers to improve traceability and automation.

---

## 🧱 Table Naming Rules

### 🥉 **Bronze Layer**  
This layer mirrors **raw data** from source systems, with no transformations applied.

> **Pattern:**  
> ```sql
> <source_system>_<entity>
> ```
>
> **Rules:**  
> - Begin with the **source system name** (e.g., `crm`, `erp`).  
> - Use the **original source table name** without renaming.  
> - Preserve raw integrity — no transformation or business logic.  
>
> **Example:**  
> ```sql
> crm_customer_info
> ```
> → Customer information extracted from the CRM system.

> **💡 Tip:** Bronze is your “single source of truth” — keep it raw, consistent, and traceable.

---

### 🥈 **Silver Layer**  
The Silver layer standardizes, cleanses, and enriches Bronze data for analytics.

> **Pattern:**  
> ```sql
> <source_system>_<entity>
> ```
>
> **Rules:**  
> - Keep the `<source_system>` prefix for lineage clarity.  
> - Merge or enrich only when necessary for analytics.  
> - Maintain clear references to Bronze origins.  
>
> **Example:**  
> ```sql
> erp_sales_orders
> ```
> → Cleaned and standardized sales order data from the ERP source.

> **🧩 Includes:**  
> - `SCD2` (Slowly Changing Dimensions Type 2) for historical accuracy.  
> - `DQ Checks` (Data Quality Validation) to detect nulls, duplicates, or invalid values.

---

### 🥇 **Gold Layer**  
The Gold layer contains **business-ready data** — aggregated, integrated, and optimized for reporting.

> **Pattern:**  
> ```sql
> <category>_<entity>
> ```
>
> **Rules:**  
> - Use clear prefixes to describe the table type:
>   - `dim_` → Dimension tables  
>   - `fact_` → Fact tables  
>   - `report_` → Summary or report-ready tables  
> - The `<entity>` should represent a **business concept** (e.g., sales, customers).  
>
> **Examples:**  
> ```sql
> dim_product
> fact_revenue
> report_monthly_sales
> ```
>
> **🧠 Notes:**  
> - Gold tables are designed for **fast Power BI analytics**.  
> - Support **RLS (Row-Level Security)** for controlled access.  
> - Include **aggregated tables** for improved performance.

---

### 🗂 **Glossary of Common Prefixes**

| Prefix | Meaning | Example |
|--------|----------|----------|
| `dim_` | Dimension table | `dim_customer`, `dim_product` |
| `fact_` | Fact or transaction table | `fact_sales`, `fact_orders` |
| `report_` | Reporting or summary table | `report_kpi_summary` |

---

## 🧩 Column Naming Standards

### 🔑 Surrogate Keys
> All surrogate keys or primary keys should end with `_key`.

**Pattern:**  
```sql
<table_name>_key
```

**Example:**  
```sql
customer_key
```
→ Surrogate key for the `dim_customer` table.

---

### ⚙️ Technical Columns
> Metadata and system-generated columns must begin with the prefix `dwh_`.

**Pattern:**  
```sql
dwh_<column_name>
```

**Rules:**  
- `dwh_` prefix marks system-managed columns (not from source data).  
- Always describe their function clearly.  

**Examples:**  
```sql
dwh_load_date   -- Date record was loaded
dwh_update_user -- User or process that last modified the record
```

---

## ⚡️ Stored Procedures
Stored procedures manage the **ETL / ELT** flow between layers and must follow a uniform pattern.

**Pattern:**  
```sql
load_<layer>
```

**Examples:**  
```sql
load_bronze  -- Ingests raw data from CSV or APIs
load_silver  -- Cleanses and standardizes Bronze data
load_gold    -- Aggregates and publishes for reporting
```

> **✅ Tip:** Use consistent naming for easier automation in deployment pipelines and scheduled jobs.

---

### ✅ Summary

Following these conventions ensures:
- 🔄 Consistent naming across all layers  
- 🧩 Simplified automation for ETL pipelines  
- 📊 Seamless integration with BI tools like Power BI  
- 📁 Readable, professional data architecture documentation

---

> **Created by:** Otto X. DeLaRocha H  
> *SQL Data Warehouse Project – Medallion Architecture*
