## Hint: The table structure for problems(1)

| Column | Type    |
| ------ | ------- |
| A      | Integer |
| B      | Integer |
| C      | Integer |

### Problem 1 (Type of Triangle)

Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:

Equilateral: It's a triangle with sides of equal length.
Isosceles: It's a triangle with sides of equal length.
Scalene: It's a triangle with sides of differing lengths.
Not A Triangle: The given values of A, B, and C don't form a triangle.

```sql
select
case
    when (t.a + t.b <= t.c or t.b + t.c <= t.a or t.a + t.c <= t.b) then "Not A Triangle"
    when (t.a = t.b and t.a = t.c) then "Equilateral"
    when (t.a = t.b or t.a = t.c or t.b = t.c) then "Isosceles"
    when (t.a != t.b and t.a != t.c and t.b != t.c) then "Scalene"
 END AS TriangleType
from TRIANGLES t;
```

## Hint: The table structure for problems(2)

| Column     | Type   |
| ---------- | ------ |
| Name       | String |
| Occupation | String |

### Problem 2 (The PADS)

Generate the following two result sets:

1- Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).

2- Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format:

There are a total of [occupation_count] [occupation]s.
where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

```sql
-- first query
Select concat(Name , '(' ,  Left(Occupation, 1) , ')')   from OCCUPATIONS order by Name;
-- second query
Select concat("There are a total of ",  count(Occupation) , " ", Lower(Occupation), "s.") from OCCUPATIONS group by Occupation order by count(Occupation) , Occupation ;
```

## Hint: The table structure for problems(3)

| Column | Type    |
| ------ | ------- |
| N      | Integer |
| P      | Integer |

### Problem 3 (Binary Tree Nodes)

ou are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.

Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

Root: If node is root node.
Leaf: If node is leaf node.
Inner: If node is neither root nor leaf node.

```sql
select b.N,
case
--  root --> no has parent
        when b.P is NULL then "Root"
--      leaf --> not parent + has parent
        when b.P is not  NULL  and (select count(*) from BST where P = b.N) = 0 then "Leaf"
        Else "Inner"
End
from BST b
order by b.N;
```

### Problem 4 (New Companies)

https://www.hackerrank.com/challenges/the-company/problem?isFullScreen=true

```sql
SELECT c.company_code,
        c.founder,
        COUNT(DISTINCT e.lead_manager_code),
        COUNT(DISTINCT e.senior_manager_code),
        COUNT(DISTINCT e.manager_code),
        COUNT(DISTINCT e.employee_code)
FROM Company c
INNER JOIN Employee e
ON c.company_code = e.company_code
GROUP BY c.company_code, c.founder
ORDER BY c.company_code ASC;
```
