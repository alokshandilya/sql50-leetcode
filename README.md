# Top SQL 50 Study Plan (LeetCode)

This repository contains solutions to the **Top SQL 50 Study Plan** from [LeetCode](https://leetcode.com/studyplan/top-sql-50/) using **PostgreSQL**. Each problem is linked to its corresponding LeetCode page, and solutions are provided in PostgreSQL syntax.

## Order of Execution

<p align="center">
    <img height="300px" src="https://github.com/user-attachments/assets/d87d4980-2c19-492e-85b1-56ed449ea3d4">
</p>

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
>
> ```sql
> SELECT
>     NULL <> 2
>         AS result;
>
> -- output: NULL
> ```

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

#### 11. [Employee Bonus (577)](https://leetcode.com/problems/employee-bonus/)

```sql
SELECT
    e.name,
    b.bonus
FROM
    Employee e
LEFT JOIN
    Bonus b
        ON b.empId = e.empId
WHERE
    b.bonus < 1000
    OR
    b.bonus IS NULL;
```

#### 12. [Students and Examinations (1280)](https://leetcode.com/problems/students-and-examinations/)

```sql
SELECT
    st.student_id,
    st.student_name,
    su.subject_name,
    COUNT(e.subject_name)
        AS attended_exams
FROM
    Students st
CROSS JOIN
    Subjects su
LEFT JOIN
    Examinations e
    ON
        e.student_id = st.student_id
        AND
        e.subject_name = su.subject_name
GROUP BY
    st.student_id,
    st.student_name,
    su.subject_name
ORDER BY
    st.student_id,
    su.subject_name;
```

#### 13. [Managers with at Least 5 Direct Reports (570)](https://leetcode.com/problems/managers-with-at-least-5-direct-reports)

```sql
-- using subquery
SELECT
    name
FROM
    Employee
WHERE
    id IN (
        SELECT
            managerId
        FROM
            Employee
        WHERE
            managerId
                IS NOT NULL
        GROUP BY
            managerId
        HAVING
            COUNT(*) >= 5
    )
```

```sql
-- using join
SELECT
    e1.name
FROM
    Employee e1
JOIN
    Employee e2
    ON
        e2.managerId = e1.id
WHERE
    e2.managerId
        IS NOT NULL
GROUP BY
    e1.name, e1.id  -- allow multiple name
HAVING
    COUNT(*) >= 5;
```

#### 14. [Confirmation Rate (1934)](https://leetcode.com/problems/confirmation-rate/)

```sql
SELECT
    s.user_id,
    ROUND(
        COUNT(*) FILTER (WHERE c.action='confirmed')::decimal / COUNT(*),
        2
    ) AS confirmation_rate
FROM
    Signups s
LEFT JOIN
    Confirmations c
    ON
        c.user_id = s.user_id
GROUP BY
    s.user_id
```

## Basic Aggregate Functions

#### 15. [Not Boring Movies (620)](https://leetcode.com/problems/not-boring-movies)

```sql
SELECT
    *
FROM
    Cinema
WHERE
    id % 2 = 1
    AND
    description <> 'boring'
ORDER BY
    rating DESC;
```

#### 16. [Average Selling Price (1251)](https://leetcode.com/problems/average-selling-price)

```sql
SELECT
    p.product_id,
    COALESCE(
        ROUND(
            SUM(u.units * p.price)::decimal / SUM(u.units), 2
        ), 0
    ) AS average_price
FROM
    Prices p
LEFT JOIN
    UnitsSold u
    ON u.product_id = p.product_id
    AND
    u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY
    p.product_id
```

#### 17. [Project Employees I (1075)](https://leetcode.com/problems/project-employees-i)

```sql
SELECT
    p.project_id,
    ROUND(
        AVG(e.experience_years), 2
    ) AS average_years
FROM
    Project p
NATURAL JOIN
    Employee e
GROUP BY
    p.project_id
```

#### 18. [Percentage of Users Attended a Contest (1633)](https://leetcode.com/problems/percentage-of-users-attended-a-contest)

```sql
SELECT
    r.contest_id,
    ROUND(
        COUNT(r.user_id)::decimal / (SELECT COUNT(*) FROM Users) * 100.0,
        2
    ) AS percentage
FROM
    Users u
NATURAL JOIN
    Register r
GROUP BY
    r.contest_id
ORDER BY
    percentage DESC,
    contest_id
```

#### 19. [Queries Quality and Percentage (1211)](https://leetcode.com/problems/queries-quality-and-percentage/description)

```sql
SELECT
    query_name,
    ROUND(
        AVG(rating::decimal / position), 2
    ) AS quality,
    ROUND(
        (COUNT(*) FILTER(WHERE rating < 3))::decimal * 100 / COUNT(*), 2
    ) AS poor_query_percentage
FROM
    Queries
GROUP BY
    query_name
```

#### 20. [Monthly Transactions I (1193)](https://leetcode.com/problems/monthly-transactions-i/description) :star2:

```sql
SELECT
    to_char(trans_date, 'YYYY-MM') AS month,
    country,
    COUNT(*) AS trans_count,
    COUNT(*) FILTER(WHERE state='approved') AS approved_count,
    COALESCE(
        SUM(amount), 0
    ) AS trans_total_amount,
    COALESCE(
        SUM(amount) FILTER(WHERE state='approved'), 0
    ) AS approved_total_amount
FROM
    Transactions
GROUP BY
    month,
    country
```

> #### 1. `to_char` Function
>
> - to format dates, timestamps, and intervals into human-readable strings. It's incredibly versatile, allowing you to customize the output precisely.
>
> ```sql
> to_char(expression, format_string)
> ```
>
> - `expression`: The date, timestamp, or interval value you want to format.
> - `format_string`: A template string specifying how the output should look. This is where you control the format.
>
> **Format String Elements (Common Examples):**
>
> - `YYYY`: Four-digit year (e.g., 2023)
> - `YY`: Two-digit year (e.g., 23)
> - `MM`: Two-digit month (e.g., 01 for January, 12 for December)
> - `Month`: Full month name (e.g., January)
> - `Mon`: Abbreviated month name (e.g., Jan)
> - `DD`: Two-digit day of the month (e.g., 01, 31)
> - `Day`: Full day of the week (e.g., Monday)
> - `Dy`: Abbreviated day of the week (e.g., Mon)
> - `HH`: Hour (24-hour clock)
> - `HH12`: Hour (12-hour clock)
> - `MI`: Minutes
> - `SS`: Seconds
> - `MS`: Milliseconds
> - `US`: Microseconds
> - `TZ`: Time zone abbreviation (e.g., IST, PST)
> - `CC`: Century
>
> **Examples of `to_char`:**
>
> ```sql
> SELECT to_char(now(), 'YYYY-MM-DD HH24:MI:SS');  -- Current date and time
> SELECT to_char(date '2024-03-15', 'Month DD, YYYY'); -- March 15, 2024
> SELECT to_char(timestamp '2023-10-27 10:30:00', 'Day, Mon DD YYYY HH12:MI AM'); -- Friday, Oct 27 2023 10:30 AM
> SELECT to_char(interval '2 years 3 months', 'YY years MM months'); -- 02 years 03 months
> ```
>
> #### 2. `date_part` Function
>
> The `date_part` function extracts a specific component (like year, month, day, hour, etc.) from a date, timestamp, or interval.
>
> ```sql
> date_part('field', source)
> ```
>
> - `field`: The part of the date/time you want to extract (e.g., 'year', 'month', 'day', 'hour', 'minute', 'second', 'dow' (day of week), 'doy' (day of year), 'week', 'quarter').
> - `source`: The date, timestamp, or interval value.
>
> **Examples of `date_part`:**
>
> ```sql
> SELECT date_part('year', date '2024-03-15');  -- 2024
> SELECT date_part('month', timestamp '2023-10-27 10:30:00'); -- 10
> SELECT date_part('day', date '2024-03-15');   -- 15
> SELECT date_part('dow', date '2024-03-15');  -- 5 (Friday, where Sunday = 0)
> SELECT date_part('hour', timestamp '2023-10-27 10:30:00'); -- 10
> SELECT date_part('week', date '2024-03-15'); -- 11 (week number of the year)
> SELECT date_part('quarter', date '2024-05-15'); -- 2
> ```
>
> **Key Differences and When to Use Which:**
>
> - **`to_char`:** Use this when you need to _format_ a date/time/interval into a specific string representation. You have fine-grained control over the output format. Think of it as converting a date/time into a _textual representation_.
> - **`date_part`:** Use this when you need to _extract_ a specific component (a number) from a date/time/interval. It gives you a numeric value representing that part. Think of it as getting a _numerical value_ representing a date/time part.

#### 21. [Immediate Food Delivery II (1174)](https://leetcode.com/problems/immediate-food-delivery-ii/description) :star2:

```sql
WITH first_order_cte AS (
    SELECT
        customer_id,
        MIN(order_date) AS first_order
    FROM
        Delivery
    GROUP BY
        customer_id
),

immediate_order_cte AS (
    SELECT
        d.customer_id,
        MIN(d.customer_pref_delivery_date) AS immediate_delivery
    FROM
        Delivery d
    JOIN
        first_order_cte fo
        ON fo.customer_id = d.customer_id
    GROUP BY
        d.customer_id
)

SELECT
    ROUND(
        COUNT(*) FILTER(WHERE f.first_order = i.immediate_delivery)::decimal * 100
        /
        COUNT(f.customer_id), 2
    ) AS immediate_percentage
FROM
    first_order_cte f
JOIN
    immediate_order_cte i
    ON
        i.customer_id = f.customer_id
```

> **Common Table Expressions (CTEs):**
>
> - CTEs are temporary result sets that you can reference within a SELECT, INSERT, UPDATE, or DELETE statement.
> - They help break down complex queries into smaller, more manageable parts.
>
> **Syntax:**
>
> ```sql
> WITH cte_name (column1, column2, ...) AS (
>     subquery
> )
> SELECT columns
> FROM cte_name
> WHERE condition;
> ```
>
> - `cte_name`: The name of the Common Table Expression.
> - `(column1, column2, ...)`: An optional list of column names for the CTE.
> - `subquery`: The subquery that defines the CTE.
> - `SELECT columns`: The main query that references the CTE.
> - `WHERE condition`: An optional condition to filter the results.

## Contributing

If you'd like to contribute, feel free to fork this repository and submit a pull request with your solutions or improvements. Make sure to follow the same format for consistency.
