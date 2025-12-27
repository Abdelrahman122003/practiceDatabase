## **SQL Joins**

### **Example Tables**

#### **Student**

| sid | sname  | did  |
| --- | ------ | ---- |
| 1   | Ahmed  | 10   |
| 2   | Khalid | 10   |
| 3   | Eman   | 20   |
| 3   | Omar   | NULL |

#### **Dept**

| did | dname |
| --- | ----- |
| 10  | SD    |
| 20  | HR    |
| 30  | IS    |
| 40  | Admin |

---

### **Types of Joins**

**`1. Cross Join`**

- Also called **Cartesian Product**.
- Combines every row from both tables.

**`Query`**

```sql
SELECT s.sname, d.dname
FROM Student s CROSS JOIN Dept d;
```

**`Result`**

- **16 rows** (4 students × 4 departments)

---

**`2. Inner Join`**

- Returns only rows where **PK = FK** matches.

**`Query`**

```sql
SELECT s.sname, d.dname
FROM Student s
INNER JOIN Dept d
    ON s.did = d.did;
```

**`Result`**

- **3 rows** (students belonging to existing departments)

---

**`3. Outer Joins`**

- **Left Outer Join**

  Returns all rows from **left** table + matched rows from right.

- **Right Outer Join**

  Returns all rows from **right** table + matched rows from left.

- **Full Outer Join**

  Returns all rows when there is a match **or not** on both sides.

---

**`4. Self Join`**

Used for **Unary Relationship** (table joins with itself).

### **Employee Example**

| eid | ename | superid |
| --- | ----- | ------- |
| 1   | Ahmed | NULL    |
| 2   | Omar  | 1       |
| 3   | Eman  | 1       |
| 4   | Nada  | 2       |

**`Goal`:** Get employee name and their supervisor name.

**`Query`**

```sql
SELECT E.ename AS Employee,
       S.ename AS Supervisor
FROM Employee E
JOIN Employee S
    ON E.superid = S.eid;
```

---

**`Join With UPDATE`**

```sql
UPDATE Customers
SET last_name = 'Dee'
WHERE age > 22;
```

---

## **Useful SQL Built-In Functions**

- **ISNULL(value, replacement)**
- **COALESCE(value1, value2, ...)**
- **CONVERT(VARCHAR(2), st_age)**
- **CONCAT(a, b, ...)**

---

## **Matching Pattern Operators**

### **Wildcards**

- `_` → **one** character
- `%` → **zero or more** characters

### **Common Patterns**

| Pattern     | Meaning                                      |
| ----------- | -------------------------------------------- |
| `'a%h'`     | starts with **a**, ends with **h**           |
| `'%a_'`     | second last char = **a**, ends with any char |
| `'ahm%'`    | starts with **ahm**                          |
| `'[ahm]%'`  | starts with **a** or **h** or **m**          |
| `'[^ahm]%'` | **does not** start with a or h or m          |
| `'[a-h]%'`  | starts with any char from a → h              |
| `'[^a-h]%'` | does not start with a → h                    |
| `'[364]%'`  | starts with 3 or 6 or 4                      |
| `'%[%]'`    | ends with the **% character**                |
| `'%[_]%'`   | contains `_` as a literal character          |

## Aggregate Functions in SQL

### **Employee Table Example**

| Eid | Ename        | Salary | Address           | did |
| --- | ------------ | ------ | ----------------- | --- |
| 101 | John Carter  | 5200   | New York, USA     | 10  |
| 102 | Maria Lopez  | 4800   | Los Angeles, USA  | 20  |
| 103 | David Smith  | 6100   | Chicago, USA      | 10  |
| 104 | Sarah Brown  | 4500   | Houston, USA      | 30  |
| 105 | James Miller | 7000   | Phoenix, USA      | 40  |
| 106 | Emma Wilson  | 5300   | Philadelphia, USA | 20  |
| 107 | Liam Harris  | 5900   | San Antonio, USA  | 30  |
| 108 | Olivia Clark | 6200   | San Diego, USA    | 40  |
| 109 | Noah Davis   | 4750   | Dallas, USA       | 20  |
| 110 | Ava Turner   | 6800   | San Jose, USA     | 10  |

---

### **Aggregate Functions**

Common SQL aggregate functions include:

- **COUNT()**
- **MAX()**
- **MIN()**
- **AVG()**
- **SUM()**

Examples:

```sql
SELECT SUM(Salary)
FROM employee;

SELECT MIN(Salary), MAX(Salary)
FROM employee;
```

---

### **Queries with GROUP BY**

#### **Example 1**

```sql
SELECT COUNT(eid), address
FROM employee
WHERE did IN (10, 30)
GROUP BY address;
```

**Execution Order:**

1. FROM
2. WHERE
3. GROUP BY
4. SELECT

**Sample Result:**

```
count | address
3     | cairo
3     | alex
4     | manoura
```

---

#### **Example 2**

```sql
SELECT MIN(salary), did
FROM employee
WHERE address LIKE '_a%'
GROUP BY did;
```

**Result:**

```
min   | did
2000  | 10
2000  | 20
1500  | 30
```

---

### **HAVING vs WHERE**

❌ **Incorrect** (WHERE can't be used AFTER GROUP BY):

```sql
SELECT SUM(salary), did
FROM employee
GROUP BY did
WHERE SUM(salary) >= 22000;
```

✔ **Correct**:

```sql
SELECT SUM(salary), did
FROM employee
GROUP BY did
HAVING SUM(salary) >= 22000;
```

**Result:**

```
28000 | 20
```

---

### **Example: HAVING with COUNT**

```sql
SELECT COUNT(eid), address
FROM employee
GROUP BY address
HAVING COUNT(eid) >= 5;
```

**Result:**

```
count | address
6     | cairo
5     | alex
```

---

### **Example: Using HAVING with COUNT After GROUPING**

```sql
SELECT SUM(salary), did
FROM employee
GROUP BY did
HAVING COUNT(eid) > 5;
```

**Result:**

```
sum     | did
20000   | 10
```

---

### **Using WHERE and HAVING Together**

```sql
SELECT SUM(salary), did
FROM employee
WHERE address LIKE '_a%'
GROUP BY did
HAVING SUM(salary) > 12000;
```

**Result:**

```
15000 | 20
14500 | 30
```

---

### **Final Example: MAX + GROUP + HAVING**

```sql
SELECT MAX(salary), address
FROM employee
WHERE did IN (10, 30)
GROUP BY address
HAVING COUNT(eid) > 3;
```

**Result:**

```
8000 | Mansoura
```

## The Difference Between Using **AVG** and **SUM with COUNT**

When calculating the average, you can use the built-in **AVG()** function or manually compute it using **SUM() / COUNT()**. However, the results may differ depending on how **NULL** values are handled.

#### **1. Using AVG()**

```sql
SELECT AVG(ISNULL(st_age, 0))
FROM student;
```

- `AVG()` **ignores NULL values by default**.
- Here, `ISNULL(st_age, 0)` replaces NULL with 0 **before** averaging, which **affects** the result.

---

#### **2. Using SUM() / COUNT()**

```sql
SELECT SUM(st_age) / COUNT(*)   -- COUNT(*) counts all rows (including NULL in st_age)
FROM student;
```

- `SUM(st_age)` **ignores NULL values**, but
- `COUNT(*)` counts **every row**, even if `st_age` is NULL.
- This usually results in a **different (lower)** average compared to `AVG()`.

---

If you want them to match exactly, use:

```sql
SELECT SUM(st_age) / COUNT(st_age)   -- COUNT(st_age) ignores NULL
FROM student;
```

This mimics `AVG(st_age)` behavior.

---

## Subqueries, Performance Notes, Subquery with DML, and Set Operators

### **Subqueries**

A **subquery** is a query written inside another query. The result of the inner query is used as input for the outer query.

**Example:**

```sql
SELECT *
FROM Student
WHERE st_age < (SELECT AVG(st_age) FROM Student);
```

- **Inner Query:**

```sql
SELECT AVG(st_age) FROM Student
```

- **Outer Query:**

```sql
SELECT *
FROM Student
WHERE st_age <
```

In business terms, this answers questions like:

> _“Get all students whose age is less than the average age of all students.”_

---

### **Performance Summary: Subquery vs JOIN**

#### **1. INNER JOIN (Usually Faster)**

- Uses optimized **join algorithms** provided by the query engine.
- Efficiently utilizes **indexes** on join columns (e.g., `dept_id`).
- Avoids repeated evaluation of a subquery.
- Performs better on **large datasets** in most cases.

#### **2. IN (Subquery)**

- The subquery may be **executed separately** or materialized.
- The optimizer _may_ convert it to a join, but this is not guaranteed.
- Can be slower if proper indexes are not available.

---

### **Subquery with DML (DELETE Example)**

Delete records using a direct condition:

```sql
DELETE FROM Stud_Course
WHERE st_id = 1;
```

Delete records using a subquery:

```sql
DELETE FROM Stud_Course
WHERE st_id = (
    SELECT st_id
    FROM Student
    WHERE st_address = 'Cairo'
);
```

Here, the subquery determines which `st_id` values should be deleted.

---

### **Set Operators (UNION Family)**

> **Rules:**

- All SELECT statements must return the **same number of columns**.
- Corresponding columns must have **compatible data types**.

#### **UNION ALL**

- Combines results **without removing duplicates**.

```sql
SELECT st_fname FROM Student
UNION ALL
SELECT ins_name FROM Instructor;
```

---

#### **UNION**

- Combines results and **removes duplicates**.

```sql
SELECT st_fname FROM Student
UNION
SELECT ins_name FROM Instructor;
```

---

#### **INTERSECT**

- Returns rows that exist in **both queries**.

```sql
SELECT st_fname FROM Student
INTERSECT
SELECT ins_name FROM Instructor;
```

---

#### **EXCEPT**

- Returns rows from the **first query only**, excluding those found in the second query.

```sql
SELECT st_fname FROM Student
EXCEPT
SELECT ins_name FROM Instructor;
```

The **EXCEPT** operator performs a **set difference**, returning distinct values that appear in the first result set but not in the second.

## Dating Methods

- **DATE_FORMAT**

  - **What it does**:
    - Formats a date into a specific format as per your requirements.
  - **Example**:
    ```sql
    SELECT DATE_FORMAT(NOW(), '%Y-%m-%d') AS FormattedDate;
    ```
    - `%Y`: Year in 4 digits (e.g., `2024`).
    - `%m`: Month in 2 digits (e.g., `12`).
    - `%d`: Day in 2 digits (e.g., `15`).
  - **Use Case**: To display dates in a user-friendly or consistent format.

- **DATE_SUB('2019-07-27', INTERVAL 30 DAY)**

  - **What it does**:
    - Subtracts 30 days from the given date (`2019-07-27`).
  - **Example**:
    ```sql
    SELECT DATE_SUB('2019-07-27', INTERVAL 30 DAY) AS NewDate;
    ```
    - **Result**: `2019-06-27`.
  - **Use Case**: To calculate a date that is a specific number of days before a given date.

## General Notes

- **COALESCE(Result2.attended_exams, 0)**

  ```sql
  COALESCE(Result2.attended_exams, 0) AS attended_exams  # Step 3: Use COALESCE to ensure 0 is shown if no matching record in Examinations
  ```

- **conditional expression**

  ```sql
  CASE WHEN activity_type = 'end' THEN timestamp ELSE NULL END
  ```

- **Difference Between Where and Having**

  - **`WHERE`** is for filtering rows before aggregation.
  - **`HAVING`** is for filtering groups after aggregation.
