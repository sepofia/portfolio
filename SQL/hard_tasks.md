[Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/description/)

```sql
WITH order_table AS (
    SELECT salary
         , departmentId
         , ROW_NUMBER() OVER (PARTITION BY departmentId ORDER BY salary DESC) AS row_num
    FROM employee
    GROUP BY departmentId, salary
    ORDER BY departmentId, salary DESC
)
SELECT t2.name AS department
     , t1.name AS employee
     , t1.salary
FROM employee AS t1
JOIN department AS t2
ON t1.departmentId = t2.id
WHERE (t1.departmentId, t1.salary) IN (
    SELECT departmentId
         , salary
    FROM order_table
    WHERE row_num <= 3
)
```