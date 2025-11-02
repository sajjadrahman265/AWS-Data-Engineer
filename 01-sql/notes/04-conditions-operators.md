Perfect 👏 Tajul, Fokrul, and Sajjad — this is a great next step in your learning series!

We’ll now build a **PostgreSQL-friendly, blog-style tutorial** for the `WHERE` clause and its operators — just like your `world` database example.

I’ll include:

* A small sample table
* Example queries for **each operator category**
* **Actual results** (mocked but realistic)
* Clear explanations for every example

---

# 🔍 SQL `WHERE` Clause — Filtering Data in PostgreSQL (With Real Examples)

The `WHERE` clause in SQL is used to **filter rows** from a table based on specific conditions.

It’s one of the most powerful parts of SQL — allowing you to retrieve *only* the data you need.

Let’s explore all the common `WHERE` operators with examples from a sample table.

---

## 🧱 Sample Table: `city`

Let’s assume we have the same table as in your `world` database:

```sql
CREATE TABLE city (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    countrycode CHAR(3),
    district VARCHAR(50),
    population INT
);
```

| id | name           | countrycode | district      | population |
| -- | -------------- | ----------- | ------------- | ---------- |
| 1  | Kabul          | AFG         | Kabol         | 1780000    |
| 2  | Qandahar       | AFG         | Qandahar      | 237500     |
| 3  | Herat          | AFG         | Herat         | 186800     |
| 4  | Mazar-e-Sharif | AFG         | Balkh         | 127800     |
| 5  | Amsterdam      | NLD         | Noord-Holland | 731200     |
| 6  | Rotterdam      | NLD         | Zuid-Holland  | 593321     |
| 7  | Utrecht        | NLD         | Utrecht       | 265446     |
| 8  | London         | GBR         | England       | 7285000    |
| 9  | Birmingham     | GBR         | England       | 1013000    |
| 10 | Manchester     | GBR         | England       | 430000     |

---

## 🧮 1️⃣ Comparison Operators

| Operator     | Description              |
| ------------ | ------------------------ |
| `=`          | Equal to                 |
| `<>` or `!=` | Not equal to             |
| `>`          | Greater than             |
| `<`          | Less than                |
| `>=`         | Greater than or equal to |
| `<=`         | Less than or equal to    |

---

### 🧩 Example 1 — Equal To (`=`)

```sql
SELECT * FROM city WHERE id = 5;
```

**Result:**

| id | name      | countrycode | district      | population |
| -- | --------- | ----------- | ------------- | ---------- |
| 5  | Amsterdam | NLD         | Noord-Holland | 731200     |

---

### 🧩 Example 2 — Not Equal To (`<>` or `!=`)

```sql
SELECT * FROM city WHERE countrycode <> 'AFG';
-- or equivalently
SELECT * FROM city WHERE countrycode != 'AFG';
```

**Result:**

| id | name       | countrycode | district      | population |
| -- | ---------- | ----------- | ------------- | ---------- |
| 5  | Amsterdam  | NLD         | Noord-Holland | 731200     |
| 6  | Rotterdam  | NLD         | Zuid-Holland  | 593321     |
| 7  | Utrecht    | NLD         | Utrecht       | 265446     |
| 8  | London     | GBR         | England       | 7285000    |
| 9  | Birmingham | GBR         | England       | 1013000    |
| 10 | Manchester | GBR         | England       | 430000     |

---

### 🧩 Example 3 — Greater Than (`>`)

```sql
SELECT * FROM city WHERE population > 1000000;
```

**Result:**

| id | name       | countrycode | district | population |
| -- | ---------- | ----------- | -------- | ---------- |
| 1  | Kabul      | AFG         | Kabol    | 1780000    |
| 8  | London     | GBR         | England  | 7285000    |
| 9  | Birmingham | GBR         | England  | 1013000    |

---

### 🧩 Example 4 — Less Than or Equal To (`<=`)

```sql
SELECT * FROM city WHERE population <= 200000;
```

**Result:**

| id | name           | countrycode | district | population |
| -- | -------------- | ----------- | -------- | ---------- |
| 3  | Herat          | AFG         | Herat    | 186800     |
| 4  | Mazar-e-Sharif | AFG         | Balkh    | 127800     |

---

## ⚙️ 2️⃣ Logical Operators

| Operator | Description                                     |
| -------- | ----------------------------------------------- |
| `AND`    | Combine multiple conditions (both must be true) |
| `OR`     | At least one condition must be true             |
| `NOT`    | Negates a condition                             |

---

### 🧩 Example 1 — AND

```sql
SELECT * FROM city
WHERE countrycode = 'AFG' AND population > 200000;
```

**Result:**

| id | name     | countrycode | district | population |
| -- | -------- | ----------- | -------- | ---------- |
| 1  | Kabul    | AFG         | Kabol    | 1780000    |
| 2  | Qandahar | AFG         | Qandahar | 237500     |

💡 Both conditions must be true:

* Country must be Afghanistan (`AFG`)
* Population must be above 200,000

---

### 🧩 Example 2 — OR

```sql
SELECT * FROM city
WHERE countrycode = 'AFG' OR countrycode = 'GBR';
```

**Result:**

| id | name           | countrycode | district | population |
| -- | -------------- | ----------- | -------- | ---------- |
| 1  | Kabul          | AFG         | Kabol    | 1780000    |
| 2  | Qandahar       | AFG         | Qandahar | 237500     |
| 3  | Herat          | AFG         | Herat    | 186800     |
| 4  | Mazar-e-Sharif | AFG         | Balkh    | 127800     |
| 8  | London         | GBR         | England  | 7285000    |
| 9  | Birmingham     | GBR         | England  | 1013000    |
| 10 | Manchester     | GBR         | England  | 430000     |

---

### 🧩 Example 3 — NOT

```sql
SELECT * FROM city
WHERE NOT (countrycode = 'GBR');
```

**Result:**

| id | name           | countrycode | district      | population |
| -- | -------------- | ----------- | ------------- | ---------- |
| 1  | Kabul          | AFG         | Kabol         | 1780000    |
| 2  | Qandahar       | AFG         | Qandahar      | 237500     |
| 3  | Herat          | AFG         | Herat         | 186800     |
| 4  | Mazar-e-Sharif | AFG         | Balkh         | 127800     |
| 5  | Amsterdam      | NLD         | Noord-Holland | 731200     |
| 6  | Rotterdam      | NLD         | Zuid-Holland  | 593321     |
| 7  | Utrecht        | NLD         | Utrecht       | 265446     |

---

## 📏 3️⃣ Range Operator — BETWEEN

| Operator          | Description                                          |
| ----------------- | ---------------------------------------------------- |
| `BETWEEN a AND b` | True if the value is between `a` and `b` (inclusive) |

---

### 🧩 Example — Find Cities with Population Between 200,000 and 1,000,000

```sql
SELECT * FROM city
WHERE population BETWEEN 200000 AND 1000000;
```

**Result:**

| id | name           | countrycode | district      | population |
| -- | -------------- | ----------- | ------------- | ---------- |
| 2  | Qandahar       | AFG         | Qandahar      | 237500     |
| 3  | Herat          | AFG         | Herat         | 186800     |
| 4  | Mazar-e-Sharif | AFG         | Balkh         | 127800     |
| 5  | Amsterdam      | NLD         | Noord-Holland | 731200     |
| 6  | Rotterdam      | NLD         | Zuid-Holland  | 593321     |
| 7  | Utrecht        | NLD         | Utrecht       | 265446     |
| 10 | Manchester     | GBR         | England       | 430000     |

---

## 🎯 4️⃣ Membership Operators — IN & NOT IN

| Operator | Description                 |
| -------- | --------------------------- |
| `IN`     | Matches any value in a list |
| `NOT IN` | Excludes values in a list   |

---

### 🧩 Example 1 — IN

```sql
SELECT * FROM city
WHERE countrycode IN ('AFG', 'GBR');
```

**Result:**

| id | name           | countrycode | district | population |
| -- | -------------- | ----------- | -------- | ---------- |
| 1  | Kabul          | AFG         | Kabol    | 1780000    |
| 2  | Qandahar       | AFG         | Qandahar | 237500     |
| 3  | Herat          | AFG         | Herat    | 186800     |
| 4  | Mazar-e-Sharif | AFG         | Balkh    | 127800     |
| 8  | London         | GBR         | England  | 7285000    |
| 9  | Birmingham     | GBR         | England  | 1013000    |
| 10 | Manchester     | GBR         | England  | 430000     |

---

### 🧩 Example 2 — NOT IN

```sql
SELECT * FROM city
WHERE countrycode NOT IN ('AFG', 'GBR');
```

**Result:**

| id | name      | countrycode | district      | population |
| -- | --------- | ----------- | ------------- | ---------- |
| 5  | Amsterdam | NLD         | Noord-Holland | 731200     |
| 6  | Rotterdam | NLD         | Zuid-Holland  | 593321     |
| 7  | Utrecht   | NLD         | Utrecht       | 265446     |

---

## 🔍 5️⃣ Search Operator — LIKE

| Operator | Description                                                                   |
| -------- | ----------------------------------------------------------------------------- |
| `LIKE`   | Pattern matching with `%` (wildcard for multiple chars) and `_` (single char) |
| `ILIKE`  | Case-insensitive version (PostgreSQL only)                                    |

---

### 🧩 Example 1 — Names Starting With a Letter

```sql
SELECT * FROM city
WHERE name LIKE 'A%';
```

**Result:**

| id | name      | countrycode | district      | population |
| -- | --------- | ----------- | ------------- | ---------- |
| 5  | Amsterdam | NLD         | Noord-Holland | 731200     |

💡 `%` matches zero or more characters.
So `'A%'` means any name starting with `A`.

---

### 🧩 Example 2 — Names Containing a Pattern

```sql
SELECT * FROM city
WHERE name LIKE '%dam%';
```

**Result:**

| id | name      | countrycode | district      | population |
| -- | --------- | ----------- | ------------- | ---------- |
| 5  | Amsterdam | NLD         | Noord-Holland | 731200     |
| 6  | Rotterdam | NLD         | Zuid-Holland  | 593321     |

💡 `%dam%` matches any name containing “dam”.

---

### 🧩 Example 3 — Case-Insensitive Match (PostgreSQL-only)

```sql
SELECT * FROM city
WHERE name ILIKE 'l%';
```

**Result:**

| id | name   | countrycode | district | population |
| -- | ------ | ----------- | -------- | ---------- |
| 8  | London | GBR         | England  | 7285000    |

💬 `ILIKE` works just like `LIKE`, but ignores case — this is a PostgreSQL extension.

---

# 🧠 Summary Table — WHERE Operators in PostgreSQL

| Category   | Operators                             | Example                                             |
| ---------- | ------------------------------------- | --------------------------------------------------- |
| Comparison | `=`, `<>`, `!=`, `>`, `<`, `>=`, `<=` | `WHERE population > 500000`                         |
| Logical    | `AND`, `OR`, `NOT`                    | `WHERE countrycode = 'AFG' AND population > 100000` |
| Range      | `BETWEEN`                             | `WHERE population BETWEEN 200000 AND 1000000`       |
| Membership | `IN`, `NOT IN`                        | `WHERE countrycode IN ('AFG', 'GBR')`               |
| Search     | `LIKE`, `ILIKE`                       | `WHERE name LIKE 'A%'`                              |

---

# 🚀 Quick Takeaways

✅ `WHERE` filters rows based on specific conditions.
✅ Combine multiple filters with `AND`, `OR`, `NOT`.
✅ Use `IN` for multiple matches, `BETWEEN` for numeric ranges.
✅ Use `LIKE` or `ILIKE` for text pattern searches.
✅ PostgreSQL’s `ILIKE` is a great case-insensitive search tool.

---

Would you like me to continue this SQL tutorial series with the next topic:
👉 **“ORDER BY, LIMIT, and OFFSET in PostgreSQL — Sorting and Paging Data Like a Pro”**?
