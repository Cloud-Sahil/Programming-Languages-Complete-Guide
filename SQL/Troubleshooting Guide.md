# SQL — Troubleshooting Guide (Common Errors & Fixes)

Format: **Symptom → Likely Cause → Fix**

## 1. Syntax / Structural Errors

**Error**: `ERROR 1064: You have an error in your SQL syntax`
- Cause: typo, missing comma, reserved keyword used as column name, wrong quote type.
- Fix: check keyword usage, wrap reserved words in backticks (MySQL) or double quotes (Postgres).

**Error**: `Column count doesn't match value count`
- Cause: INSERT statement column list doesn't match VALUES count.
- Fix: explicitly list column names in INSERT.

## 2. Constraint Violations

**Error**: `Duplicate entry 'x' for key 'PRIMARY'`
- Cause: inserting a duplicate primary/unique key value.
- Fix: use `INSERT IGNORE`, `ON DUPLICATE KEY UPDATE`, or check existence first.

**Error**: `Cannot add or update a child row: a foreign key constraint fails`
- Cause: inserting a child row whose FK value doesn't exist in parent table, or deleting a parent still referenced by children.
- Fix: insert parent first, or use `ON DELETE CASCADE`/`SET NULL` appropriately; verify FK value exists.

**Error**: `Column 'x' cannot be null`
- Cause: NOT NULL column missing a value on insert.
- Fix: provide a value or set a sensible DEFAULT.

## 3. Deadlocks & Locking

**Error**: `Deadlock found when trying to get lock; try restarting transaction`
- Cause: two transactions locking rows in opposite order (Txn A locks row1 then wants row2; Txn B locks row2 then wants row1).
- Fix:
  - Always access tables/rows in a **consistent order** across the app.
  - Keep transactions short.
  - Add retry logic with exponential backoff on deadlock errors.
  - Use lower isolation level if business logic allows.

**Symptom**: Query hangs indefinitely
- Cause: waiting on a row/table lock held by another long-running transaction.
- Fix: check `SHOW ENGINE INNODB STATUS` (MySQL) or `pg_locks` (Postgres) to find blocking session; kill or optimize the blocking transaction; add lock timeout.

## 4. Performance Issues

**Symptom**: Query suddenly slow after working fine for months
- Cause: table grew large, missing/unused index, stale statistics, full table scan.
- Fix:
  - Run `EXPLAIN`/`EXPLAIN ANALYZE` to check execution plan.
  - Check if index is actually being used (look for "Using filesort", "Using temporary", "type: ALL").
  - Run `ANALYZE TABLE` to refresh optimizer statistics.
  - Add composite index matching WHERE + ORDER BY columns.

**Symptom**: Query fast in dev, slow in production
- Cause: data volume difference, different indexes, different hardware/config, no query cache.
- Fix: test with production-like data volume; verify indexes exist in prod; check connection pool and server resource limits.

**Symptom**: `SELECT *` queries are slow / high memory usage
- Cause: fetching unnecessary columns/blobs, no covering index.
- Fix: select only required columns; use covering indexes.

## 5. Connection & Resource Issues

**Error**: `Too many connections`
- Cause: connection pool exhausted, connections not closed properly, connection leak in app code.
- Fix: use connection pooling (HikariCP, PgBouncer), ensure `close()`/`try-with-resources`, increase `max_connections` cautiously, monitor idle connections.

**Error**: `Lock wait timeout exceeded`
- Cause: transaction waited too long for a lock.
- Fix: shorten transactions, add appropriate indexes to reduce lock scope, review isolation level.

**Symptom**: Database server CPU/memory maxed out
- Cause: inefficient queries running repeatedly, missing indexes, N+1 query pattern from application, no caching layer.
- Fix: identify slow queries via slow query log, add indexes/caching, batch operations, use read replicas.

## 6. Data Integrity / Logic Issues

**Symptom**: NULL comparisons returning unexpected results
- Cause: `column = NULL` never evaluates true in SQL.
- Fix: use `IS NULL` / `IS NOT NULL`; use `COALESCE()` / `IFNULL()` when needed.

**Symptom**: Duplicate rows appearing after JOIN
- Cause: one-to-many relationship causing row multiplication (fan-out).
- Fix: use `DISTINCT`, aggregate before joining, or restructure query with subqueries/CTEs.

**Symptom**: Aggregate function giving wrong totals after JOIN
- Cause: JOIN duplicating rows before aggregation (classic fan-out trap).
- Fix: aggregate in a subquery/CTE *before* joining to the other table.

## 7. Character Set / Collation Issues

**Error**: `Incorrect string value` / mojibake (garbled text)
- Cause: mismatched character sets (e.g. latin1 vs utf8mb4) between column, connection, and application.
- Fix: standardize on `utf8mb4` end-to-end (DB, table, column, connection charset).

**Error**: `Illegal mix of collations`
- Cause: comparing/joining columns with different collations.
- Fix: use `CONVERT()`/`COLLATE` to align collations, or standardize schema collation.

## 8. Migration / Schema Change Issues

**Symptom**: `ALTER TABLE` locks the table for a long time in production
- Cause: large table, blocking DDL on some engines/versions.
- Fix: use online schema change tools (`gh-ost`, `pt-online-schema-change` for MySQL), or native online DDL where supported; run during low-traffic windows.

**Symptom**: Migration script fails halfway, leaving inconsistent schema
- Cause: no transaction wrapping DDL, or DB doesn't support transactional DDL.
- Fix: use migration tools (Flyway, Liquibase) with proper rollback scripts; test migrations on staging first.

## 9. Backup / Recovery Issues

**Symptom**: Restore from backup fails or data missing
- Cause: incomplete backup, backup taken mid-transaction without consistency flag, wrong binlog position for point-in-time recovery.
- Fix: use `--single-transaction` (mysqldump) for consistent backups; verify backups regularly by test-restoring; document RPO/RTO and PITR steps.

## 10. Quick Diagnostic Checklist
1. Reproduce the issue — get the exact query and error message.
2. Run `EXPLAIN`/`EXPLAIN ANALYZE`.
3. Check indexes on filter/join/order columns.
4. Check current locks/blocking sessions.
5. Check server metrics — CPU, memory, disk I/O, connection count.
6. Check recent schema/data changes or deployments.
7. Check logs — slow query log, error log.
