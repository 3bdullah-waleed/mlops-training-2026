# mlops-training-2026

Hands-on tasks from the MLOps Training 2026/2027 track.

## Tasks

| # | Task | Status |
|---|---|---|
| 1 | [Get the Data Into a Database](#task-1--get-the-data-into-a-database) | ✅ Done |

---

## Task 1 — Get the Data Into a Database

Loaded the [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) into a real relational database, in preparation for a later task: predicting whether an order is delivered late or on time.

**Stack used:** Docker · PostgreSQL 18.4 · psql

### Reference Diagrams

**Figure 1: Olist Dataset Table Relationships**

![Image Alt](https://github.com/3bdullah-waleed/mlops-training-2026/blob/555dfefe43ba4348af17c8a28db841c5b6640e22/HRhd2Y0.png)

Shows the 9 raw tables in the dataset and the keys that connect them: `order_id`, `customer_id`, `product_id`, `seller_id`, and `zip_code_prefix`.

**Figure 2: Order Lifecycle — From Customer to Review**

![Image Alt](https://github.com/3bdullah-waleed/mlops-training-2026/blob/2ebb1aa1a56fdf577091410e94a7b3ffe3c2b1f3/Order%20Lifecycle.png)

Reframes the same schema as a business process: a customer places an order, the order contains items (products from sellers), the order is paid for, and after delivery it's reviewed — with customers and sellers both tied back to a shared geolocation table.

### Setup

1. Installed Docker Desktop and ran a PostgreSQL 18.4 container (`olist-postgres`) on port `5432`.
2. Created the full schema — 9 tables (`customers`, `sellers`, `products`, `orders`, `order_items`, `order_payments`, `order_reviews`, `geolocation`, `product_category_name_translation`) — matching the relationships shown in Figure 1.
3. Downloaded all CSV files from the Kaggle dataset page.
4. Loaded every CSV into its matching table using PostgreSQL's `\copy` command.
5. Verified the load with row counts and a `JOIN` query across related tables.

### Evidence

**Figure 3: Schema Creation**

![Image Alt](https://github.com/3bdullah-waleed/mlops-training-2026/blob/c602065efd1c754c7f00fc9906675af04046129c/Screenshot%202026-08-08%20195312.png)

Creating the `customers` table inside the running Postgres container via `psql`.

**Figure 4: Container Running With All Tables Inside**

![Image Alt](https://github.com/3bdullah-waleed/mlops-training-2026/blob/142911e84c123ee8083e9f3b346b398f0f6515e5/Screenshot%202026-08-08%20195605.png)

The `olist-postgres` container running in Docker Desktop, with all 9 tables visible via `\dt` in the container's Exec terminal.

**Figure 5: Full Table List**

![Image Alt]()

`\dt` output confirming all 9 tables exist inside the `olist` database.

**Figure 6: Loading Data**

![Image Alt]()

Loading the `customers` CSV into the `customers` table with `\copy` — 99,441 rows inserted. The same `\copy` process was repeated for the remaining 8 tables, each loaded successfully with its full row count.

**Figure 7: Join Across Tables**

![Image Alt]()

A `JOIN` between `orders` and `customers` on `customer_id`, returning combined columns from both tables.

**Figure 8: Row Count Verification**

*(figure placeholder)*

`COUNT(*)` on `customers` confirms 99,441 rows, matching the number of rows loaded.
