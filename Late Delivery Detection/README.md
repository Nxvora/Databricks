<div align="center">

<img src="https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white"/>
<img src="https://img.shields.io/badge/Delta%20Lake-003366?style=for-the-badge&logo=delta&logoColor=white"/>
<img src="https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white"/>

# 🚚 Late Delivery Detection using Databricks

</div>

---

## ❗ The Problem

Every morning, an operations team receives a CSV file with the day's orders.
They **manually scan** each row to find which deliveries were late.

This is slow, error-prone, and doesn't scale.

---

## ✅ The Solution

A simple **4-step Databricks pipeline** that:
1. Reads the raw order data
2. Flags every order as `LATE` or `ON TIME` (rule: more than 5 days = late)
3. Saves the result as a **Delta table** (versioned, queryable like a database)
4. Produces a clean category-level report for the ops team

```
Raw Orders CSV
      │
      ▼
  Flag Late Orders
  (days_to_deliver > 5)
      │
      ▼
  Save to Delta Table
      │
      ▼
  Category Report
  (which dept is worst?)
```

---

## 📊 Sample Output

**Step 2 — Flagged orders**

| order_id | customer | category    | days_to_deliver | delivery_status |
|----------|----------|-------------|-----------------|-----------------|
| ORD002   | Bob      | Clothing    | 7               | 🔴 LATE         |
| ORD003   | Carol    | Groceries   | 1               | 🟢 ON TIME      |
| ORD004   | David    | Electronics | 8               | 🔴 LATE         |
| ORD005   | Eva      | Home        | 2               | 🟢 ON TIME      |

**Step 4 — Category report**

| category    | total_orders | late_orders | avg_days | worst_case_days |
|-------------|-------------|-------------|----------|-----------------|
| Electronics | 8           | 6           | 7.4      | 9               |
| Clothing    | 6           | 4           | 6.8      | 9               |
| Home        | 5           | 3           | 6.0      | 9               |
| Groceries   | 6           | 0           | 1.3      | 2               |

> **Insight found instantly:** Electronics and Clothing are the problem categories. Groceries always delivers on time.

---

## 🗂️ Files

```
├── late_delivery_detection.py   ← The entire pipeline (4 cells)
└── README.md
```

---

## 🚀 How to Run

1. Go to [community.cloud.databricks.com](https://community.cloud.databricks.com) — free account
2. Create any cluster (Runtime 12.x or above)
3. **Workspace → Import → File** → upload `late_delivery_detection.py`
4. Click **Run All**

No installs. No config. No external data needed — data is built into the notebook.

---

## 💡 Why Databricks for this?

| Without Databricks | With Databricks |
|---|---|
| Manual CSV scanning | Automated flagging in seconds |
| No history of past results | Delta table keeps full version history |
| Breaks on large files | Same code works on 25 rows or 25 million |
| Hard to share with team | Notebook lives in workspace, shareable instantly |

---

## 🔌 Using real data? One line change

Replace the hardcoded list in Step 1 with:

```python
df_raw = spark.read.format("csv").option("header", True).load("s3://your-bucket/orders/")
```

Everything else works without touching.

---

<div align="center">
<i>⭐ Star this repo if it helped you!</i>
</div>
