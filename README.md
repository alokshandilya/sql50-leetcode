# Top SQL 50 Study Plan (LeetCode)

This repository contains solutions to the **Top SQL 50 Study Plan** from [LeetCode](https://leetcode.com/studyplan/top-sql-50/) using **PostgreSQL**. Each problem is linked to its corresponding LeetCode page, and solutions are provided in PostgreSQL syntax.

## Select

#### 1. [Recyclable and Low Fat Products (1757)](https://leetcode.com/problems/recyclable-and-low-fat-products/)

```sql
SELECT
    product_id
FROM
    Products
WHERE
    low_fats = 'Y'
    AND
    recyclable = 'Y';
```

#### 2. [Find Customer Referee (584)](https://leetcode.com/problems/find-customer-referee/)

```sql
SELECT
    name
FROM
    Customer
WHERE
    referee_id <> 2
    OR
    referee_id IS NULL;
```

> **NOTE:** The `<>`, `!=` operator is used to compare two values and return `TRUE` if they are not equal, and `FALSE` otherwise.

```sql
SELECT
    NULL <> 2
        AS result;

-- output: NULL
```

#### 3. [Big Countries (595)](https://leetcode.com/problems/big-countries/)

```sql
SELECT
    name,
    population,
    area
FROM
    World
WHERE
    area >= 3000000
    OR
    population >= 25000000;
```

#### 4. [Article Views I (1148)](https://leetcode.com/problems/article-views-i/)

```sql
SELECT
    DISTINCT
        author_id AS id
FROM
    Views
WHERE
    author_id = viewer_id
ORDER BY
    id;  -- or 1 (1st selected column) author_id/id)
```

#### 5. [Invalid Tweets (1683)](https://leetcode.com/problems/invalid-tweets/)

```sql
SELECT
    tweet_id
FROM
    Tweets
WHERE
    LENGTH(content) > 15;
```

> [PostgreSQL string functions](https://www.postgresql.org/docs/current/functions-string.html) docs  
> The `LENGTH()` function returns the length of a string in characters.
>
> ```
>  length (text) → integer
>      Returns the number of characters in the string.
>      length('jose') → 4
> ```

## Basic Joins

> JOINs are used to combine rows from two or more tables based on a related column between them.
>
> - [Neon docs | PostgreSQL Joins](https://neon.tech/postgresql/postgresql-tutorial/postgresql-joins)

#### 6. [Replace Employee ID With The Unique Identifier (1378)](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/)

```sql
SELECT
    e.name,
    u.unique_id
FROM
    Employees e
LEFT JOIN
    EmployeeUNI u
        ON e.id = u.id;
```

#### 7. [Product Sales Analysis I (1068)](https://leetcode.com/problems/product-sales-analysis-i/)

```sql
SELECT
    p.product_name,
    s.year,
    s.price
FROM
    Sales s
NATURAL JOIN
    Product p
```

#### 8. [Customer Who Visited but Did Not Make Any Transactions (1581)](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions)

```sql
SELECT
    v.customer_id,
    COUNT(*)
        AS count_no_trans
FROM
    Visits v
LEFT JOIN
    Transactions t
        ON t.visit_id = v.visit_id
WHERE
    t.transaction_id
        IS NULL
GROUP BY
    v.customer_id;
```

#### 9. [Rising Temperature (197)](https://leetcode.com/problems/rising-temperature/)

```sql
SELECT
    w2.id
FROM
    Weather w1
INNER JOIN
    Weather w2
    ON w2.temperature > w1.temperature
WHERE
    w2.recordDate::date - w1.recordDate::date = 1
```

#### 10. [Average Time of Process per Machine (1661)](https://leetcode.com/problems/average-time-of-process-per-machine/)

```sql
SELECT
    a2.machine_id,
    ROUND(AVG(a2.timestamp - a1.timestamp)::numeric, 3)  -- or, decimal
        AS processing_time
FROM
    Activity a1
INNER JOIN
    Activity a2
    ON a2.machine_id = a1.machine_id
WHERE
    a2.activity_type = 'end'
    AND a1.activity_type = 'start'
GROUP BY
    a2.machine_id
```

> The `::` operator is used to cast a value to a specific data type.
>
> - [PostgreSQL numeric data types](https://www.postgresql.org/docs/current/datatype-numeric.html) docs

## Contributing

If you'd like to contribute, feel free to fork this repository and submit a pull request with your solutions or improvements. Make sure to follow the same format for consistency.
