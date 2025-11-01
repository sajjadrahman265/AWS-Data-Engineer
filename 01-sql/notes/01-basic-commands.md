# SQL Basic Query : Part-01

When learning SQL, it’s easy to memorize keywords like `SELECT`, `WHERE`, `GROUP BY`, and `ORDER BY`…
But **SQL doesn’t execute them in the order you write them** — and that’s where confusion (and tricky interview questions) begin.

Once you understand **how SQL thinks**, you’ll be able to:

✅ Debug errors faster
✅ Optimize slow queries
✅ Write elegant, logical SQL code
✅ Explain your reasoning confidently in interviews

Let’s go step by step through each SQL clause, understand its purpose, and see how they all connect — with examples and sample tables.

### **1️⃣ FROM — Choosing the Data Source**

#### 📘 Definition:

The `FROM` clause tells SQL **which table(s)** to retrieve data from.
It’s the **first step** in query execution.

#### 📋 Example Table: `Customers`

| id | name  | country | score |
| -- |  -- |   - |  -- |
| 1  | Alice | USA     | 450   |
| 2  | Bob   | USA     | 470   |
| 3  | Carol | Canada  | 420   |
| 4  | Dan   | Canada  | 0     |
| 5  | Eva   | UK      | 480   |

#### 💻 Example:

```sql
SELECT *
FROM Customers;
```

🧾 **Result:**
All rows and columns from the `Customers` table.

| id | name  | country | score |
| -- |  -- |   - |  -- |
| 1  | Alice | USA     | 450   |
| 2  | Bob   | USA     | 470   |
| 3  | Carol | Canada  | 420   |
| 4  | Dan   | Canada  | 0     |
| 5  | Eva   | UK      | 480   |

#### 💡 Tip:

You can also join tables in `FROM`:

```sql
SELECT Orders.id, Customers.name
FROM Orders
JOIN Customers ON Orders.customer_id = Customers.id;
```

SQL first combines data **before** applying filters or aggregations.

 

### **2️⃣ WHERE — Filtering Rows**

#### 📘 Definition:

`WHERE` filters rows **before** grouping or aggregation.
Only rows that meet the condition move forward.

#### 💻 Example:

```sql
SELECT *
FROM Customers
WHERE score > 0;
```

🧾 **Result:**

| id | name  | country | score |
| -- |  -- |   - |  -- |
| 1  | Alice | USA     | 450   |
| 2  | Bob   | USA     | 470   |
| 3  | Carol | Canada  | 420   |
| 5  | Eva   | UK      | 480   |

Rows with a `score` of `0` are removed.

#### ⚡ Tip:

Always filter early — it reduces data volume for later steps.

 

### **3️⃣ GROUP BY — Combining Similar Rows**

#### 📘 Definition:

`GROUP BY` groups rows that share the same values into buckets — so you can summarize them.

#### 💻 Example:

```sql
SELECT country, AVG(score) AS avg_score
FROM Customers
WHERE score > 0
GROUP BY country;
```

🧾 **Result:**

| country | avg_score |
|   - |     |
| USA     | 460       |
| Canada  | 420       |
| UK      | 480       |

#### 💡 Tip:

Every column in `SELECT` must either:

* Appear in the `GROUP BY`
* Or use an aggregate function (`AVG()`, `SUM()`, `COUNT()`)

Otherwise, SQL doesn’t know which value to show.

 

### **4️⃣ HAVING — Filtering Groups (After Aggregation)**

#### 📘 Definition:

`HAVING` filters *groups* after aggregation — like a “WHERE for groups.”

#### 💻 Example:

```sql
SELECT country, AVG(score) AS avg_score
FROM Customers
WHERE score > 0
GROUP BY country
HAVING AVG(score) > 430;
```

🧾 **Result:**

| country | avg_score |
|   - |     |
| USA     | 460       |
| UK      | 480       |

💬 `HAVING` is used because `AVG(score)` is a **group result**, not a row value.

 

### **5️⃣ SELECT — Choosing What to Display**

#### 📘 Definition:

`SELECT` specifies which columns or calculations to display in the final output.

#### 💻 Example:

```sql
SELECT 
    country,
    COUNT(id) AS total_customers,
    AVG(score) AS average_score
FROM Customers
WHERE score > 0
GROUP BY country;
```

🧾 **Result:**

| country | total_customers | average_score |
|   - |       |     - |
| USA     | 2               | 460           |
| Canada  | 1               | 420           |
| UK      | 1               | 480           |

#### 💡 Tip:

Aliases (like `average_score`) can’t be used inside `WHERE` because they’re created *after* the selection step.

### **6️⃣ DISTINCT — Removing Duplicate Rows**

#### 📘 Definition:

`DISTINCT` returns only **unique rows** from your result set.

#### 📋 Example Table:

| id | country |
| -- |   - |
| 1  | USA     |
| 2  | USA     |
| 3  | Canada  |
| 4  | UK      |

#### 💻 Example:

```sql
SELECT DISTINCT country
FROM Customers;
```

🧾 **Result:**

| country |
|   - |
| USA     |
| Canada  |
| UK      |

#### ⚙️ Behind the Scenes:

`DISTINCT` sorts or hashes data to find duplicates, which can slow queries on big tables.

💡 **Tip:** If you’re using `GROUP BY`, you don’t need `DISTINCT`.

### **7️⃣ ORDER BY — Sorting the Results**

#### 📘 Definition:

`ORDER BY` sorts your results in ascending (`ASC`) or descending (`DESC`) order.

#### 💻 Example:

```sql
SELECT country, AVG(score) AS avg_score
FROM Customers
WHERE score > 0
GROUP BY country
ORDER BY avg_score DESC;
```

🧾 **Result:**

| country | avg_score |
|   - |     |
| UK      | 480       |
| USA     | 460       |
| Canada  | 420       |

### ⚙️ ORDER BY Left-to-Right Behavior

SQL sorts from **left to right** — like a multi-level sort in Excel.

#### 💻 Example:

```sql
SELECT * 
FROM Employees
ORDER BY department ASC, salary DESC, name ASC;
```

🧠 Meaning:

1. Sort by `department` alphabetically.
2. If two employees share a department, sort by `salary` descending.
3. If both are identical, sort by `name` ascending.

💡 **Tip:** Always explicitly define the sort direction for clarity.

### **8️⃣ TOP / LIMIT — Restricting the Number of Rows**

#### 📘 Definition:

`TOP` (SQL Server) and `LIMIT` (MySQL/PostgreSQL) restrict how many rows appear in your final output.

#### 💻 Example (SQL Server):

```sql
SELECT TOP (2) country, AVG(score) AS avg_score
FROM Customers
WHERE score > 0
GROUP BY country
ORDER BY avg_score DESC;
```

🧾 **Result:**

| country | avg_score |
|   - |     |
| UK      | 480       |
| USA     | 460       |

#### 💻 Example (MySQL / PostgreSQL):

```sql
SELECT country, AVG(score) AS avg_score
FROM Customers
WHERE score > 0
GROUP BY country
ORDER BY avg_score DESC
LIMIT 2;
```

💡 **Tip:** Use it with `ORDER BY` to return “Top N” results — like *Top 5 products by sales*.

## 🔄 SQL Execution Flow Summary

| Step               | Clause                                        | What Happens |
|        |                 |      |
| **1. FROM**        | Identify the table(s) and join data if needed |              |
| **2. WHERE**       | Filter rows (remove unwanted data early)      |              |
| **3. GROUP BY**    | Group rows with matching values               |              |
| **4. HAVING**      | Filter the grouped results                    |              |
| **5. SELECT**      | Choose columns and calculate aggregates       |              |
| **6. DISTINCT**    | Remove duplicates                             |              |
| **7. ORDER BY**    | Sort the final result                         |              |
| **8. TOP / LIMIT** | Return only top N rows                        |              |

## 🧮 Coding Order vs. Execution Order

### 💻 Coding Order:

```sql
SELECT DISTINCT TOP 2
    col1, col2, SUM(col3)
FROM Table
WHERE col = 10
GROUP BY col1
HAVING SUM(col3) > 30
ORDER BY col1 ASC;
```

### ⚙️ Execution Order:

1. `FROM`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `DISTINCT`
7. `ORDER BY`
8. `TOP / LIMIT`

 

## 🧩 Full Practical Example: Everything Together

```sql
SELECT TOP (2)
    country,
    COUNT(id) AS total_customers,
    AVG(score) AS avg_score
FROM Customers
WHERE score <> 0
GROUP BY country
HAVING AVG(score) > 430
ORDER BY avg_score DESC;
```

### Input Table: `Customers`

| id | name  | country | score |
| -- |  -- |   - |  -- |
| 1  | Alice | USA     | 450   |
| 2  | Bob   | USA     | 470   |
| 3  | Carol | Canada  | 420   |
| 4  | Dan   | Canada  | 0     |
| 5  | Eva   | UK      | 480   |

### Execution Breakdown:

| Step     | Action                   | Output             |
|   -- |          |        |
| FROM     | Load all rows            | 5 rows             |
| WHERE    | Exclude score = 0        | 4 rows             |
| GROUP BY | Group by country         | 3 groups           |
| HAVING   | Keep avg > 430           | 2 groups (USA, UK) |
| SELECT   | Show country, count, avg | 2 rows             |
| ORDER BY | Sort by avg_score DESC   | UK → USA           |
| TOP      | Keep top 2               | Final output       |

🧾 **Final Output:**

| country | total_customers | avg_score |
|   - |       |     |
| UK      | 1               | 480       |
| USA     | 2               | 460       |

## 🏁 Key Takeaways

✅ SQL executes queries *from the bottom up*, not the top down.
✅ `WHERE` filters **before** grouping; `HAVING` filters **after**.
✅ Always include non-aggregated columns in `GROUP BY`.
✅ `ORDER BY` sorts left-to-right — ties are resolved by the next column.
✅ Avoid unnecessary `DISTINCT` to improve performance.
✅ Understanding execution order helps you **think like SQL** — and that’s what separates beginners from professionals.

