# SQL – LeetCode Practice

## Problems Solved
- Movie Rating
- Department Top Three Salaries

## Key Concepts Used
- GROUP BY with aggregate functions
- ORDER BY with LIMIT
- Window functions (DENSE_RANK)
- PARTITION BY
- JOINs between multiple tables

## What I Learned
- Aggregate functions like `COUNT` and `AVG` are often combined with GROUP BY to rank entities
- `DENSE_RANK()` with `PARTITION BY` is the cleanest way to get top N values per group
- Window functions avoid complex subqueries and improve readability

## Mistake I Made
For Department Top Three Salaries, I initially tried using `LIMIT 3`, which failed because it returns top rows globally.
Using `DENSE_RANK()` partitioned by department correctly returns top salaries **per department**.

## Queries to Remember

### Department Top Three Salaries
```sql
SELECT d.name AS Department,
       e.name AS Employee,
       e.salary AS Salary
FROM (
    SELECT *,
           DENSE_RANK() OVER (
               PARTITION BY departmentId
               ORDER BY salary DESC
           ) AS rnk
    FROM Employee
) e
JOIN Department d
ON e.departmentId = d.id
WHERE e.rnk <= 3;
