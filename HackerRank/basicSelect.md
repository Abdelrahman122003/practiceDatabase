## Hint: The table structure for problems(1 -> 6)

The CITY table is described as follows:

| Field       | Type         |
| ----------- | ------------ |
| ID          | NUMBER       |
| NAME        | VARCHAR2(17) |
| COUNTRYCODE | VARCHAR2(3)  |
| DISTRICT    | VARCHAR2(20) |
| POPULATION  | NUMBER       |

## Problem 1 (Revising the Select Query I)

Query all columns for all American cities in the CITY table with populations larger than 100000. The CountryCode for America is USA.

```sql
select * from CITY where POPULATION > 100000 and COUNTRYCODE = "USA";
```

## Problem 2 (Revising the Select Query II)

Query the NAME field for all American cities in the CITY table with populations larger than 120000. The CountryCode for America is USA.

```sql
select NAME from  CITY where POPULATION > 120000 and COUNTRYCODE = "USA";
```

## Problem 3 (Select All)

Query all columns (attributes) for every row in the CITY table.

```sql
select * from CITY;
```

## Problem 4 (Select By ID)

Query all columns for a city in CITY with the ID 1661.

```sql
select * from CITY where ID = 1661;
```

## Problem 5 (Japanese Cities' Attributes)

Query all attributes of every Japanese city in the CITY table. The COUNTRYCODE for Japan is JPN.

```sql
select * from CITY where COUNTRYCODE = "JPN";
```

## Problem 6 (Japanese Cities' Names)

Query the names of all the Japanese cities in the CITY table. The COUNTRYCODE for Japan is JPN.

```sql
select NAME from CITY where COUNTRYCODE = "JPN";
```

## Hint: The table structure for problems(7 -> 19)

The STATION table is described as follows:

| Field  | Data Type    |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

## Problem 7 (Weather Observation Station 1)

Query a list of CITY and STATE from the STATION table.

```sql
select CITY, STATE from STATION;
```

## Problem 8 (Weather Observation Station 2)

## Problem 9 (Weather Observation Station 3)

Query a list of CITY names from STATION for cities that have an even ID number. Print the results in any order, but exclude duplicates from the answer.

```sql
SELECT DISTINCT CITY FROM STATION WHERE ID%2=0;
```

## Problem 11 (Weather Observation Station 4)

Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.

```sql
select count(*)  - count(distinct CITY) from STATION;
```

### Problem 12 (Weather Observation Station 5)

Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

```sql
SELECT CITY C, LENGTH(CITY) L FROM STATION ORDER BY L ASC , C ASC LIMIT 1;
SELECT CITY C, LENGTH(CITY) L FROM STATION order by L  DESC, C ASC LIMIT 1;
```

### Problem 13 (Weather Observation Station 6)

Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE 'a%' OR CITY LIKE 'o%' or CITY LIKE 'i%' or CITY LIKE 'u%' or CITY LIKE 'e%';
```

### Problem 14 (Weather Observation Station 7)

Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE '%a' or CITY LIKE '%e' or CITY LIKE '%i' or CITY LIKE '%o' or
CITY LIKE '%u';
```

### Problem 15 (Weather Observation Station 8)

Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE
(CITY LIKE 'a%' or CITY LIKE 'e%' or CITY LIKE 'i%' or CITY LIKE 'o%' or CITY LIKE 'u%')
and
(CITY LIKE '%a' or CITY LIKE '%e' or CITY LIKE '%i' or CITY LIKE '%o' or CITY LIKE '%u');

-- SELECT DISTINCT CITY
-- FROM STATION
-- WHERE CITY LIKE '[aeiouAEIOU]%[aeiouAEIOU]';
```

### Problem 16 (Weather Observation Station 9)

Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE
not (CITY LIKE 'a%' or  CITY LIKE 'e%' or  CITY LIKE 'i%' or  CITY LIKE 'o%' or CITY LIKE 'u%');
```

### Problem 17 (Weather Observation Station 10)

Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE
not (CITY LIKE '%a' or  CITY LIKE '%e' or  CITY LIKE '%i' or  CITY LIKE '%o' or CITY LIKE '%u');
```

### Problem 18 (Weather Observation Station 11)

Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE
not (CITY LIKE 'a%' or  CITY LIKE 'e%' or  CITY LIKE 'i%' or  CITY LIKE 'o%' or CITY LIKE 'u%')
or
not (CITY LIKE '%a' or  CITY LIKE '%e' or  CITY LIKE '%i' or  CITY LIKE '%o' or CITY LIKE '%u');
```

### Problem 19 (Weather Observation Station 12)

Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY FROM STATION WHERE
not (CITY LIKE 'a%' or  CITY LIKE 'e%' or  CITY LIKE 'i%' or  CITY LIKE 'o%' or CITY LIKE 'u%')
and
not (CITY LIKE '%a' or  CITY LIKE '%e' or  CITY LIKE '%i' or  CITY LIKE '%o' or CITY LIKE '%u');
```

## Hint: The table structure for problems(20)

| Column | Type    |
| ------ | ------- |
| ID     | Integer |
| Name   | String  |
| Marks  | Integer |

### Problem 20 (Higher Than 75 Marks)

Query the Name of any student in STUDENTS who scored higher than Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

```sql
SELECT Name from STUDENTS WHERE Marks > 75 order by right(Name, 3) asc,   ID asc ;
```

## Hint: The table structure for problems(21 and 22)

| Column      | Type    |
| ----------- | ------- |
| employee_id | Integer |
| name        | String  |
| months      | Integer |
| salary      | Integer |

### Problem 21 (Employee Names)

Write a query that prints a list of employee names (i.e.: the name attribute) from the Employee table in alphabetical order.

```sql
Select name from Employee order by name asc;
```

### Problem 22 (Employee Salaries)

Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than per month who have been employees for less than months. Sort your result by ascending employee_id.

```sql
select name from Employee where salary > 2000 and months < 10 order by employee_id asc;
```
