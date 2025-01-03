## Hint: The table structure for problems(1 -> 3)

The CITY and COUNTRY tables are described as follows:

| Field       | Type          |
| ----------- | ------------- |
| ID          | NUMBER        |
| NAME        | VARCHAR2 (17) |
| COUNTRYCODE | VARCHAR2 (3)  |
| DISTRICT    | VARCHAR2 (20) |
| POPULATION  | NUMBER        |

### problem 1 (Population Census)

Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

```sql
SELECT SUM(C.POPULATION) FROM CITY C
INNER JOIN  COUNTRY CO ON C.COUNTRYCODE  = CO.Code
AND CO.CONTINENT  = 'Asia';
```

### problem 2 (Population Census)

Given the CITY and COUNTRY tables, query the names of all cities where the CONTINENT is 'Africa'.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

```sql
SELECT C.NAME FROM CITY C
INNER JOIN COUNTRY CO ON C.COUNTRYCODE = CO.Code AND
CO.CONTINENT = 'Africa';
```

### problem 3 (Average Population of Each Continent)

Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns

```SQL
SELECT  CO.Continent, FLOOR(AVG(C.POPULATION)) FROM CITY C
INNER JOIN COUNTRY CO ON C.COUNTRYCODE = CO.Code
GROUP BY CO.Continent;
```

### problem 4 (The Report)

https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=true

```SQL
SELECT
    CASE
        WHEN g.Grade < 8 THEN 'NULL'
        ELSE s.Name
    END AS Name,
    g.Grade,
    s.Marks
FROM
    Students s
INNER JOIN
    Grades g
    ON g.Min_Mark <= s.Marks AND g.Max_Mark >= s.Marks
ORDER BY g.Grade DESC,
CASE
    WHEN g.Grade >= 8 THEN  s.Name
    ELSE s.Marks
END  ASC;
```

### problem 5 (Contest Leaderboard)

https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=true

```sql
SELECT h.hacker_id, h.name, SUM(submission_with_max_score.max_score) AS total_score
FROM Hackers h
INNER JOIN (
    SELECT s.hacker_id, s.challenge_id, MAX(s.score) AS max_score
    FROM Submissions s
    GROUP BY s.hacker_id, s.challenge_id
) AS submission_with_max_score ON h.hacker_id = submission_with_max_score.hacker_id
GROUP BY h.hacker_id, h.name
HAVING total_score != 0
ORDER BY total_score DESC, h.hacker_id ASC;
```

### problem 6 (Top Competitors)

https://www.hackerrank.com/challenges/full-score/problem?isFullScreen=true

```sql
SELECT
    h.hacker_id,
    h.name
FROM
    (
        SELECT
            s.hacker_id,
            COUNT(*) AS num_challenges
        FROM
            Submissions s
        JOIN
            Challenges c
        ON
            s.challenge_id = c.challenge_id
        JOIN
            Difficulty d
        ON
            c.difficulty_level = d.difficulty_level
        WHERE
            s.score = d.score
        GROUP BY
            s.hacker_id
        HAVING COUNT(*) > 1
    ) AS hf
JOIN
    Hackers h
ON
    hf.hacker_id = h.hacker_id
ORDER BY
    hf.num_challenges DESC,
    h.hacker_id ASC;
```

### problem 7 (Challenges)

https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=true

```sql
WITH hacker_num_challenge AS (
    SELECT
        c.hacker_id,
        COUNT(*) AS total_challenges
    FROM
        Challenges c
    GROUP BY
        c.hacker_id
)
SELECT
    h.hacker_id,
    hacker.name,
    h.total_challenges
FROM
    hacker_num_challenge h
JOIN
    Hackers hacker
ON
    hacker.hacker_id = h.hacker_id
WHERE
    h.total_challenges = (
        SELECT
            MAX(total_challenges)
        FROM
            hacker_num_challenge
    )
    OR
    h.total_challenges IN (
        SELECT
            total_challenges
        FROM
            hacker_num_challenge
        GROUP BY
            total_challenges
        HAVING
            COUNT(*) = 1
    )
ORDER BY
    h.total_challenges DESC,
    h.hacker_id ASC;
```

### problem 8 (Ollivander's Inventory)

https://www.hackerrank.com/challenges/harry-potter-and-wands/problem?isFullScreen=true

```sql
WITH wands_with_properties AS (
    SELECT
        w.id,
        wp.age,
        w.coins_needed,
        w.power,
        w.code,
        wp.is_evil
    FROM
        Wands w
    JOIN
        Wands_Property wp
    ON
        w.code = wp.code
)
SELECT
    w.id,
    w.age,
    w.coins_needed,
    w.power
FROM
    wands_with_properties w
WHERE
    w.coins_needed = (
        SELECT MIN(w1.coins_needed)
        FROM wands_with_properties w1
        WHERE
            w1.code = w.code
            AND w1.age = w.age
            AND w1.power = w.power
    )
    AND
    w.is_evil = 0
ORDER BY
    w.power DESC,
    w.age DESC;
```
