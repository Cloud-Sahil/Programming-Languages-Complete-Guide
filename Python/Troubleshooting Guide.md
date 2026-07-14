# Python — Troubleshooting Guide (Common Errors & Fixes)

## 1. Common Runtime Errors

**Error**: `IndentationError: unexpected indent` / `expected an indented block`
- Cause: mixing tabs and spaces, inconsistent indentation.
- Fix: use a consistent indentation style (4 spaces is PEP 8 standard); configure editor to show whitespace and convert tabs to spaces.

**Error**: `NameError: name 'x' is not defined`
- Cause: variable used before assignment, typo, or wrong scope (variable defined inside a function but used outside).
- Fix: check spelling, verify variable is defined in an accessible scope before use.

**Error**: `TypeError: unsupported operand type(s)`
- Cause: performing an operation between incompatible types (e.g. `"5" + 5`).
- Fix: explicitly convert types (`int("5") + 5` or `str(5) + "5"`).

**Error**: `AttributeError: 'NoneType' object has no attribute 'x'`
- Cause: calling a method/attribute on `None` — often because a function that should return a value implicitly returned `None` (missing `return` statement), or a dict `.get()` returned `None`.
- Fix: check for `None` before accessing attributes; verify all code paths in the function actually `return` a value.

**Error**: `KeyError: 'key'`
- Cause: accessing a dict key that doesn't exist.
- Fix: use `.get(key, default)` instead of `dict[key]` when the key might not exist, or check `if key in dict` first.

**Error**: `IndexError: list index out of range`
- Cause: accessing an index beyond the list's length (common in off-by-one loop errors).
- Fix: check `len()` before indexing, use safe iteration patterns (`enumerate`, slicing) instead of manual index arithmetic.

## 2. Import / Module Issues

**Error**: `ModuleNotFoundError: No module named 'x'`
- Cause: package not installed, wrong virtual environment active, or a typo in the import.
- Fix: `pip install <package>` in the correct environment; verify `which python`/`pip list` shows the expected environment is active.

**Error**: `ImportError: cannot import name 'x' from partially initialized module` (circular import)
- Cause: two modules importing from each other, creating a circular dependency.
- Fix: restructure code to remove the circular dependency (move shared code to a third module), or use local imports inside functions as a workaround.

## 3. Mutable Default Argument Bug (very common, subtle)

```python
def add_item(item, items=[]):
    items.append(item)
    return items

add_item("a")  # ['a']
add_item("b")  # ['a', 'b']  <- unexpected! Default list is shared across all calls
```
- Fix: use `None` as default and initialize inside the function body.

## 4. Mutable Object Aliasing Bugs

**Symptom**: Modifying one variable unexpectedly changes another
```python
a = [1, 2, 3]
b = a          # b references the SAME list, not a copy
b.append(4)
print(a)       # [1, 2, 3, 4] — a changed too!
```
- Fix: use `b = a.copy()` (shallow) or `copy.deepcopy(a)` (deep, for nested structures) when an independent copy is needed.

## 5. Concurrency Issues

**Symptom**: Threading doesn't speed up a CPU-bound task
- Cause: the Global Interpreter Lock (GIL) prevents true parallel execution of Python bytecode across threads.
- Fix: use `multiprocessing` for CPU-bound parallelism instead of `threading`.

**Symptom**: `asyncio` code hangs or doesn't run concurrently
- Cause: calling a blocking (synchronous) function inside an `async` function without `await`, which blocks the event loop.
- Fix: use async-compatible libraries (`aiohttp` instead of `requests`), or run blocking calls in an executor (`loop.run_in_executor`).

**Symptom**: Deadlock or race condition in multi-threaded code
- Cause: multiple threads accessing shared mutable state without synchronization.
- Fix: use `threading.Lock`, minimize shared mutable state, or prefer `multiprocessing`/`queue.Queue` for safer inter-process/thread communication.

## 6. Performance Issues

**Symptom**: Loop-heavy code is very slow
- Cause: using explicit Python-level loops instead of vectorized operations (especially with data processing), or repeated expensive operations inside a loop (e.g. re-opening a file, re-compiling a regex).
- Fix: use list comprehensions/generator expressions, use NumPy/pandas vectorized operations for numerical data, hoist invariant computations (like `re.compile`) outside loops.

**Symptom**: High memory usage processing large files
- Cause: loading an entire large file into memory at once (`f.read()` on a huge file).
- Fix: process line-by-line/in chunks (`for line in f:`), use generators instead of building large lists in memory.

## 7. Virtual Environment / Dependency Issues

**Symptom**: "Works on my machine" — different behavior across environments
- Cause: different package versions installed, no lockfile/pinned dependencies, using global Python instead of a virtual environment.
- Fix: always use a virtual environment (`venv`, `poetry`, `pipenv`), pin dependencies in `requirements.txt`/`pyproject.toml`, use `pip freeze > requirements.txt`.

**Error**: `pip install` fails with a compilation error
- Cause: missing system-level build dependencies for a package with C extensions.
- Fix: install required system packages (build-essential, python-dev), or use a pre-built wheel where available.

## 8. Exception Handling Anti-Patterns

**Symptom**: Errors silently swallowed, bugs hard to trace
```python
try:
    risky_operation()
except Exception:
    pass  # BAD: silently hides the real error
```
- Fix: catch specific exceptions, log the error, and only suppress exceptions intentionally with clear reasoning (and ideally still log them).

## 9. Debugging Toolkit
1. Read the full traceback bottom-to-top — the actual error is usually at the very bottom, the call chain above shows how you got there.
2. Use `print()` debugging or `pdb`/`breakpoint()` for step-through debugging.
3. Use `logging` module instead of scattered `print()` statements for production code.
4. Use a linter (`flake8`, `pylint`) and type checker (`mypy`) to catch issues before runtime.
5. Reproduce the bug with a minimal example before diving into the full codebase.
6. Check `pip list`/`python --version` to confirm you're in the expected environment.
