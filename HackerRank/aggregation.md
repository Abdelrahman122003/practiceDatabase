## Hint: The table structure for problems(1 -> 6)

| Field       | Type         |
| ----------- | ------------ |
| ID          | NUMBER       |
| NAME        | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3)  |
| DISTRICT    | VARCHAR2(20) |
| POPULATION  | NUMBER       |

### problem 1 (Revising Aggregations - The Count Function)

Query a count of the number of cities in CITY having a Population larger than 100000.

```sql
select count(*) from CITY where POPULATION > 100000;
```

### problem 2 (Revising Aggregations - The Sum Function)

Query the total population of all cities in CITY where District is California.

```sql
SELECT SUM(POPULATION)
FROM CITY
WHERE DISTRICT = 'California';
```

### problem 3 (Revising Aggregations - The Sum Function)

Query the average population of all cities in CITY where District is California.

```sql
select avg(POPULATION) from CITY where DISTRICT = 'California';
```

### problem 4 (Average Population)

Query the average population for all cities in CITY, rounded down to the nearest integer.

```sql
select floor(avg(POPULATION)) from CITY;
```

### problem 5 (Japan Population)

Query the sum of the populations for all Japanese cities in CITY. The COUNTRYCODE for Japan is JPN.

```SQL
select SUM(POPULATION) from CITY where COUNTRYCODE = 'JPN';
```

### problem 6 (Population Density Difference)

Query the difference between the maximum and minimum populations in CITY.

```SQL
SELECT MAX(POPULATION) - MIN(POPULATION) FROM CITY;
```

## Hint: The table structure for problems(7)

| Column      | Type    |
| ----------- | ------- |
| employee_id | Integer |
| name        | String  |
| months      | Integer |
| salary      | Integer |

### problem 7 (Top Earners)

We define an employee's total earnings to be their monthly (salary \* months)worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.

```sql
SELECT  max(months * salary)  ,  count(*) from Employee
where (salary * months) = (select max(months * salary) from Employee);
```

## Hint: The table structure for problems(8 -> 16)

### problem 8 (Weather Observation Station 2)

Query the following two values from the STATION table:

1- The sum of all values in LAT_N rounded to a scale of decimal places.
2- The sum of all values in LONG_W rounded to a scale of decimal places.

```sql
SELECT ROUND(SUM(LAT_N), 2) , ROUND(SUM(LONG_W), 2) FROM STATION;
```

### problem 9 (Weather Observation Station 13)

Query the sum of Northern Latitudes (LAT_N) from STATION having values greater than 38.7880 and less than 137.2345. Truncate your answer to decimal places.

```SQL
SELECT TRUNCATE(SUM(LAT_N), 4) FROM STATION
WHERE LAT_N >   38.7880 AND LAT_N < 137.2345;
```

### problem 10 (Weather Observation Station 14)

Query the greatest value of the Northern Latitudes (LAT_N) from STATION that is less than 137.2345. Truncate your answer to decimal places.

```SQL
SELECT TRUNCATE(MAX(LAT_N), 4) FROM STATION WHERE LAT_N < 137.2345;
```

### problem 11 (Weather Observation Station 15)

Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to decimal places.

```sql
SELECT ROUND(LONG_W, 4) FROM STATION WHERE LAT_N < 137.2345 ORDER BY LAT_N DESC LIMIT 1;
```

### problem 12 (Weather Observation Station 16)

Query the smallest Northern Latitude (LAT_N) from STATION that is greater than 38.7780 . Round your answer to decimal places.

```sql
SELECT ROUND(LAT_N,4) FROM STATION WHERE LAT_N > 38.7780
ORDER BY LAT_N ASC LIMIT 1;
```

### problem 13 (Weather Observation Station 17)

Query the Western Longitude (LONG_W)where the smallest Northern Latitude (LAT_N) in STATION is greater than 38.7780. Round your answer to 4 decimal places.

```sql
SELECT ROUND(LONG_W, 4) FROM STATION  WHERE LAT_N > 38.7780 ORDER BY LAT_N ASC LIMIT 1 ;
```

### problem 14 (Weather Observation Station 18)

Consider P1(a, b)and P2(c, d) to be two points on a 2D plane.

a happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
b happens to equal the minimum value in Western Longitude (LONG_W in STATION).
c happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
d happens to equal the maximum value in Western Longitude (LONG_W in STATION).

Query the Manhattan Distance between points and and round it to a scale of 4 decimal places.

```SQL
-- X1 -> A , X2 -> C
-- Y1 -> B , Y2 -> D
-- DISTANCE = |X1 - X2| + |Y1 - Y2|
SELECT ROUND(ABS(MIN(LAT_N) - MAX(LAT_N)) + ABS(MIN(LONG_W) - MAX(LONG_W)), 4) FROM  STATION;
```

### problem 15 (Weather Observation Station 19)

Consider P1(a, c) and P2(b, d) to be two points on a 2D plane where (a, b) are the respective minimum and maximum values of Northern Latitude (LAT_N) and (c, d) are the respective minimum and maximum values of Western Longitude (LONG_W) in STATION.

Query the Euclidean Distance between points and and format your answer to display 4 decimal digits.

```sql
-- euclidean distance = sqrt((x1  - x2) * (x1 - x2) + (y1 - y2) * (y1, y2))
-- x1 -> a
-- x2 -> b
-- y1 -> c
-- y2 -> d
SELECT ROUND(SQRT( (MIN(LAT_N) - MAX(LAT_N)) * (MIN(LAT_N) - MAX(LAT_N)) + (MIN(LONG_W) - MAX(LONG_W)) * (MIN(LONG_W) - MAX(LONG_W))), 4) FROM STATION;
```

### problem 16 (Weather Observation Station 20)

## Hint: The table structure for problems(8 -> 16)

| Column | Type    |
| ------ | ------- |
| ID     | Integer |
| Name   | String  |
| Salary | Integer |

### problem 17 (The Blunder)

Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's 0 key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.: actual - miscalculation average monthly salaries), and round it up to the next integer.

```sql
SELECT CEIL(AVG(Salary) - AVG( REPLACE(Salary, 0, '')) ) FROM EMPLOYEES ;
```
