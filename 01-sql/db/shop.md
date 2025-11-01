# 🏪 Shop Database (PostgreSQL)

This project contains the SQL schema and mock data for a sample **Shop Management System**.

It includes the following tables:
- `customers`
- `products`
- `orders`
- `order_items`

## 🚀 Setup Instructions

### 1️⃣ Create the database
```bash
psql -U postgres -c "CREATE DATABASE shop;"
````

### 2️⃣ Import the SQL dump

```bash
psql -U postgres -d shop -f shop.sql
```

## 📋 Verify Setup

After import, connect to the database:

```bash
psql -U postgres -d shop
```

List all tables:

```sql
\dt
```

View data:

```sql
SELECT * FROM customers LIMIT 5;
```

## 🧱 Schema Overview

**Relationships**

* Each order belongs to one customer.
* Each order has multiple order_items.
* Each order_item references a product.

```
customers (1)───< orders (1)───< order_items >───(1) products
```

## 🧰 Notes

* The `shop.sql` file includes both schema and data.
* You can generate new mock data using the `generate_mock.py` script.
* PostgreSQL version used: `15+`
* Default port: `5432`


## 🧑‍💻 Commands Reference

| Task              | Command                                     |
| ----------------- | ------------------------------------------- |
| Export DB to file | `pg_dump -U postgres -d shop -f shop.sql`   |
| Restore DB        | `psql -U postgres -d shop -f shop.sql`      |
| Drop DB           | `psql -U postgres -c "DROP DATABASE shop;"` |

## 🗂️ File Structure

```
/db
 ├── shop.sql          # Full SQL dump (schema + mock data)
 ├── generate_mock.py  # Python script to regenerate data
 └── shop.md           # This instruction file
```

## 📅 Last Updated

**2025-11-01**
