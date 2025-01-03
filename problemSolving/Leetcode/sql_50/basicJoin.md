### PROBLEM NAME: 577. Employee Bonus

```sql
-- Select employee name and bonus amount
SELECT
    e.name,            -- Select employee name from Employee table
    b.bonus           -- Select bonus from Bonus table
FROM
    Employee e         -- From the Employee table aliased as 'e'
LEFT OUTER JOIN
    Bonus b            -- Left outer join with the Bonus table aliased as 'b'
ON
    e.empId = b.empId  -- Join condition: matching employee IDs from both tables
WHERE
    b.bonus < 1000     -- Filter for bonus less than 1000
    OR b.bonus IS NULL -- Include employees without bonuses (due to LEFT JOIN)

```

### 1280. Students and Examinations

```sql
-- Step 1: Generate all combinations of students and subjects using CROSS JOIN
-- This will create a Cartesian product where each student is paired with every subject.
SELECT
    Result1.student_id,
    Result1.student_name,
    Result1.subject_name,
    COALESCE(Result2.attended_exams, 0) AS attended_exams  -- Step 3: Use COALESCE to ensure 0 is shown if no matching record in Examinations
FROM
(
    -- Step 1: Creating Result1 by CROSS JOIN between Students and Subjects
    -- This step produces all possible combinations of student_id and subject_name.
    SELECT
        s.student_id,
        s.student_name,
        sub.subject_name
    FROM
        Students s
    CROSS JOIN
        Subjects sub
) AS Result1

-- Step 2: Aggregate data from Examinations table
-- Group the data by student_id and subject_name, counting how many times each student has attended a specific subject.
LEFT OUTER JOIN
(
    SELECT
        e.student_id,
        e.subject_name,
        COUNT(e.subject_name) AS attended_exams  -- Counting the number of times each student attended a subject
    FROM
        Examinations e
    GROUP BY
        e.student_id,
        e.subject_name  -- Grouping by student_id and subject_name to get the count for each subject
) AS Result2
ON Result1.student_id = Result2.student_id
    AND Result1.subject_name = Result2.subject_name  -- Join on student_id and subject_name to match the records

-- Step 3: Final result is ordered by student_id, student_name, and subject_name
ORDER BY
    student_id,
    student_name,
    subject_name;
```

### 570. Managers with at Least 5 Direct Reports

```sql
-- Select the names of employees who are managers and have at least 5 direct reports.
SELECT
    e.name -- Fetch the name of the employee who meets the criteria.
FROM
    Employee e -- The main Employee table is referred to as 'e'.
INNER JOIN
(
    -- Create a subquery to count the number of employees reporting to each manager.
    SELECT
        *, -- Select all columns from the Employee table (not ideal; specify columns if possible).
        COUNT(e.managerId) AS report_num -- Count the number of employees for each manager.
    FROM
        Employee e -- Use the Employee table to group data by managers.
    GROUP BY
        managerId -- Group employees by their manager's ID to calculate the count.
) AS Mang_report -- Alias the subquery result as 'Mang_report'.
ON
    e.id = Mang_report.managerId -- Match the manager's ID in the main table with the IDs in the subquery.
    AND
    Mang_report.report_num >= 5; -- Only include managers with 5 or more direct reports.
```

### 1661. Average Time of Process per Machine

**`Solution1`**

```sql
SELECT
    machine_id,  -- Select the machine_id to calculate the average processing time for each machine
    ROUND(AVG(end_time - start_time), 3) AS processing_time  -- Calculate the average processing time for each machine, rounded to 3 decimal places
FROM

    (
        SELECT
            machine_id,  -- Group by machine_id to calculate times for each machine
            process_id,  -- Group by process_id to ensure we handle each process separately
            MAX(CASE WHEN activity_type = 'end' THEN timestamp ELSE NULL END) AS end_time,  -- Get the 'end' timestamp (using MAX to handle possible multiple 'end' entries for each process)
            MAX(CASE WHEN activity_type = 'start' THEN timestamp ELSE NULL END) AS start_time  -- Get the 'start' timestamp (using MAX to handle multiple 'start' entries)
        FROM
            Activity  -- The main table containing machine activity data
        GROUP BY
            machine_id,  -- Group by machine_id to calculate the processing time for each machine
            process_id  -- Group by process_id to ensure we get time calculations for each process separately
    ) AS ProcessTimes  -- Subquery that calculates start and end times for each process on each machine
GROUP BY
    machine_id;  -- Group by machine_id to calculate the overall average processing time per machine
```

**`Solution2`**

```sql
SELECT
    a.machine_id,  -- Select the machine_id (the final result will be grouped by machine_id)
    ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time  -- Calculate the average processing time per machine, rounded to 3 decimal places
FROM
    Activity a,  -- Alias 'a' for the Activity table (represents the 'start' activity)
    Activity b   -- Alias 'b' for the Activity table (represents the 'end' activity)
WHERE
    a.machine_id = b.machine_id  -- Join the two instances of the Activity table on machine_id
AND
    a.process_id = b.process_id  -- Join the two instances of the Activity table on process_id (same process)
AND
    a.activity_type = 'start'
AND
    b.activity_type = 'end'
GROUP BY machine_id;  -- Group the result by machine_id to calculate the average processing time per machine

```

**`Solution3`**

```sql
SELECT
    machine_id,
    ROUND(
        SUM(
            CASE
                WHEN activity_type = 'start'
                THEN timestamp * -1  -- Multiply the 'start' timestamp by -1 to handle negative times
                ELSE timestamp       -- Leave the 'end' timestamp as it is
            END
        ) * 1.0 / (  -- Multiply by 1.0 to ensure floating point division
            (
                SELECT COUNT(DISTINCT process_id)  -- Count the number of unique processes across all activities
                FROM Activity
            )
        , 3) AS processing_time  -- Round the result to 3 decimal places
FROM
    Activity
GROUP BY
    machine_id;  -- Group by machine_id to calculate processing time for each machine
```
