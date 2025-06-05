1. [Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/description/)

```sql
SELECT teacher_id
     , COUNT(DISTINCT subject_id) AS cnt
FROM teacher
GROUP BY teacher_id
```

2. [Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails/description/)

```sql
SELECT *
FROM users
WHERE REGEXP_LIKE(mail, '^[A-Za-z][A-Za-z0-9._-]*@leetcode\.com
```)
```

3. [List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/description/)

```sql
SELECT t1.product_name
     , SUM(t2.unit) AS unit
FROM products AS t1
JOIN orders AS t2
ON t1.product_id = t2.product_id

WHERE DATE_TRUNC('month', t2.order_date) = '2020-02-01'
GROUP BY t2.product_id, t1.product_name
HAVING SUM(t2.unit) >= 100
```

4. [Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/description/)

```sql
SELECT sell_date
     , COUNT(DISTINCT product) AS num_sold
     , STRING_AGG(DISTINCT product, ',') AS products
FROM activities
GROUP BY sell_date
ORDER BY sell_date
```

5. [Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/description/)

```sql
WITH correct_id AS (
    SELECT MIN(id)
    FROM person
    GROUP BY email
)
DELETE FROM person
WHERE id NOT IN (SELECT * FROM correct_id)
```
