# Python — Interview Questions (1–4 Years Experience)

## Conceptual

**Q1. Difference between a list and a tuple?**
List: mutable, slightly slower, uses `[]`. Tuple: immutable, slightly faster, hashable (can be a dict key/set element), uses `()`.

**Q2. What is the GIL and how does it affect multithreading?**
Global Interpreter Lock — ensures only one thread executes Python bytecode at a time in CPython. Threading helps I/O-bound tasks but not CPU-bound ones (use `multiprocessing` for that).

**Q3. Difference between `is` and `==`?**
`==` compares values for equality. `is` compares object identity (whether both references point to the same object in memory).

**Q4. What are `*args` and `**kwargs`?**
`*args` collects extra positional arguments into a tuple; `**kwargs` collects extra keyword arguments into a dict.

**Q5. What's the difference between deep copy and shallow copy?**
Shallow copy (`copy.copy()`) copies the outer object but nested objects are still shared references. Deep copy (`copy.deepcopy()`) recursively copies all nested objects too.

**Q6. What are decorators?**
Functions that wrap another function to modify/extend its behavior without changing its source code, using the `@decorator_name` syntax.

**Q7. What is a generator? How is it different from a normal function?**
A function using `yield` that produces values lazily, one at a time, pausing state between calls — memory-efficient for large sequences, unlike a normal function which computes and returns everything at once.

**Q8. Explain list comprehension vs generator expression.**
List comprehension `[x for x in ...]` builds the entire list in memory immediately. Generator expression `(x for x in ...)` produces values lazily on demand, using less memory.

**Q9. What is the difference between `@staticmethod`, `@classmethod`, and instance methods?**
- Instance method: takes `self`, operates on instance data.
- `@classmethod`: takes `cls`, operates on class-level data, callable on the class itself.
- `@staticmethod`: takes neither, behaves like a plain function namespaced inside the class.

**Q10. What is duck typing?**
"If it walks like a duck and quacks like a duck, it's a duck" — Python cares about an object's behavior/methods rather than its explicit type, enabling flexible polymorphism.

## Practical / Coding

**Q11. Reverse a string.**
```python
s = "hello"
reversed_s = s[::-1]
```

**Q12. Check if a string is a palindrome.**
```python
def is_palindrome(s):
    s = s.lower().replace(" ", "")
    return s == s[::-1]
```

**Q13. Find duplicates in a list.**
```python
def find_duplicates(lst):
    seen, dupes = set(), set()
    for x in lst:
        if x in seen:
            dupes.add(x)
        seen.add(x)
    return list(dupes)
```

**Q14. Count word frequency in a string.**
```python
from collections import Counter
def word_freq(text):
    return Counter(text.lower().split())
```

**Q15. Merge two dictionaries.**
```python
merged = {**dict1, **dict2}
# or (Python 3.9+): merged = dict1 | dict2
```

**Q16. Write a decorator that logs function execution time.**
```python
import time, functools

def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time()-start:.4f}s")
        return result
    return wrapper
```

**Q17. Implement a simple LRU cache.**
```python
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_computation(n):
    return n ** 2
```

**Q18. Flatten a nested list.**
```python
def flatten(lst):
    result = []
    for item in lst:
        if isinstance(item, list):
            result.extend(flatten(item))
        else:
            result.append(item)
    return result
```

## Scenario-Based

**Q19. How would you handle a function that might be called with a mutable default argument?**
Never use a mutable object (list/dict) as a default argument directly — use `None` and initialize inside the function body.

**Q20. How would you process a very large file without running out of memory?**
Read and process line-by-line (`for line in f:`) or in chunks, rather than loading the entire file with `.read()`.

**Q21. How would you speed up a CPU-bound Python script?**
Use `multiprocessing` for true parallelism, optimize algorithms/data structures, consider vectorization (NumPy) for numerical work, or rewrite hot paths in a compiled extension if needed.

**Q22. How do you handle exceptions in a way that doesn't hide bugs?**
Catch specific exception types (not bare `except:`), log the error with context, and only suppress exceptions when you have a clear, justified reason.

**Q23. What's your approach to writing unit tests for a function with external dependencies (e.g. an API call)?**
Use mocking (`unittest.mock`/`pytest-mock`) to isolate the function from the external dependency, testing the function's logic independently.
