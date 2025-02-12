# Top SQL 50 Study Plan (LeetCode)

This repository contains solutions to the **Top SQL 50 Study Plan** from [LeetCode](https://leetcode.com/studyplan/top-sql-50/) using **PostgreSQL**. Each problem is linked to its corresponding LeetCode page, and solutions are provided in PostgreSQL syntax.

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
> ```
>  length (text) → integer
>      Returns the number of characters in the string.
>      length('jose') → 4
>  ```


## Contributing

If you'd like to contribute, feel free to fork this repository and submit a pull request with your solutions or improvements. Make sure to follow the same format for consistency.
