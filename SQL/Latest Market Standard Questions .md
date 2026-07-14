# SQL — Latest Market Standard Questions 

These reflect what's currently being asked as SQL usage shifts toward cloud-native, AI-integrated, and high-scale systems.

## Modern SQL Features

**Q1. How do you query JSON data in SQL?**
```sql
-- Postgres
SELECT data->>'name' AS name FROM users WHERE data->>'city' = 'Pune';
-- MySQL
SELECT JSON_EXTRACT(data, '$.name') FROM users;
```
Discuss JSON columns, indexing JSON paths (Postgres GIN index / MySQL generated columns + index).

**Q2. What are vector/embedding columns in modern databases (pgvector, etc.) and when would you use SQL alongside a vector search?**
Modern Postgres (`pgvector` extension) and cloud DBs support storing embeddings and doing similarity search (`<->` operator) — used in RAG/AI applications combined with traditional relational filters.

**Q3. What is a "Lakehouse" and how does it relate to SQL?**
A Lakehouse (e.g. Databricks, Snowflake) combines data lake flexibility with data warehouse SQL querying capability over large-scale, often semi-structured data.

**Q4. Explain generated/computed columns.**
Columns whose value is automatically computed from other columns (`GENERATED ALWAYS AS (...) STORED`), useful for indexing derived JSON fields or computed business logic.

**Q5. What's the difference between a traditional data warehouse and cloud-native warehouses like Snowflake/BigQuery/Redshift?**
Separation of storage and compute, auto-scaling, pay-per-query models, columnar storage optimized for analytics, seamless handling of semi-structured data.

## Cloud & Scale

**Q6. How do you design a database schema for horizontal scaling (sharding)?**
Discuss choosing a shard key with even distribution, avoiding cross-shard joins/transactions, and consistent hashing.

**Q7. What is connection pooling and why is it critical in serverless/cloud environments?**
In serverless (Lambda etc.), each invocation can spawn a new connection; without pooling (e.g. PgBouncer, RDS Proxy) the DB can hit connection limits quickly.

**Q8. What is a read replica and how do you handle read-after-write consistency issues with it?**
Explain replication lag and strategies: read from primary for critical reads right after write, or use "read-your-writes" session stickiness.

**Q9. What's the CAP theorem and how does it relate to choosing a database?**
Consistency, Availability, Partition Tolerance — you can only guarantee two of three during a network partition. Relevant when choosing between SQL (often CP-leaning) and distributed NoSQL (often AP-leaning).

**Q10. How would you design an idempotent API that writes to SQL to avoid duplicate processing (e.g. payment retries)?**
Use a unique idempotency key column with a UNIQUE constraint + `INSERT ... ON CONFLICT DO NOTHING`/`ON DUPLICATE KEY`.

## AI / Data Engineering Adjacent

**Q11. How is SQL used in modern data pipelines (dbt, Airflow)?**
dbt: SQL-based transformation layer (T in ELT), version-controlled models, tests, documentation. Airflow: orchestrates when SQL transformation jobs run.

**Q12. What is Change Data Capture (CDC) and how does it relate to SQL databases?**
Capturing row-level changes (insert/update/delete) from the database transaction log (binlog/WAL) in real time — used for streaming data to other systems (e.g. via Debezium + Kafka) without polling.

**Q13. How would you use SQL to prep data for an LLM/RAG pipeline?**
Discuss chunking source text, storing chunks + embeddings in a table (possibly with a vector column), filtering with metadata columns before/after similarity search.

## Best Practices / Modern Engineering

**Q14. What's your approach to schema migrations in a CI/CD pipeline?**
Version-controlled migration files (Flyway/Liquibase/Alembic/Prisma Migrate), backward-compatible migrations (expand-contract pattern), automated testing on staging before prod.

**Q15. How do you handle multi-tenant SQL database design?**
Options: separate database per tenant, separate schema per tenant, shared schema with `tenant_id` column + row-level security. Discuss trade-offs (isolation vs operational overhead vs cost).

**Q16. What is row-level security (RLS) and when would you use it?**
Database-enforced policy restricting which rows a user/role can see or modify — common in multi-tenant SaaS and Postgres-based systems, reduces reliance on application-layer filtering.

**Q17. How do you monitor and observe SQL database health in production today?**
Mention tools: pg_stat_statements, slow query logs, APM tools (Datadog, New Relic), query performance insights (RDS Performance Insights), automated alerting on lock waits/replication lag/connection saturation.
