# SQL — Zero to Hero: Complete Study Guide

## Table of Contents
1. Introduction & Theory
2. Basics (Beginner)
3. Intermediate SQL
4. Advanced SQL
5. Database Design & Normalization
6. Indexing & Performance Tuning
7. Transactions & Concurrency (Theory Deep-Dive)
8. Window Functions
9. Stored Procedures, Functions, Triggers
10. SQL in Real Projects (User-Side / Application Integration)

---

## 1. Introduction & Theory

### What is SQL?
SQL (Structured Query Language) is a domain-specific language used to communicate with **Relational Database Management Systems (RDBMS)** like MySQL, PostgreSQL, Oracle, SQL Server, and SQLite.

### RDBMS Core Theory
- **Table** — a collection of rows (records) and columns (fields/attributes).
- **Schema** — the logical structure describing tables, columns, data types, and relationships.
- **Primary Key (PK)** — uniquely identifies each row; cannot be NULL; only one per table (can be composite).
- **Foreign Key (FK)** — a column (or set) that references a PK in another table, enforcing referential integrity.
- **Candidate Key** — any column(s) that could qualify as PK.
- **Composite Key** — a key made of two or more columns.
- **Surrogate Key** — an artificial key (e.g., auto-increment ID) with no business meaning.

### Types of SQL Commands (Theory)
| Category | Full Form | Commands | Purpose |
|---|---|---|---|
| DDL | Data Definition Language | CREATE, ALTER, DROP, TRUNCATE | Defines/modifies structure |
| DML | Data Manipulation Language | INSERT, UPDATE, DELETE | Modifies data |
| DQL | Data Query Language | SELECT | Retrieves data |
| DCL | Data Control Language | GRANT, REVOKE | Controls access/permissions |
| TCL | Transaction Control Language | COMMIT, ROLLBACK, SAVEPOINT | Manages transactions |

### ACID Properties (Very Important Theory — Frequently Asked)
- **Atomicity** — a transaction is all-or-nothing.
- **Consistency** — database moves from one valid state to another valid state.
- **Isolation** — concurrent transactions don't interfere with each other.
- **Durability** — once committed, changes persist even after a crash.

### OLTP vs OLAP
- **OLTP** (Online Transaction Processing): Many short read/write transactions — e.g., banking apps.
- **OLAP** (Online Analytical Processing): Complex read-heavy queries for analytics/reporting — e.g., data warehouses.

---

## 2. Basics (Beginner)

### Data Types
```sql
INT, BIGINT, DECIMAL(p,s), FLOAT, VARCHAR(n), CHAR(n), TEXT,
DATE, DATETIME, TIMESTAMP, BOOLEAN, JSON (MySQL/Postgres)
```

### Creating a Database & Table
```sql
CREATE DATABASE company_db;
USE company_db;

CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10,2),
    dept_id INT,
    hire_date DATE DEFAULT CURRENT_DATE,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

### Basic CRUD
```sql
-- INSERT
INSERT INTO employees (first_name, last_name, email, salary, dept_id)
VALUES ('Rahul', 'Sharma', 'rahul@company.com', 55000, 2);

-- SELECT
SELECT first_name, salary FROM employees WHERE salary > 50000;

-- UPDATE
UPDATE employees SET salary = salary * 1.10 WHERE dept_id = 2;

-- DELETE
DELETE FROM employees WHERE emp_id = 10;
```

### Filtering & Sorting
```sql
SELECT * FROM employees
WHERE dept_id = 2 AND salary BETWEEN 40000 AND 80000
ORDER BY salary DESC
LIMIT 10;

SELECT * FROM employees WHERE first_name LIKE 'R%';
SELECT * FROM employees WHERE dept_id IN (1,2,3);
SELECT * FROM employees WHERE email IS NOT NULL;
```

### Aggregate Functions
```sql
SELECT COUNT(*), AVG(salary), MAX(salary), MIN(salary), SUM(salary)
FROM employees;

SELECT dept_id, COUNT(*) AS emp_count
FROM employees
GROUP BY dept_id
HAVING COUNT(*) > 5;
```
**Theory:** `WHERE` filters rows *before* grouping; `HAVING` filters *after* aggregation — this distinction is a classic interview question.

---

## 3. Intermediate SQL

### Joins (Theory + Practice)
- **INNER JOIN** — returns matching rows in both tables.
- **LEFT JOIN** — all rows from left + matched rows from right (NULL if no match).
- **RIGHT JOIN** — all rows from right + matched rows from left.
- **FULL OUTER JOIN** — all rows from both, matched where possible.
- **SELF JOIN** — a table joined with itself.
- **CROSS JOIN** — Cartesian product of two tables.

```sql
SELECT e.first_name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

SELECT e.first_name, m.first_name AS manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;
```

### Subqueries
```sql
-- Scalar subquery
SELECT first_name, salary FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Correlated subquery
SELECT e.first_name FROM employees e
WHERE e.salary > (
    SELECT AVG(salary) FROM employees e2 WHERE e2.dept_id = e.dept_id
);

-- EXISTS
SELECT d.dept_name FROM departments d
WHERE EXISTS (SELECT 1 FROM employees e WHERE e.dept_id = d.dept_id);
```

### Set Operations
```sql
SELECT emp_id FROM employees_2024
UNION
SELECT emp_id FROM employees_2025;   -- removes duplicates

SELECT emp_id FROM employees_2024
UNION ALL
SELECT emp_id FROM employees_2025;   -- keeps duplicates

SELECT emp_id FROM employees INTERSECT SELECT emp_id FROM managers;
SELECT emp_id FROM employees EXCEPT SELECT emp_id FROM managers;
```

### Views
```sql
CREATE VIEW high_earners AS
SELECT first_name, salary FROM employees WHERE salary > 100000;

SELECT * FROM high_earners;
```
**Theory:** A view is a virtual table backed by a query — doesn't store data itself (unless materialized). Used for security/abstraction and reusability.

### Common String/Date Functions
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
SELECT UPPER(first_name), LOWER(last_name) FROM employees;
SELECT LENGTH(email) FROM employees;
SELECT DATEDIFF(NOW(), hire_date) AS days_employed FROM employees;
SELECT DATE_FORMAT(hire_date, '%Y-%m') AS hire_month FROM employees;
```

---

## 4. Advanced SQL

### Window Functions (High-Frequency Interview Topic)
Window functions perform calculations across a set of rows *related* to the current row, without collapsing rows like GROUP BY does.

```sql
-- Ranking
SELECT emp_id, dept_id, salary,
       RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk,
       DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dense_rnk,
       ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS row_num
FROM employees;

-- Running total
SELECT emp_id, salary,
       SUM(salary) OVER (ORDER BY emp_id) AS running_total
FROM employees;

-- LAG / LEAD
SELECT emp_id, salary,
       LAG(salary, 1) OVER (ORDER BY emp_id) AS prev_salary,
       LEAD(salary, 1) OVER (ORDER BY emp_id) AS next_salary
FROM employees;

-- NTILE (buckets)
SELECT emp_id, salary, NTILE(4) OVER (ORDER BY salary) AS quartile
FROM employees;
```

**RANK vs DENSE_RANK vs ROW_NUMBER (classic interview Q):**
- `ROW_NUMBER()` — unique sequential number, no ties.
- `RANK()` — same rank for ties, but skips subsequent ranks (1,1,3).
- `DENSE_RANK()` — same rank for ties, no gaps (1,1,2).

### Common Table Expressions (CTEs)
```sql
WITH dept_avg AS (
    SELECT dept_id, AVG(salary) AS avg_sal
    FROM employees
    GROUP BY dept_id
)
SELECT e.first_name, e.salary, d.avg_sal
FROM employees e
JOIN dept_avg d ON e.dept_id = d.dept_id
WHERE e.salary > d.avg_sal;

-- Recursive CTE (org hierarchy)
WITH RECURSIVE org_chart AS (
    SELECT emp_id, first_name, manager_id, 1 AS level
    FROM employees WHERE manager_id IS NULL
    UNION ALL
    SELECT e.emp_id, e.first_name, e.manager_id, oc.level + 1
    FROM employees e
    JOIN org_chart oc ON e.manager_id = oc.emp_id
)
SELECT * FROM org_chart;
```

### Pivoting Data
```sql
SELECT dept_id,
    SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS male_count,
    SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS female_count
FROM employees
GROUP BY dept_id;
```

### Stored Procedures, Functions & Triggers
```sql
DELIMITER //
CREATE PROCEDURE GiveRaise(IN emp_id_in INT, IN pct DECIMAL(5,2))
BEGIN
    UPDATE employees SET salary = salary * (1 + pct/100) WHERE emp_id = emp_id_in;
END //
DELIMITER ;

CALL GiveRaise(101, 10);

-- Trigger
CREATE TRIGGER before_employee_delete
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_audit(emp_id, action, action_date)
    VALUES (OLD.emp_id, 'DELETE', NOW());
END;
```

### Transactions (Theory + Code)
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 500 WHERE acc_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE acc_id = 2;
COMMIT;
-- If anything fails: ROLLBACK;
```

**Isolation Levels (theory, frequently asked):**
1. **Read Uncommitted** — dirty reads possible.
2. **Read Committed** — no dirty reads (default in PostgreSQL, Oracle).
3. **Repeatable Read** — no dirty/non-repeatable reads (default in MySQL InnoDB).
4. **Serializable** — strictest, fully isolated, can cause more locking/deadlocks.

**Concurrency problems:**
- **Dirty Read** — reading uncommitted data from another transaction.
- **Non-Repeatable Read** — same query returns different results within a transaction.
- **Phantom Read** — new rows appear in a repeated query due to another transaction's insert.

---

## 5. Database Design & Normalization

### Normalization (Very Common Interview Theory)
- **1NF** — atomic values, no repeating groups.
- **2NF** — 1NF + no partial dependency on part of a composite key.
- **3NF** — 2NF + no transitive dependency (non-key attribute depends on another non-key attribute).
- **BCNF** — stricter version of 3NF; every determinant is a candidate key.

### Denormalization
Intentionally introducing redundancy to improve read performance in analytics/reporting systems — trade-off between write complexity and read speed.

### ER Modeling
- Entities → tables; Attributes → columns; Relationships → foreign keys.
- Relationship types: One-to-One, One-to-Many, Many-to-Many (needs a junction/bridge table).

---

## 6. Indexing & Performance Tuning

### Index Theory
An index is a data structure (commonly a **B-Tree** or **B+ Tree**, sometimes a **Hash Index**) that speeds up lookups at the cost of extra storage and slower writes (INSERT/UPDATE/DELETE must also update the index).

```sql
CREATE INDEX idx_emp_salary ON employees(salary);
CREATE UNIQUE INDEX idx_emp_email ON employees(email);
CREATE INDEX idx_emp_dept_salary ON employees(dept_id, salary);  -- composite
DROP INDEX idx_emp_salary ON employees;
```

**Types of Indexes:**
- **Clustered Index** — determines physical row order; only one per table (usually the PK).
- **Non-Clustered Index** — separate structure pointing to actual row location; multiple allowed.
- **Composite Index** — index on multiple columns; column order matters (leftmost prefix rule).
- **Covering Index** — an index that contains all columns needed by a query, avoiding a table lookup.

### Query Optimization
```sql
EXPLAIN SELECT * FROM employees WHERE dept_id = 2;
EXPLAIN ANALYZE SELECT * FROM employees WHERE dept_id = 2;
```
**Key optimization practices:**
- Avoid `SELECT *`; select only needed columns.
- Avoid functions on indexed columns in WHERE (`WHERE YEAR(hire_date)=2024` breaks index use).
- Use proper indexes on JOIN and WHERE columns.
- Avoid N+1 query problems in application code.
- Use `LIMIT`/pagination for large result sets.
- Use `EXPLAIN` to check if a query does a full table scan vs index seek.

---

## 7. SQL for Application/User-Side Integration

### Connecting from Application Code (Python example)
```python
import mysql.connector

conn = mysql.connector.connect(
    host="localhost", user="app_user", password="secret", database="company_db"
)
cursor = conn.cursor()
cursor.execute("SELECT * FROM employees WHERE dept_id = %s", (2,))
rows = cursor.fetchall()
conn.close()
```

### Preventing SQL Injection (Critical Real-World Practice)
```python
# NEVER do this (vulnerable to SQL Injection):
query = f"SELECT * FROM users WHERE username = '{username}'"

# ALWAYS use parameterized queries:
cursor.execute("SELECT * FROM users WHERE username = %s", (username,))
```

### Connection Pooling
Real production systems use connection pools (e.g., HikariCP in Java, SQLAlchemy pool in Python) instead of opening a new DB connection per request — because connections are expensive to create.

### Read Replicas & Sharding (System Design Adjacent — often asked at 2-4 YOE level)
- **Read Replica** — copy of the DB used for read queries to offload the primary/write DB.
- **Sharding** — splitting a large table across multiple databases/servers by a key (e.g., user_id) to scale horizontally.
- **Master-Slave Replication** — writes go to master, replicated (async/sync) to slaves for reads.

---

## Quick Revision Cheat-Sheet
| Concept | One-Line Recall |
|---|---|
| PK vs FK | PK uniquely identifies; FK references another table's PK |
| WHERE vs HAVING | WHERE before grouping, HAVING after grouping |
| DELETE vs TRUNCATE vs DROP | DELETE=rows(logged,rollback-able), TRUNCATE=all rows(fast,no rollback in most DBs), DROP=whole table structure |
| Clustered vs Non-Clustered | Clustered = physical order (1/table), Non-clustered = pointer structure (many/table) |
| UNION vs UNION ALL | UNION removes duplicates, UNION ALL keeps them |
| Normalization | Reduce redundancy; Denormalization = improve read speed |
