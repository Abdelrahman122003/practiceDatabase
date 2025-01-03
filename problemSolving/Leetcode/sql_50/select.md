### 584. Find Customer Referee

```sql
SELECT
    name
FROM
    Customer c
WHERE
    c.referee_id != 2 OR c.referee_id IS NULL;
```

### Big Countries

```sql
SELECT
    w.name,
    w.population,
    w.area
FROM
    World w
WHERE
    w.area >= 3000000
    OR  w.population >= 25000000;

```

### 1148. Article Views I

```sql
SELECT
    DISTINCT v.author_id AS id
FROM
    Views v
WHERE
    v.author_id = v.viewer_id
ORDER BY v.author_id;
```

### 1683. Invalid Tweets

```sql
SELECT
    t.tweet_id
FROM
    Tweets t
WHERE
    LENGTH(t.content) > 15;
```
