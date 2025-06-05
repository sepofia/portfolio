1. [Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/description/)

```sql
SELECT customer_id
FROM customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
    SELECT COUNT(*)
    FROM product
)
```

2. [Second Highest Salary](https://leetcode.com/problems/second-highest-salary/description/)

```sql
WITH order_salary AS (
    SELECT salary
         , ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
    FROM (
        SELECT DISTINCT salary
        FROM employee
    )
)
SELECT salary AS secondHighestSalary
FROM order_salary
WHERE row_num = 2
UNION
SELECT NULL
LIMIT 1
```

3. [Investments in 2016](https://leetcode.com/problems/investments-in-2016/description/)

```sql
WITH correct_tiv AS (
    SELECT tiv_2015
    FROM insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) != 1
),
correct_lat_lon AS (
    SELECT lat
         , lon
    FROM insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
)
SELECT SUM(tiv_2016) :: decimal(16, 2) AS tiv_2016
FROM insurance
WHERE tiv_2015 IN (SELECT * FROM correct_tiv)
  AND (lat, lon) IN (SELECT * FROM correct_lat_lon)
```

4. [Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/description/)

```sql
WITH users AS (
    SELECT requester_id AS user_id
    FROM requestAccepted
    UNION ALL (SELECT accepter_id AS user_id FROM requestAccepted)
)
SELECT user_id AS id
     , COUNT(*) AS num
FROM users
GROUP BY id
ORDER BY num DESC
LIMIT 1
```

5. [Restaurant Growth](https://leetcode.com/problems/restaurant-growth/description/)

```sql
WITH count_sum AS (
    SELECT visited_on
         , SUM(amount) OVER (
            ORDER BY visited_on
            RANGE BETWEEN INTERVAL '6 DAYS' PRECEDING AND CURRENT ROW
        ) AS average_amount
    FROM customer
),
start_date AS (
    SELECT MIN(visited_on) + 6
    FROM customer
)

SELECT visited_on
     , AVG(average_amount) AS amount
     , ROUND(AVG(average_amount) / 7, 2) AS average_amount
FROM count_sum
GROUP BY visited_on
HAVING visited_on >= (SELECT * FROM start_date)
```
