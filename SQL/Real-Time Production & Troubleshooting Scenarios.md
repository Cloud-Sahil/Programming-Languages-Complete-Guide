# SQL — Real-Time Production & Troubleshooting Scenarios

Scenario format: **Situation → Investigation Steps → Root Cause → Fix → Prevention**

## Scenario 1: Checkout page suddenly times out during a flash sale

- **Situation**: E-commerce checkout query that inserts an order + updates inventory starts timing out under high concurrent traffic.
- **Investigation**: Checked `SHOW PROCESSLIST` / `pg_stat_activity`; found many sessions in "waiting for lock" state on the `inventory` table.
- **Root Cause**: Multiple transactions updating the same product row (`UPDATE inventory SET stock = stock - 1 WHERE product_id = X`) causing row-lock contention; some transactions also held the lock longer than needed due to an unrelated slow query inside the same transaction.
- **Fix**: Shortened the transaction to only the essential statements; added a queue/rate-limiter in front of hot-product inventory updates; considered optimistic locking with a `version` column.
- **Prevention**: Load test hot paths before sales events; monitor lock wait time as an alert metric.

## Scenario 2: Reports going stale / read replica lag

- **Situation**: Dashboard shows outdated numbers; users complain data is "wrong."
- **Investigation**: Checked replication lag (`SHOW SLAVE STATUS` → `Seconds_Behind_Master`, or Postgres `pg_stat_replication`).
- **Root Cause**: A large batch UPDATE job on the primary generated a huge amount of binlog/WAL, causing the replica to fall behind by several minutes.
- **Fix**: Ran batch job in smaller chunks with pauses between batches; moved reporting queries to read after confirming lag is near zero, or added lag-aware routing.
- **Prevention**: Break large batch jobs into chunks; monitor replication lag with alerts; consider a dedicated replica for heavy batch jobs.

## Scenario 3: Table locked, app completely down

- **Situation**: All writes to `orders` table hang; app appears down.
- **Investigation**: Ran query to find blocking sessions (`SELECT * FROM information_schema.innodb_trx` / `pg_locks` joined with `pg_stat_activity`).
- **Root Cause**: A developer ran an `ALTER TABLE orders ADD COLUMN ...` directly on production during business hours; DDL took a metadata lock that blocked all subsequent queries.
- **Fix**: Killed the migration; scheduled it for a maintenance window using an online schema change tool.
- **Prevention**: Never run raw schema changes on production directly; use CI/CD with migration tools (Flyway/Liquibase) and online DDL tools for large tables.

## Scenario 4: Query worked fine, then became slow after data growth

- **Situation**: A query that used to return in 50ms now takes 8 seconds.
- **Investigation**: Ran `EXPLAIN ANALYZE`; saw a full table scan (`type: ALL`) instead of an index seek.
- **Root Cause**: Table grew from 10K to 10M rows; existing index wasn't being used because the query filtered on a function over the column (`WHERE DATE(created_at) = '2026-07-14'`), which disables index usage.
- **Fix**: Rewrote query to use a sargable range condition (`WHERE created_at >= '2026-07-14' AND created_at < '2026-07-15'`); this let the existing index be used.
- **Prevention**: Code review checklist for sargable predicates; periodic index usage audits as data grows.

## Scenario 5: N+1 query problem killing API latency

- **Situation**: An API endpoint listing 50 orders takes 3+ seconds; DB shows 500+ small queries per request.
- **Investigation**: Enabled query logging; saw 1 query to fetch orders, then 1 query per order to fetch its items (classic ORM N+1).
- **Root Cause**: ORM lazy-loading each order's items in a loop instead of a single JOIN/batched query.
- **Fix**: Used eager loading / a single JOIN query or `WHERE order_id IN (...)` batched fetch.
- **Prevention**: Review ORM query logs in staging; add N+1 detection tooling (e.g. Bullet gem for Rails, or APM query count alerts).

## Scenario 6: Data corruption after a failed migration

- **Situation**: After a deploy, some rows have `NULL` in a column that should never be null; app throws errors.
- **Investigation**: Checked migration script and deploy logs; migration script updated rows in batches but crashed halfway through (server restart mid-migration).
- **Root Cause**: Migration wasn't idempotent/resumable and wasn't wrapped safely; partial run left inconsistent data.
- **Fix**: Wrote a backfill script to detect and fix the specific NULL rows using business logic; added the missing NOT NULL constraint after fixing.
- **Prevention**: Make migrations idempotent, resumable, and tested on a staging copy of production data before running live.

## Scenario 7: Disk full on the database server

- **Situation**: Writes suddenly start failing with I/O errors.
- **Investigation**: `df -h` on DB server shows 100% disk usage; checked binlog/WAL directory size.
- **Root Cause**: Binary logs (MySQL) / WAL segments (Postgres) not being purged because a replica was disconnected, so the primary retained logs indefinitely waiting for it to catch up.
- **Fix**: Freed space by archiving old logs; fixed/removed the disconnected replica; set `expire_logs_days` (MySQL) or appropriate WAL retention settings.
- **Prevention**: Monitor disk usage with alerts at 70/85/95%; set log retention policies; monitor replica connectivity.

## Scenario 8: Wrong query plan after a bulk data load

- **Situation**: Query that was fast becomes slow right after a nightly ETL job loads millions of new rows.
- **Investigation**: `EXPLAIN` showed the optimizer choosing a bad plan (e.g., nested loop instead of hash join).
- **Root Cause**: Table statistics were stale — optimizer didn't know the table had grown massively.
- **Fix**: Ran `ANALYZE TABLE` / `ANALYZE` (Postgres) after the ETL job to refresh statistics.
- **Prevention**: Add `ANALYZE`/statistics refresh as a standard step at the end of ETL pipelines; enable auto-analyze/auto-vacuum tuning (Postgres).

## Scenario 9: Two microservices deadlocking each other via shared DB

- **Situation**: Intermittent deadlock errors in logs, unrelated services involved.
- **Investigation**: Correlated deadlock timestamps with both services' transaction logs; found both updated `users` and `wallets` tables but in opposite order.
- **Root Cause**: Service A did `UPDATE users` then `UPDATE wallets`; Service B did `UPDATE wallets` then `UPDATE users` — classic deadlock cycle.
- **Fix**: Standardized lock acquisition order across all services (always users → wallets); added retry-with-backoff on deadlock error codes.
- **Prevention**: Document and enforce a canonical lock ordering convention for multi-table transactions across teams.

## Scenario 10: Production incident — accidental DELETE without WHERE

- **Situation**: Engineer runs `DELETE FROM orders;` intending to delete a subset but forgets WHERE clause.
- **Investigation**: Immediate widespread data loss reported.
- **Root Cause**: No safeguard against unfiltered destructive queries in production; direct DB access allowed.
- **Fix**: Restored from latest backup + replayed binlogs/WAL up to just before the incident (point-in-time recovery).
- **Prevention**: Disable direct production DB write access for engineers; require destructive queries to go through reviewed migration scripts; enable `sql_safe_updates`/similar guardrails; take frequent backups with tested PITR.
