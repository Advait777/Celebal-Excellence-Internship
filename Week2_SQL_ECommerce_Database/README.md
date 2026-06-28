# Week 2 – E-Commerce Sales Database (SQL)

**Celebal Technologies | Data Engineering Internship**


## Tools Used

- **MySQL 8.0.46** — query execution and transaction handling
- **Jupyter Notebook** — all 27 queries written with `%%sql` magic, output visible inline
- **ipython-sql + SQLAlchemy + mysql-connector-python** — MySQL ↔ Jupyter bridge

---

## Database Schema

4 tables, with FK relationships enforcing referential integrity throughout:

```
customers ──(1:N)──▶ orders ──(1:N)──▶ order_items ◀──(N:1)── products
```


**Indexes created:**
- `idx_customers_city`, `idx_customers_state`
- `idx_products_category`
- `idx_orders_date`, `idx_orders_status`
- `idx_customers_joindate` (added during Q12 analysis)

---

## Assignment Coverage

### Section A — SQL Basics (Q1–Q6)
Basic SELECT, column selection, DISTINCT, Primary Keys, constraints, and intentional constraint violations (UNIQUE on email, CHECK on unit_price).

### Section B — Filtering & Optimization (Q7–Q12)
WHERE clauses, multi-condition filters, BETWEEN, date filtering, index behavior, and SARGability. Q12 in particular explores why `YEAR(join_date) = 2024` prevents index usage and how to rewrite it properly.

### Section C — Aggregation (Q13–Q18)
COUNT, SUM, AVG, MIN, MAX with GROUP BY. Q18 demonstrates the difference between WHERE (pre-aggregation filter) and HAVING (post-aggregation filter).

### Section D — Joins & Relationships (Q19–Q23)
INNER JOIN, LEFT JOIN, three-table JOIN chain, LEFT vs RIGHT JOIN comparison, FULL OUTER JOIN simulation (MySQL doesn't support it natively — simulated with LEFT JOIN UNION RIGHT JOIN), and FK violation demonstration.

### Section E — Advanced Concepts (Q24–Q27)
CASE for price tier classification, CASE inside aggregate for conditional counting, full ACID property explanation with real-world examples.


---

## Sample Data

- **8 customers** across Maharashtra, Gujarat, Delhi, Telangana, Rajasthan, Tamil Nadu, Kerala
- **8 products** across Electronics, Clothing, and Home categories
- **10 orders** with statuses: Delivered, Shipped, Pending, Cancelled
- **15 order items** linking orders to products with quantity and discount info

---

## How to Run

1. Make sure MySQL 8.0+ is running locally and the `Ecomm` database exists
2. Open `ECommerce_queries.ipynb` in Jupyter
3. Run the connection cell first (it will prompt for your MySQL password securely via `getpass`)
4. Run the setup cells top to bottom — these create the 4 tables and load all sample data
5. All query cells can then be run in order, with output appearing inline

**Requirements:** `ipython-sql`, `sqlalchemy`, `mysql-connector-python`
```bash
pip install ipython-sql sqlalchemy mysql-connector-python
```

---

## Folder Structure

```
Week2_SQL_ECommerce_Database/
├── ECommerce_queries.ipynb   # all queries with inline output
└── README.md
```
