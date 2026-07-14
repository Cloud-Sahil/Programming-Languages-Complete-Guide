# SQL — Rapid Fire Questions

Quick one-line Q&A — good for last-minute revision.

1. **Full form of SQL?** Structured Query Language.
2. **DDL full form?** Data Definition Language.
3. **Which command removes all rows but keeps structure, fastest?** TRUNCATE.
4. **Which clause filters after GROUP BY?** HAVING.
5. **Default JOIN type?** INNER JOIN.
6. **Which key allows only one NULL and multiple such columns?** UNIQUE KEY.
7. **Which normal form removes transitive dependency?** 3NF.
8. **Command to see query execution plan?** EXPLAIN.
9. **Function to replace NULL with a default value?** COALESCE() / IFNULL().
10. **Which join returns Cartesian product?** CROSS JOIN.
11. **Command to give privileges?** GRANT.
12. **Command to undo a transaction?** ROLLBACK.
13. **Which window function never repeats a rank?** ROW_NUMBER().
14. **Which window function leaves gaps after ties?** RANK().
15. **What does ACID's "I" stand for?** Isolation.
16. **Fastest way to check duplicate rows?** GROUP BY + HAVING COUNT(*) > 1.
17. **Which SQL keyword avoids duplicate rows in result?** DISTINCT.
18. **What's the default isolation level in MySQL InnoDB?** REPEATABLE READ.
19. **What's the default isolation level in PostgreSQL?** READ COMMITTED.
20. **Which index type is limited to one per table?** Clustered index.
21. **Symbol used for wildcard match in LIKE?** % (any chars), _ (single char).
22. **Which command changes table structure?** ALTER.
23. **Which SQL statement type includes SELECT?** DML.
24. **What operator checks for value in a list?** IN.
25. **What operator checks a range?** BETWEEN.
26. **What's a self join used for?** Comparing rows within the same table (e.g. employee-manager).
27. **What is a surrogate key?** An artificial key (e.g. auto-increment ID) with no business meaning.
28. **What is a natural key?** A key derived from existing business data (e.g. email, SSN).
29. **What does CASCADE do on delete?** Automatically deletes/updates dependent child rows.
30. **What's the difference between COUNT(*) and COUNT(column)?** COUNT(*) counts all rows; COUNT(column) ignores NULLs in that column.
31. **What does EXISTS check?** Whether a subquery returns any rows (true/false).
32. **Which is generally faster for large data: IN or EXISTS?** EXISTS (for correlated large subqueries).
33. **What is a composite key?** Primary key made of more than one column.
34. **What's the SQL command to rename a table?** RENAME TABLE / ALTER TABLE ... RENAME TO.
35. **What's a materialized view?** A physically stored, pre-computed query result.
36. **What does the OFFSET clause do?** Skips a specified number of rows before returning results.
37. **What's a covering index?** An index containing all the columns a query needs, avoiding a table lookup.
38. **What is denormalization used for?** Improving read performance by adding redundancy.
39. **What is a phantom read?** New rows appearing in a repeated query within the same transaction.
40. **Which TCL command sets a point to rollback to?** SAVEPOINT.
