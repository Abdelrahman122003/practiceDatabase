## Ranking Functions

**Table example to use it;**

`Employee table`:

```txt
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```

### Row_Number() Function

`ROW_NUMBER()` assigns a unique, sequential number to each row in a result set based on the specified ordering, starting from 1. It is commonly used for ranking, pagination, and identifying individual rows within ordered data.

**`Question: `**

I want to display each employee’s rank based on their salary, with the ranking increasing incrementally.

`Query`:

```sql
SELECT *,
          Row_Number() Over (ORDER BY Salary DESC) AS rn
FROM Employee;
```

`Output`:

```TXT
| id | name  | salary | departmentId | rn |
| -- | ----- | ------ | ------------ | -- |
| 4  | Max   | 90000  | 1            | 1  |
| 1  | Joe   | 85000  | 1            | 2  |
| 6  | Randy | 85000  | 1            | 3  |
| 2  | Henry | 80000  | 2            | 4  |
| 7  | Will  | 70000  | 1            | 5  |
| 5  | Janet | 69000  | 1            | 6  |
| 3  | Sam   | 60000  | 2            | 7  |
```

### Dense_Rank()

`DENSE_RANK()` assigns a rank to each row based on the specified order, where rows with the same values receive the same rank, and no rank numbers are skipped.

**`Question: `**

How can I display employee rankings based on their salaries, where employees with the same salary share the same rank?

`Query`:

```sql
SELECT *,
          Dense_Rank() Over (ORDER BY Salary DESC) AS rn
FROM Employee;
```

`Output`:

```TXT

| id | name  | salary | departmentId | rn  |
| -- | ----- | ------ | ------------ | --- |
| 4  | Max   | 90000  | 1            | 1   |
| 1  | Joe   | 85000  | 1            | 2   |
| 6  | Randy | 85000  | 1            | 2   |
| 2  | Henry | 80000  | 2            | 3   |
| 7  | Will  | 70000  | 1            | 4   |
| 5  | Janet | 69000  | 1            | 5   |
| 3  | Sam   | 60000  | 2            | 6   |
```

### NTile()

`NTILE(N)` divides the result set into N approximately equal groups and assigns a group number to each row based on the specified ordering. It is commonly used for data segmentation, such as quartiles or percentiles.

**`Question`**:

Divide the Employee table into three groups based on a specified ordering.

`Query:`

```sql
SELECT *,
    NTile(3) Over (ORDER BY Salary DESC) AS rn
FROM Employee;
```

`Output:`

```TXT
| id | name  | salary | departmentId | G |
| -- | ----- | ------ | ------------ | - |
| 4  | Max   | 90000  | 1            | 1 |
| 1  | Joe   | 85000  | 1            | 1 |
| 6  | Randy | 85000  | 1            | 1 |
| 2  | Henry | 80000  | 2            | 2 |
| 7  | Will  | 70000  | 1            | 2 |
| 5  | Janet | 69000  | 1            | 3 |
| 3  | Sam   | 60000  | 2            | 3 |
```

### Rank()

RANK() assigns a ranking to rows based on the specified ordering, where rows with equal values receive the same rank, but subsequent ranks are skipped.

**`Question`**:

Show each employee’s rank according to salary, where employees with the same salary receive the same rank and ranking numbers may be skipped.

`Query:`

```sql
SELECT *,
          RANK() Over (ORDER BY Salary DESC) AS G
FROM Employee;
```

`Output:`

```TXT
| id | name  | salary | departmentId | rn|
| -- | ----- | ------ | ------------ | - |
| 4  | Max   | 90000  | 1            | 1 |
| 1  | Joe   | 85000  | 1            | 2 |
| 6  | Randy | 85000  | 1            | 2 |
| 2  | Henry | 80000  | 2            | 4 |
| 7  | Will  | 70000  | 1            | 5 |
| 5  | Janet | 69000  | 1            | 6 |
| 3  | Sam   | 60000  | 2            | 7 |
```

## PARTITION

The **PARTITION BY** function is a powerful SQL feature used in **window functions**. It allows you to divide the result set into partitions (subsets of rows) to apply calculations on each partition independently. Unlike the **GROUP BY** clause, which aggregates rows into a single row per group, **PARTITION BY** retains individual rows in the result while applying aggregate or window functions over partitions.

### Syntax of PARTITION BY:

```sql
<window_function>() OVER (PARTITION BY column_name ORDER BY column_name)
```

### Key Components:

1. **PARTITION BY**: Divides the data into partitions based on the values in specified columns.
2. **ORDER BY**: Determines the order of rows within each partition (optional).
3. **Window Function**: Functions like `ROW_NUMBER()`, `RANK()`, `SUM()`, `AVG(), Group_concat()`, etc., that are applied to each partition.

---

### Example Use Cases:

#### 1. Rank Employees by Department

```sql
SELECT
    employee_id,
    department_id,
    salary,
    RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rank
FROM employees;
```

- **PARTITION BY department_id**: Groups rows by department.
- **RANK()**: Assigns a rank to each employee within their department based on salary.

---

#### 2. Calculate Running Total (Cumulative Sum)

```sql
SELECT
    customer_id,
    order_date,
    amount,
    SUM(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS running_total
FROM orders;
```

- **PARTITION BY customer_id**: Divides rows by customer.
- **ORDER BY order_date**: Ensures the cumulative total is calculated in chronological order.

---

#### 3. Find Maximum Salary in Each Department

```sql
SELECT
    employee_id,
    department_id,
    salary,
    MAX(salary) OVER (PARTITION BY department_id) AS max_salary_in_dept
FROM employees;
```

- **MAX(salary)**: Calculates the maximum salary for each department.
- **PARTITION BY department_id**: Groups rows by department without collapsing them.

---

### Differences Between GROUP BY and PARTITION BY:

| Feature           | GROUP BY                           | PARTITION BY                |
| ----------------- | ---------------------------------- | --------------------------- |
| Aggregates Rows   | Collapses rows into one per group. | Retains individual rows.    |
| Window Functions  | Not applicable.                    | Used with window functions. |
| Result Visibility | One row per group.                 | All rows remain visible.    |

---

### Advantages of PARTITION BY:

- Useful for window functions that need partition-level operations without collapsing rows.
- Works seamlessly with ranking, aggregate, and analytical calculations.
- Enables advanced querying for reports and analytics.

Let me know if you'd like further examples or deeper exploration of specific scenarios!
