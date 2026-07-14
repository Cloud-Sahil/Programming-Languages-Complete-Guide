# Python — Real-Time Production & Troubleshooting Scenarios

## Scenario 1: API server suddenly runs out of memory in production

- **Situation**: A FastAPI/Django service's memory usage climbs steadily and eventually gets OOM-killed by the container orchestrator.
- **Investigation**: Used `tracemalloc`/memory profiler; found large in-memory lists growing unboundedly.
- **Root Cause**: A module-level cache dictionary was used to store request-derived data with no eviction policy or TTL, growing indefinitely as traffic increased.
- **Fix**: Replaced the unbounded dict with an LRU cache (`functools.lru_cache` with `maxsize`, or Redis for a distributed cache with TTL).
- **Prevention**: Code review checklist item: any in-process cache must have a bounded size/eviction policy; add memory usage alerts.

## Scenario 2: Background job processing slows to a crawl under load

- **Situation**: A Celery/RQ worker queue that processes image uploads starts backing up badly during peak hours.
- **Investigation**: Checked worker CPU usage — pegged at 100% on a single core per worker despite having multiple threads configured.
- **Root Cause**: CPU-bound image processing task was run via `threading`, which doesn't achieve parallelism for CPU-bound work due to the GIL.
- **Fix**: Switched to `multiprocessing`-based worker pool (or scaled out more worker processes) to achieve real parallel CPU utilization.
- **Prevention**: Document guidance: use threads for I/O-bound tasks, processes for CPU-bound tasks; load-test background job throughput before launch.

## Scenario 3: Intermittent database connection errors under load

- **Situation**: Under moderate traffic, the app throws `TooManyConnectionsError` from the database.
- **Investigation**: Checked ORM/connection pool configuration.
- **Root Cause**: Each request was opening a new DB connection without returning it to a pool (connection leak), combined with a pool size not tuned for the number of app workers/processes.
- **Fix**: Configured proper connection pooling (SQLAlchemy pool settings, Django `CONN_MAX_AGE`), ensured connections were always released (`with` context managers), and right-sized the pool relative to worker count.
- **Prevention**: Load test with realistic concurrency before launch; monitor active DB connections as a standard metric.

## Scenario 4: Silent data corruption from a race condition

- **Situation**: An inventory count occasionally goes negative, which should be impossible.
- **Investigation**: Reviewed the update logic — a read-then-write pattern (`read stock, subtract, write stock`) without locking.
- **Root Cause**: Two concurrent requests both read the same stock value before either wrote back the decremented value, causing a lost update (classic race condition).
- **Fix**: Used an atomic database-level update (`UPDATE inventory SET stock = stock - 1 WHERE id = X AND stock > 0`) instead of read-then-write in application code, or added row-level locking (`SELECT ... FOR UPDATE`).
- **Prevention**: Establish a pattern/library for atomic stock updates; code review flags any read-modify-write sequence on shared counters.

## Scenario 5: `asyncio` service appears to hang under load

- **Situation**: An async FastAPI service becomes unresponsive to new requests even though CPU usage looks low.
- **Investigation**: Profiled event loop; found a blocking synchronous call (`requests.get()`) inside an `async def` endpoint.
- **Root Cause**: A blocking library call inside an async function blocks the entire single-threaded event loop, so no other requests can be processed while it's waiting.
- **Fix**: Replaced the blocking HTTP client with an async-compatible one (`httpx.AsyncClient`/`aiohttp`), or wrapped the blocking call in `run_in_executor` if replacing the library wasn't immediately feasible.
- **Prevention**: Lint/review rule: no blocking I/O calls inside `async def` functions; document the list of approved async-safe libraries for the team.

## Scenario 6: Deployment fails due to dependency version mismatch

- **Situation**: A deploy that worked in staging fails in production with import/version errors.
- **Investigation**: Compared installed package versions between environments.
- **Root Cause**: `requirements.txt` didn't pin exact versions, and a transitive dependency released a breaking update between the staging and production deploy.
- **Fix**: Pinned exact versions using a lockfile (`pip-compile`, `poetry.lock`), redeployed with the known-good versions.
- **Prevention**: Always use a lockfile for reproducible builds; run staging and production deploys close together, or use the same lockfile artifact for both.

## Scenario 7: Data pipeline job crashes midway, leaving partial data

- **Situation**: A nightly ETL script processing millions of records crashes 80% through, leaving the destination table half-updated.
- **Investigation**: Checked logs — found an unhandled exception on a malformed record deep in the dataset.
- **Root Cause**: No error isolation per record — one bad record crashed the entire batch job with no transactional rollback or checkpointing.
- **Fix**: Wrapped per-record processing in try/except with logging for bad records (skip and continue, or route to a dead-letter table), wrapped the whole batch in a database transaction where feasible for atomicity, and added checkpointing for resumability.
- **Prevention**: Standardize a fault-tolerant ETL pattern (per-record error isolation + checkpointing + alerting) across all pipeline jobs.

## Scenario 8: Slow API response traced to N+1 ORM queries

- **Situation**: An endpoint listing orders with their items takes several seconds under normal load.
- **Investigation**: Enabled Django/SQLAlchemy query logging — saw one query per order to fetch its items (N+1 problem).
- **Root Cause**: ORM lazy-loading related objects in a loop instead of eager-loading them in a single query.
- **Fix**: Used `select_related`/`prefetch_related` (Django) or `joinedload`/`selectinload` (SQLAlchemy) to batch-fetch related data in one or two queries.
- **Prevention**: Add query count assertions in tests for critical endpoints; review ORM query logs in staging before release.
