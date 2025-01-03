The **PARTITION BY** function is a powerful SQL feature used in **window functions**. It allows you to divide the result set into partitions (subsets of rows) to apply calculations on each partition independently. Unlike the **GROUP BY** clause, which aggregates rows into a single row per group, **PARTITION BY** retains individual rows in the result while applying aggregate or window functions over partitions.

### Syntax of PARTITION BY:

```sql
<window_function>() OVER (PARTITION BY column_name ORDER BY column_name)
```

### Key Components:

1. **PARTITION BY**: Divides the data into partitions based on the values in specified columns.
2. **ORDER BY**: Determines the order of rows within each partition (optional).
3. **Window Function**: Functions like `ROW_NUMBER()`, `RANK()`, `SUM()`, `AVG()`, etc., that are applied to each partition.

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
