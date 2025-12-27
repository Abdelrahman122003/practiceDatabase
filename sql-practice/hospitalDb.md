App => https://www.sql-practice.com/

**Database Relations**

<p align="center">
<img src="./images/hospitalRelations.png" width="300" height="350">
</p>

**Question:**
Each admission costs `$50` for patients without insurance and `$10` for patients with insurance.
Patients with an **even patient_id** have insurance.
Show whether each patient has insurance (`Yes` or `No`) and calculate the total admission cost for each group.

**My Solution:**
I used a `CASE` statement to determine insurance status and another `CASE` inside `SUM` to calculate costs.

```sql
SELECT
  CASE
    WHEN patient_id % 2 = 0 THEN 'Yes'
    ELSE 'No'
  END AS has_insurance,
  SUM(
    CASE
      WHEN patient_id % 2 = 0 THEN 10
      ELSE 50
    END
  ) AS cost_after_insurance
FROM admissions
GROUP BY has_insurance;
```

---

**Question:**
Show the full province names where the number of male patients (`M`) is greater than the number of female patients (`F`).

**My Solution:**
I counted males and females per province using conditional aggregation inside a subquery.

```sql
SELECT province_name
FROM (
  SELECT pn.province_name,
         SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
         SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count
  FROM patients p
  JOIN province_names pn ON p.province_id = pn.province_id
  GROUP BY pn.province_name
)
WHERE male_count > female_count;
```

**Optimized Solution:**
Using `HAVING` removes the need for a subquery and makes the query simpler.

```sql
SELECT pr.province_name
FROM patients pa
JOIN province_names pr ON pa.province_id = pr.province_id
GROUP BY pr.province_name
HAVING
  COUNT(CASE WHEN gender = 'M' THEN 1 END) >
  COUNT(CASE WHEN gender = 'F' THEN 1 END);
```

---

**Question:**
Show the percentage of patients whose gender is `M`.
Round the result to **two decimal places** and display it as a percentage.

**My Solution:**
I used `AVG` with a boolean condition and converted it to a percentage.

```sql
SELECT
  ROUND(100 * AVG(gender = 'M'), 2) || '%' AS percent_of_male_patients
FROM patients;
```

---

**Question:**
Sort the province names in ascending order, but make sure the province **Ontario** always appears at the top.

**My Solution:**
I used a conditional expression in the `ORDER BY` clause to prioritize Ontario before applying alphabetical sorting.

**Statement:**
SQL sorts `ORDER BY` from left to right, so this query puts Ontario first using a priority condition, then sorts all provinces alphabetically.

```sql
SELECT province_name
FROM province_names
ORDER BY (province_name = 'Ontario') DESC, province_name ASC;
```
