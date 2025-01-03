### Problem 1(Placements)

https://www.hackerrank.com/challenges/placements/problem?isFullScreen=true

**`Solution`**

```sql
SELECT
    s.name
FROM
(
    SELECT
        f.ID AS id,
        p1.Salary as salary
    FROM
        Friends f
    JOIN
        Packages p
    ON
        f.ID = p.ID
    JOIN
        Packages p1
    ON
        f.Friend_ID = p1.ID
    WHERE
        p.Salary < p1.Salary
)AS subQuery
JOIN
    Students s
ON
    s.ID = subQuery.id
ORDER BY subQuery.salary;
```

### Problem 2(Placements)

https://www.hackerrank.com/challenges/symmetric-pairs/problem?isFullScreen=true

**`Solution`**

```sql
SELECT
    f.x AS X1,
    f.y AS Y1
FROM
    Functions f
JOIN
    Functions f1
ON
    f.x = f1.y
    AND
    f1.x = f.y
GROUP BY f.x, f.y
HAVING COUNT(f.x) > 1 OR f.x < f.y
ORDER BY
    X1;
```

### Problem 3(Project Planning)

https://www.hackerrank.com/challenges/sql-projects/problem?isFullScreen=true

**`Solution`**

```sql
WITH projects_start_dates AS (
    SELECT
        Start_Date,
        RANK() OVER (ORDER BY Start_Date) AS sd_rank
    FROM
        Projects
    WHERE
        Start_Date NOT IN (
            SELECT
                End_Date
            FROM
                Projects
        )
),
projects_end_dates AS (
    SELECT
        End_Date,
        RANK() OVER (ORDER BY End_Date) AS ed_rank
    FROM
        Projects
    WHERE
        End_Date NOT IN (
            SELECT
                Start_Date
            FROM
                Projects
        )
)
SELECT
    psd.Start_Date,
    ped.End_Date
FROM
    projects_start_dates psd
JOIN
    projects_end_dates ped
ON
    psd.sd_rank = ped.ed_rank
ORDER BY
    DATEDIFF(DAY, psd.Start_Date, ped.End_Date),
    psd.Start_Date;

```
