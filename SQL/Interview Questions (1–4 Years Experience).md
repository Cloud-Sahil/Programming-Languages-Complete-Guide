# SQL — Interview Questions (1–4 Years Experience)

## Conceptual

**Q1. What is the difference between WHERE and HAVING?**
WHERE filters individual rows before grouping; HAVING filters groups after aggregation via GROUP BY.

**Q2. Difference between DELETE, TRUNCATE, DROP?**
- DELETE: DML, removes rows (can use WHERE), logged, can rollback, triggers fire.
- TRUNCATE: DDL, removes all rows, resets identity, faster, minimal logging, can't use WHERE.
- DROP: removes the entire table structure and data.

**Q3. Difference between PRIMARY KEY and UNIQUE KEY?**
PRIMARY KEY: only one per table, doesn't allow NULL, used to identify rows. UNIQUE KEY: multiple allowed per table, allows one NULL (in most RDBMS).

**Q4. What is a JOIN? Explain types.**
Combines rows from two or more tables based on a related column. Types: INNER, LEFT, RIGHT, FULL OUTER, SELF, CROSS. (See theory file for details.)

**Q5. What is normalization? Why is it needed?**
Process of organizing data to reduce redundancy and improve integrity, by splitting data into related tables following normal forms (1NF, 2NF, 3NF).

**Q6. What's the difference between clustered and non-clustered index?**
Clustered index determines the physical storage order of data (one per table). Non-clustered index is a separate structure with pointers to the actual rows (multiple allowed).

**Q7. What is a subquery? Types?**
A query nested inside another query. Types: scalar, correlated, non-correlated, multi-row.

**Q8. What are ACID properties?**
Atomicity, Consistency, Isolation, Durability — guarantee reliable transaction processing.

**Q9. Difference between UNION and UNION ALL?**
UNION removes duplicates (slower, does a sort/dedupe); UNION ALL keeps all rows including duplicates (faster).

**Q10. What is a stored procedure vs a function?**
Stored procedure: can perform DML, doesn't have to return a value, called with CALL/EXEC. Function: must return a value, can be used inside a SELECT statement, generally can't perform DML.

## Query-Writing

**Q11. Find the second highest salary.**
```sql
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
-- or
SELECT DISTINCT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 1;
-- or with window function
SELECT salary FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk FROM employees
) t WHERE rnk = 2;
```

**Q12. Find duplicate rows in a table.**
```sql
SELECT email, COUNT(*) 
FROM users 
GROUP BY email 
HAVING COUNT(*) > 1;
```

**Q13. Delete duplicate rows keeping only one copy.**
```sql
DELETE u1 FROM users u1
INNER JOIN users u2 
WHERE u1.id > u2.id AND u1.email = u2.email;
```

**Q14. Find employees who earn more than their manager.**
```sql
SELECT e.name FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

**Q15. Find the Nth highest salary (general).**
```sql
SELECT salary FROM (
  SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk FROM employees
) t WHERE rnk = N;
```

**Q16. Find departments with more than 5 employees.**
```sql
SELECT dept_id, COUNT(*) FROM employees GROUP BY dept_id HAVING COUNT(*) > 5;
```

**Q17. Write a query to find employees who joined in the last 30 days.**
```sql
SELECT * FROM employees WHERE join_date >= CURRENT_DATE - INTERVAL 30 DAY;
```

**Q18. How do you find the top 3 selling products per category?**
```sql
SELECT * FROM (
  SELECT product_id, category_id, sales,
  ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY sales DESC) AS rn
  FROM products
) t WHERE rn <= 3;
```

## Performance / Practical

**Q19. How would you optimize a slow SELECT query?**
Check EXPLAIN plan, ensure proper indexing, avoid SELECT *, avoid functions on indexed columns, avoid unnecessary JOINs/subqueries, consider caching, rewrite correlated subqueries as JOINs.

**Q20. What is index and when should you NOT use it?**
Avoid heavy indexing on tables with frequent INSERT/UPDATE/DELETE, on low-cardinality columns (e.g. boolean), or on very small tables.

**Q21. What is a deadlock and how do you handle it in your application code?**
Explain deadlock (see theory/troubleshooting file), and mention catching the deadlock error code and retrying the transaction.

**Q22. How do you handle NULL values in SQL?**
Use `IS NULL`/`IS NOT NULL`, `COALESCE()`, `IFNULL()`, be careful with aggregate functions (they ignore NULLs except COUNT(*)).

**Q23. Explain the difference between OLTP and OLAP.**
OLTP: transactional systems, many small read/write operations (e.g. banking app). OLAP: analytical systems, complex queries over large historical data (e.g. data warehouse/reporting).

**Q24. What's the difference between a view and a materialized view?**
View: virtual table, query re-runs every time it's accessed. Materialized view: physically stores the result, needs manual/scheduled refresh, faster to read.

**Q25. How do transactions work across microservices with separate databases?**
Discuss distributed transaction challenges, two-phase commit, and the more common pattern: Saga pattern (choreography/orchestration) with compensating transactions.
