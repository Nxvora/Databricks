# Challenge 01: Handling Changing Data (Upserts) in the Data Lake

## 🏢 The Real-World Business Problem
When ingesting daily data from operational databases (like MySQL or Oracle) into a Data Lake, data is never static. Customers update their profiles, and order statuses change. 

If a data pipeline simply *appends* daily data dumps into the lake, it creates massive duplication. A single customer might appear 5 times with 5 different addresses. This destroys the accuracy of downstream Power BI/Tableau reports, leading to executives making decisions on inflated, false numbers.

## 🛠️ Our Enterprise Technical Solution
To maintain a "Single Source of Truth", we do not append; we **MERGE**. 

Using **Azure Databricks** and the **Delta Lake** format, our engineering team implements ACID-compliant `MERGE` operations (Upserts). 

**How this code works:**
1. We load the existing Data Lake table (Target).
2. We load the new daily changes (Source).
3. We execute a PySpark Delta `MERGE`.
4. **WHEN MATCHED:** If the CustomerID already exists, we *update* their details.
5. **WHEN NOT MATCHED:** If it's a new CustomerID, we *insert* the new record.

**Business Value:** Zero duplicate records, perfectly accurate BI dashboards, and optimized cloud storage costs.

## 🚀 Running this Code
This PySpark notebook is ready to be imported directly into any Azure Databricks workspace. It generates synthetic data to demonstrate the MERGE functionality safely without requiring external database connections.
