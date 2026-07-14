# Python — Latest Market Standard Questions (2026–2027)

## Modern Python & Async

**Q1. What's new/standard in Python typing as of recent versions (3.10-3.13)?**
Structural pattern matching (`match`/`case`), `X | Y` union syntax instead of `Optional[X]`/`Union`, improved generics syntax (`class Stack[T]:` in 3.12+), and stricter type-checking adoption via `mypy`/`pyright` as a CI standard in most 2026 teams.

```python
match command:
    case "start":
        ...
    case "stop" | "halt":
        ...
    case _:
        ...
```

**Q2. Why has `asyncio` become close to a default expectation for backend roles?**
Modern Python web frameworks (FastAPI, Django with async views) and I/O-heavy workloads (APIs, microservices, AI inference calls) benefit heavily from async concurrency; most 2026 backend job postings expect comfort with `async`/`await` patterns.

**Q3. What is structural pattern matching and when is it preferred over if/elif chains?**
`match`/`case` (Python 3.10+) offers cleaner destructuring/matching on data shapes (useful for parsing structured data like JSON-like dicts, enums, or command dispatch) compared to long if/elif chains.

**Q4. What's the difference between Pydantic models and plain dataclasses, and why is Pydantic so widely used in 2026?**
`dataclasses` (standard library) give you structured classes with less boilerplate but no runtime validation. Pydantic adds runtime data validation, parsing, and serialization — foundational to FastAPI and widely used in AI/LLM tooling for structured output validation.

```python
from pydantic import BaseModel
class User(BaseModel):
    name: str
    age: int
```

## AI / Data Engineering Adjacent

**Q5. How is Python used in building LLM/AI applications today?**
Frameworks like LangChain/LlamaIndex for orchestration, `openai`/`anthropic` SDKs for API calls, Pydantic for structured output validation, `asyncio` for concurrent API calls, FastAPI for serving inference endpoints.

**Q6. How would you handle streaming responses from an LLM API in Python?**
```python
import asyncio
async def stream_response(client, prompt):
    async for chunk in client.stream(prompt):
        print(chunk, end="", flush=True)
```
Discuss using async generators/streaming APIs to process tokens as they arrive rather than waiting for the full response.

**Q7. What is vectorization and why does it matter for data-heavy Python code?**
Using NumPy/pandas operations that apply to whole arrays at once (implemented in C under the hood) instead of Python-level loops — dramatically faster for numerical/data processing workloads.

**Q8. What's the role of Python in MLOps/data pipelines (Airflow, Dagster, Prefect)?**
Python is the primary language for defining pipeline tasks/DAGs, orchestrating ETL/ML training jobs, and integrating with cloud data warehouses.

## Modern Engineering Practices

**Q9. What's the standard modern Python project tooling stack in 2026?**
`uv`/`poetry`/`pip-tools` for dependency management, `ruff` for linting/formatting (replacing separate flake8+black+isort in many teams), `mypy`/`pyright` for type checking, `pytest` for testing, pre-commit hooks for enforcing standards.

**Q10. How do you structure a production-grade FastAPI application?**
Discuss layered architecture (routers/endpoints, services/business logic, repositories/data access), dependency injection via FastAPI's `Depends`, Pydantic schemas for request/response validation, and async database drivers.

**Q11. What is dependency injection and how does FastAPI implement it?**
A pattern where dependencies (DB sessions, auth, config) are provided to a function rather than hardcoded inside it, improving testability. FastAPI implements this via the `Depends()` system.

**Q12. How do you handle configuration/secrets management in a modern Python app?**
Environment variables loaded via `pydantic-settings`/`python-dotenv`, secrets manager integration (AWS Secrets Manager, Vault) rather than hardcoding, separate configs per environment (dev/staging/prod).

**Q13. What testing practices are expected in a modern Python codebase?**
`pytest` with fixtures, parametrized tests, mocking external dependencies (`unittest.mock`/`pytest-mock`), coverage thresholds enforced in CI, and increasingly, property-based testing (`hypothesis`) for critical logic.

**Q14. How would you profile and optimize a slow Python function in production?**
Use `cProfile`/`py-spy` (production-safe sampling profiler) to identify bottlenecks, check for unnecessary I/O in loops, consider caching (`lru_cache`, Redis), and vectorize where applicable before considering a rewrite in a faster language/extension.

**Q15. What's the difference between synchronous WSGI (Flask/Django classic) and ASGI (FastAPI/Django async) deployment?**
WSGI handles one request per worker thread/process synchronously. ASGI supports async request handling, WebSockets, and higher concurrency for I/O-bound workloads with fewer resources — the standard for new high-concurrency Python web services in 2026.
