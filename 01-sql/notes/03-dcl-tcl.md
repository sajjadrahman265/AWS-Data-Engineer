# 🔐 DCL and TCL in PostgreSQL — Managing Permissions & Transactions Like a Pro

In previous parts of our SQL series, we learned:
✅ **DDL** — how to define and structure databases (CREATE, ALTER, DROP)
✅ **DML** — how to manipulate data (INSERT, UPDATE, DELETE, TRUNCATE)

Now, we move to the **final two types of SQL commands**:
👉 **DCL (Data Control Language)** and
👉 **TCL (Transaction Control Language)**

These commands don’t create or edit data directly — instead, they **control how data is accessed and managed safely**.

---

## 🧩 1️⃣ DCL — Data Control Language

### 📘 Definition

DCL commands **control user access and permissions** in the database.
They decide **who** can do **what** — like who can read data, who can modify it, and who can’t.

---

### 🔑 Common DCL Commands

| Command  | Purpose                                  |
| -------- | ---------------------------------------- |
| `GRANT`  | Give permission to a user or role        |
| `REVOKE` | Take back permission from a user or role |

---

### 🧱 Example Setup

Let’s create a simple table for demonstration.

```sql
CREATE TABLE employees (
    emp_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    emp_name VARCHAR(50),
    salary NUMERIC(10,2)
);
```

Now, assume we have a PostgreSQL role (user) called **report_user**.

---

### 🧑‍💻 GRANT — Give Access Privileges

#### 📘 Definition

`GRANT` lets you assign permissions to users or roles.

#### 💻 Example 1: Give SELECT Permission

```sql
GRANT SELECT ON employees TO report_user;
```

✅ This means **report_user** can **read** data from the `employees` table, but not change it.

---

#### 💻 Example 2: Give INSERT and UPDATE Privileges

```sql
GRANT INSERT, UPDATE ON employees TO report_user;
```

Now the user can also insert and update rows.

---

#### 💻 Example 3: Grant All Privileges

```sql
GRANT ALL PRIVILEGES ON employees TO report_user;
```

✅ `report_user` can now perform **any operation** on the `employees` table — including SELECT, INSERT, UPDATE, DELETE.

---

### 🛑 REVOKE — Take Back Permissions

#### 📘 Definition

`REVOKE` removes privileges previously granted to a user.

#### 💻 Example 1: Remove Update Privilege

```sql
REVOKE UPDATE ON employees FROM report_user;
```

Now `report_user` can no longer modify data.

---

#### 💻 Example 2: Remove All Privileges

```sql
REVOKE ALL PRIVILEGES ON employees FROM report_user;
```

✅ The user loses all access to the table.

---

### 💬 Tip: Checking User Privileges

In PostgreSQL, you can check privileges by describing the table:

```bash
\d+ employees
```

Output will show which roles have what access permissions.

---

### 🧠 DCL Summary

| Command  | Description       | Example                                          |
| -------- | ----------------- | ------------------------------------------------ |
| `GRANT`  | Give privileges   | `GRANT SELECT ON employees TO user1;`            |
| `REVOKE` | Remove privileges | `REVOKE ALL PRIVILEGES ON employees FROM user1;` |

---

## ⚙️ 2️⃣ TCL — Transaction Control Language

### 📘 Definition

TCL commands control **transactions** — a set of SQL statements that are executed together as one logical unit.

If something fails, you can **undo** it (ROLLBACK).
If everything succeeds, you **save** it (COMMIT).

---

### 💡 Why Transactions Matter

Imagine transferring money between two bank accounts:

1. Withdraw from Account A
2. Deposit to Account B

If the deposit fails after the withdrawal, you’d lose money!
That’s why databases use **transactions** — they ensure **all-or-nothing consistency**.

---

### 🔄 Common TCL Commands

| Command                       | Purpose                                             |
| ----------------------------- | --------------------------------------------------- |
| `BEGIN` / `START TRANSACTION` | Start a new transaction                             |
| `COMMIT`                      | Save changes permanently                            |
| `ROLLBACK`                    | Undo all changes made in the current transaction    |
| `SAVEPOINT`                   | Create a point to roll back to within a transaction |
| `RELEASE SAVEPOINT`           | Delete a savepoint                                  |
| `SET TRANSACTION`             | Change transaction settings (like isolation level)  |

---

### 🧮 Example: COMMIT and ROLLBACK in Action

Let’s use our `employees` table again.

```sql
BEGIN;

INSERT INTO employees (emp_name, salary)
VALUES ('Sakib', 5000.00);

INSERT INTO employees (emp_name, salary)
VALUES ('Sara', 6000.00);

COMMIT;
```

✅ Both rows are permanently saved once COMMIT runs.

---

### 💥 Example: ROLLBACK (Undo Changes)

```sql
BEGIN;

INSERT INTO employees (emp_name, salary)
VALUES ('Arif', 7000.00);

-- Oops! We realize this is wrong
ROLLBACK;
```

❌ The insert is **undone**.
No new data will appear in the `employees` table.

---

### 🪄 Example: SAVEPOINT and ROLLBACK TO

Savepoints allow you to roll back **part** of a transaction.

```sql
BEGIN;

INSERT INTO employees (emp_name, salary)
VALUES ('Fokrul', 5500.00);

SAVEPOINT after_first_insert;

INSERT INTO employees (emp_name, salary)
VALUES ('Sajjad', NULL); -- Missing salary, causes error or logic fail

ROLLBACK TO after_first_insert;

COMMIT;
```

✅ The first insert (Fokrul) is saved.
✅ The second one (Sajjad) is undone.
✅ Transaction successfully completes.

---

### 🧠 PostgreSQL Transaction Notes

| Feature                                                                     | Description                            |
| --------------------------------------------------------------------------- | -------------------------------------- |
| Transactions are **atomic**                                                 | All operations succeed or none do      |
| Transactions are **durable**                                                | COMMIT makes changes permanent         |
| PostgreSQL supports **nested savepoints**                                   | You can roll back to any defined point |
| You can view current transaction status using `SHOW transaction_isolation;` |                                        |

---

## 🧾 Full Example: DCL + TCL Combined

Here’s a small real-world example combining both:

```sql
-- Step 1: Start a transaction
BEGIN;

-- Step 2: Insert a new employee
INSERT INTO employees (emp_name, salary)
VALUES ('Aisha', 6500.00);

-- Step 3: Commit the transaction
COMMIT;

-- Step 4: Grant read-only access to report_user
GRANT SELECT ON employees TO report_user;
```

✅ Data inserted and saved
✅ Permissions granted to another user safely

---

## 🧠 Summary Table — DCL and TCL

| Category | Command       | Purpose                  | Example                               |
| -------- | ------------- | ------------------------ | ------------------------------------- |
| **DCL**  | `GRANT`       | Give privileges          | `GRANT SELECT ON employees TO user1;` |
|          | `REVOKE`      | Remove privileges        | `REVOKE ALL ON employees FROM user1;` |
| **TCL**  | `BEGIN`       | Start transaction        | `BEGIN;`                              |
|          | `COMMIT`      | Save changes             | `COMMIT;`                             |
|          | `ROLLBACK`    | Undo changes             | `ROLLBACK;`                           |
|          | `SAVEPOINT`   | Create rollback point    | `SAVEPOINT sp1;`                      |
|          | `ROLLBACK TO` | Undo part of transaction | `ROLLBACK TO sp1;`                    |

---

## 🚀 Pro Tips for Using DCL & TCL in PostgreSQL

💡 **1. Always wrap critical updates in transactions:**

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

💡 **2. Use `ROLLBACK` if something looks off before committing.**

💡 **3. Give minimum privileges needed — follow the principle of least privilege.**

💡 **4. Use `GRANT` on roles instead of individual users for easier management.**

💡 **5. Use `SAVEPOINT` in long transactions for better recovery options.**

---

# 🏁 Final Thoughts

You’ve now completed all four major SQL command groups:

| Type    | Purpose              | Example Commands                           |
| ------- | -------------------- | ------------------------------------------ |
| **DDL** | Define structure     | `CREATE`, `ALTER`, `DROP`                  |
| **DML** | Work with data       | `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`   |
| **DCL** | Control access       | `GRANT`, `REVOKE`                          |
| **TCL** | Control transactions | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

By mastering these, you can:

* Design secure databases
* Manage data safely
* Handle rollback & permissions like a professional DBA

---
