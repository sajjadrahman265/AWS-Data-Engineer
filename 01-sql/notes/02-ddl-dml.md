# 🧱 DDL vs DML in PostgreSQL — The Foundation of Database Commands (With Table Examples)

When working with SQL, every command you write falls into one of two major categories:

👉 **DDL (Data Definition Language)** or
👉 **DML (Data Manipulation Language)**

Both are essential — but they serve very different purposes.
Understanding the difference is key for both **database design** and **data manipulation**.

---

## ⚙️ What’s the Difference Between DDL and DML?

| Feature           | DDL (Data Definition Language)             | DML (Data Manipulation Language)                   |
| ----------------- | ------------------------------------------ | -------------------------------------------------- |
| **Purpose**       | Defines or modifies the database structure | Works with the data inside tables                  |
| **Affects**       | Schema / structure                         | Rows / records                                     |
| **Returns Data?** | ❌ No                                       | ✅ Yes (with `SELECT`)                              |
| **Examples**      | `CREATE`, `ALTER`, `DROP`                  | `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, `SELECT` |

---

# 🧩 Part 1: DDL — Data Definition Language

**DDL commands** define or modify the structure of your PostgreSQL database.
These commands **do not return data**, but they **change how your database is built**.

---

## 🏗️ 1️⃣ CREATE — Create a New Table

### 📘 Definition

`CREATE TABLE` defines a new table and its columns.

### 💻 Example

Let’s create a table called `persons` with columns for ID, name, date of birth, phone, and email.

```sql
CREATE TABLE persons (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    person_name VARCHAR(50) NOT NULL,
    dob DATE,
    phone VARCHAR(13),
    email VARCHAR(50)
);
```

### 📋 Table Created

| id                          | person_name | dob | phone | email |
| --------------------------- | ----------- | --- | ----- | ----- |
| *(empty — structure ready)* |             |     |       |       |

💡 **Tip:**
`GENERATED ALWAYS AS IDENTITY` automatically generates unique IDs — it’s PostgreSQL’s modern alternative to `SERIAL`.

---

## 🧱 2️⃣ ALTER — Modify an Existing Table

### 📘 Definition

Used to add, modify, or remove columns.

---

### 🧩 Example 1: Add a New Column

```sql
ALTER TABLE persons
ADD COLUMN address VARCHAR(100);
```

📋 **After Adding:**

| id                                  | person_name | dob | phone | email | address |
| ----------------------------------- | ----------- | --- | ----- | ----- | ------- |
| *(empty table — structure changed)* |             |     |       |       |         |

---

### 🧩 Example 2: Drop a Column

```sql
ALTER TABLE persons
DROP COLUMN phone;
```

📋 **After Dropping `phone`:**

| id | person_name | dob | email | address |
| -- | ----------- | --- | ----- | ------- |

⚠️ **Note:**
Dropping a column permanently deletes all its data.

---

### 🧩 Example 3: Add a NOT NULL Column Safely

When adding a NOT NULL column to a table that already has data:

```sql
ALTER TABLE persons ADD COLUMN country VARCHAR(50);

UPDATE persons
SET country = 'Unknown'
WHERE country IS NULL;

ALTER TABLE persons
ALTER COLUMN country SET NOT NULL;
```

💡 **Pro Tip:**
In PostgreSQL, adding a NOT NULL column to a table with existing rows requires a default value or a safe backfill.

---

## 💣 3️⃣ DROP — Delete a Table Completely

### 📘 Definition

Deletes a table and all its data permanently.

```sql
DROP TABLE IF EXISTS persons;
```

🚨 **Warning:**
Once you run `DROP TABLE`, both the table and its data are gone forever.

💬 “Creating takes effort, destroying takes one command.”

---

### 🧠 DDL Summary

| Command  | Description                |
| -------- | -------------------------- |
| `CREATE` | Create tables or databases |
| `ALTER`  | Modify existing tables     |
| `DROP`   | Delete a table or database |

---

# 🧮 Part 2: DML — Data Manipulation Language

**DML commands** manipulate the data inside tables — inserting, updating, deleting, or retrieving rows.

---

## 🧍 1️⃣ INSERT — Add New Data

### 📘 Definition

Adds one or more new rows into a table.

---

### 💻 Example: Insert One Row

```sql
INSERT INTO persons (person_name, dob, phone, email, address, country)
VALUES ('Sakib', '1995-08-20', '0123456789', 'sakib@example.com', 'Dhaka', 'Bangladesh');
```

📋 **Table After Insert**

| id | person_name | dob        | phone      | email                                         | address | country    |
| -- | ----------- | ---------- | ---------- | --------------------------------------------- | ------- | ---------- |
| 1  | Sakib       | 1995-08-20 | 0123456789 | [sakib@example.com](mailto:sakib@example.com) | Dhaka   | Bangladesh |

---

### 💻 Example: Insert Multiple Rows

```sql
INSERT INTO persons (person_name, dob, phone, email, address, country)
VALUES 
('Sara', '1990-01-10', '0987654321', 'sara@gmail.com', 'London', 'UK'),
('Arif', NULL, '0130000000', 'arif@gmail.com', 'Toronto', 'Canada');
```

📋 **Result**

| id | person_name | dob        | phone      | email                                         | address | country    |
| -- | ----------- | ---------- | ---------- | --------------------------------------------- | ------- | ---------- |
| 1  | Sakib       | 1995-08-20 | 0123456789 | [sakib@example.com](mailto:sakib@example.com) | Dhaka   | Bangladesh |
| 2  | Sara        | 1990-01-10 | 0987654321 | [sara@gmail.com](mailto:sara@gmail.com)       | London  | UK         |
| 3  | Arif        | NULL       | 0130000000 | [arif@gmail.com](mailto:arif@gmail.com)       | Toronto | Canada     |

---

### 💻 Example: Insert Data from Another Table

Let’s copy data from a `customers` table:

```sql
INSERT INTO persons (person_name, dob, email)
SELECT first_name, birth_date, email
FROM customers;
```

💬 **Use Case:** Copying or migrating data between tables.

---

## ✏️ 2️⃣ UPDATE — Modify Existing Rows

### 📘 Definition

Updates one or more existing rows.

---

### 📋 Before Update

| id | person_name | country | score |
| -- | ----------- | ------- | ----- |
| 10 | Sara        | NULL    | NULL  |

---

### 💻 Example

```sql
UPDATE persons
SET country = 'UK', email = 'sara@newmail.com'
WHERE id = 10;
```

📋 **After Update**

| id | person_name | country | email                                       |
| -- | ----------- | ------- | ------------------------------------------- |
| 10 | Sara        | UK      | [sara@newmail.com](mailto:sara@newmail.com) |

⚠️ **Warning:**
If you forget `WHERE`, **all rows** will be updated.

💡 **Best Practice:**

```sql
-- Check first
SELECT * FROM persons WHERE id = 10;

-- Then update
UPDATE persons SET country = 'UK' WHERE id = 10;
```

---

## 🗑️ 3️⃣ DELETE — Remove Rows

### 📘 Definition

Deletes one or more rows based on a condition.

---

### 📋 Before Delete

| id | person_name | country    | email                                         |
| -- | ----------- | ---------- | --------------------------------------------- |
| 1  | Sakib       | Bangladesh | [sakib@example.com](mailto:sakib@example.com) |
| 2  | Sara        | UK         | [sara@gmail.com](mailto:sara@gmail.com)       |
| 3  | Arif        | Canada     | [arif@gmail.com](mailto:arif@gmail.com)       |

---

### 💻 Example

```sql
DELETE FROM persons
WHERE id = 3;
```

📋 **After Delete**

| id | person_name | country    | email                                         |
| -- | ----------- | ---------- | --------------------------------------------- |
| 1  | Sakib       | Bangladesh | [sakib@example.com](mailto:sakib@example.com) |
| 2  | Sara        | UK         | [sara@gmail.com](mailto:sara@gmail.com)       |

⚠️ **Tip:** Always test with a `SELECT` before deleting:

```sql
SELECT * FROM persons WHERE id = 3;
```

---

## 🧹 4️⃣ TRUNCATE — Quickly Remove All Rows

### 📘 Definition

Removes **all rows** from a table instantly while keeping its structure.

```sql
TRUNCATE TABLE persons;
```

💡 **Difference Between DELETE and TRUNCATE**

| Feature   | DELETE                              | TRUNCATE        |
| --------- | ----------------------------------- | --------------- |
| Removes   | Specific rows (with `WHERE`)        | All rows        |
| Logging   | Logs each row                       | Minimal logging |
| Speed     | Slower                              | Much faster     |
| Rollback  | ✅ Yes (transactional in PostgreSQL) | ✅ Yes           |
| Structure | Stays                               | Stays           |

💬 **Tip:**
Use `TRUNCATE` for a quick full cleanup — it’s safe inside a transaction in PostgreSQL.

---

## 🧠 DML Summary

| Command    | Purpose         | Returns Data? |
| ---------- | --------------- | ------------- |
| `INSERT`   | Add new rows    | No            |
| `UPDATE`   | Modify rows     | No            |
| `DELETE`   | Remove rows     | No            |
| `TRUNCATE` | Remove all rows | No            |
| `SELECT`   | Retrieve data   | ✅ Yes         |

---

# 🔍 DDL vs DML — Quick Comparison

| Category | Command    | Affects   | Description                          |
| -------- | ---------- | --------- | ------------------------------------ |
| **DDL**  | `CREATE`   | Structure | Creates new tables/databases         |
|          | `ALTER`    | Structure | Changes table columns or constraints |
|          | `DROP`     | Structure | Deletes tables/databases             |
| **DML**  | `INSERT`   | Data      | Adds records                         |
|          | `UPDATE`   | Data      | Modifies records                     |
|          | `DELETE`   | Data      | Removes records                      |
|          | `TRUNCATE` | Data      | Clears all data                      |

---

# 🚀 PostgreSQL Pro Tips

💡 **1. Always back up before DROP or TRUNCATE.**
They remove data permanently.

💡 **2. Use `IF EXISTS`** with `DROP` to avoid errors:

```sql
DROP TABLE IF EXISTS persons;
```

💡 **3. Always test your WHERE clause with `SELECT`.**
Accidental full-table updates/deletes are common mistakes.

💡 **4. Use transactions for safety.**

```sql
BEGIN;
DELETE FROM persons WHERE country = 'UK';
ROLLBACK; -- or COMMIT if safe
```

💡 **5. Use `LIMIT` instead of `TOP`.**
PostgreSQL uses:

```sql
SELECT * FROM persons ORDER BY id DESC LIMIT 5;
```

💡 **6. Use `IDENTITY` instead of `SERIAL` for modern auto-increment IDs.**

---

# 🏁 Final Thoughts

Understanding **DDL vs DML** is fundamental to mastering PostgreSQL.

* **DDL** defines *what your database looks like.*
* **DML** defines *how data moves and lives inside it.*

Together, they power every data workflow — from schema design to data updates and maintenance.

By learning both deeply, you’ll be able to **design, manage, and optimize** your PostgreSQL databases like a professional.

