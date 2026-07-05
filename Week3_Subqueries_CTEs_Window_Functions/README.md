# Week 3 – Subqueries, CTEs, and Window Functions

**Celebal Technologies | Data Engineering Internship**

## Objective

Analyze sales data from the Superstore dataset using advanced SQL concepts —
Subqueries, CTEs, and Window Functions — to answer real business questions
about customer behavior and sales performance.

---

## Dataset

**Sample Superstore** — a retail sales dataset widely used for business analytics.

| Detail | Value |
|---|---|
| Total rows | 9,994 |
| Unique customers | 793 |
| Unique orders | 5,009 |
| Unique products | 1,862 |
| Columns | 21 (order info, customer info, product info, sales metrics) |

---

## Tools Used

- **MySQL 8.0.46** — database and query execution
- **Jupyter Notebook + ipython-sql** — queries written with `%%sql`, output visible inline
- **pandas + SQLAlchemy** — used in setup to bulk-load the CSV into MySQL via `to_sql()`

---

## Database Setup

Created a dedicated `superstore` database with 3 derived tables:

```
superstore_raw          ← full CSV loaded via pandas to_sql()
      │
      ├── customers     ← 793 rows  (GROUP BY customer_id to avoid DISTINCT duplicates)
      ├── products      ← 1,862 rows (GROUP BY product_id)
      └── orders        ← 9,994 rows (one row per order-product line item)
```

**Why GROUP BY instead of SELECT DISTINCT for customers/products?**
Using `SELECT DISTINCT` across multiple columns (name, city, state, segment) created
duplicate rows for the same customer_id appearing with minor variations across orders —
793 unique customers ballooned to 4,910 rows. `GROUP BY customer_id` with `MIN()` on
other columns correctly collapsed them to one row per customer.

---


### Step 2 — Subqueries (Query 1–2)
- Orders with sales above the overall average
- Highest sales order per customer using a correlated subquery

### Step 2 — CTEs (Query 3–4)
- Total sales per customer computed with a named CTE
- Customers above average total sales — CTE referenced twice in one query,
  which is the main advantage over subqueries (write once, use many times)

### Step 2 — Window Functions (Query 5–7)
- `RANK()` to rank customers by total sales globally
- `ROW_NUMBER() + PARTITION BY` to number orders chronologically per customer
- Top 3 customers filtered using a ranked subquery

### Step 3 — Final Combined Query
One query combining JOIN + CTE + Window Function:
- CTE 1 aggregates total sales per customer
- CTE 2 applies `RANK()` on those totals
- Final SELECT JOINs with customers table to resolve name

### Mini Project — Business Insight Queries (Q1–Q5)
Real business questions answered end-to-end:

| Question | Approach |
|---|---|
| Top 5 customers | CTE + RANK() + JOIN |
| Bottom 5 customers | CTE + RANK() ASC + JOIN |
| Single-order customers | CTE + COUNT(DISTINCT order_id) |
| Above-average sales customers | CTE + correlated subquery on AVG |
| Highest order value per customer | Two CTEs + RANK() OVER PARTITION BY |

---

## How to Run

1. Make sure MySQL 8.0+ is running and the `superstore` database exists:
   ```sql
   CREATE DATABASE IF NOT EXISTS superstore;
   ```
2. Place `Superstore.csv` 
3. Open `superstore_analysis.ipynb` in Jupyter
4. Run the connection cell first — it will prompt for your MySQL password via `getpass`
5. Run the setup cells in order (they load the CSV and create the 3 derived tables)
6. All query cells can then be run top to bottom with output inline

**Requirements:**
```bash
pip install ipython-sql sqlalchemy mysql-connector-python prettytable==2.5.0
```

---

## Folder Structure

```
Week3_Subqueries_CTEs_Window_Functions/
├── superstore_analysis.ipynb    ← all queries with inline output
├── Superstore.csv
└── README.md
```
