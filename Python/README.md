# Python — Zero to Hero: Complete Study Guide

## Table of Contents
1. Theory & Fundamentals
2. Basics (Beginner)
3. Data Structures
4. Functions & Functional Programming
5. OOP in Python
6. Exception Handling
7. File Handling & Context Managers
8. Modules, Packages & Virtual Environments
9. Advanced Python (Decorators, Generators, Iterators, Multithreading)
10. Python for Real-World Applications (Web/Data/Scripting)

---

## 1. Theory & Fundamentals

### What is Python?
Python is a **high-level, interpreted, dynamically-typed, garbage-collected, multi-paradigm** (procedural, OOP, functional) programming language.

### CPython Execution Theory (Common Interview Topic)
1. Source code (`.py`) is compiled to **bytecode** (`.pyc`).
2. The **Python Virtual Machine (PVM)** interprets bytecode line by line.
3. Python uses a **Global Interpreter Lock (GIL)** — only one thread executes Python bytecode at a time, even on multi-core CPUs (major theory point for concurrency discussions).

### Python is Dynamically & Strongly Typed
- **Dynamically typed** — variable types are determined at runtime, not declared explicitly.
- **Strongly typed** — no implicit/silent type coercion between incompatible types (`"2" + 2` raises `TypeError`, unlike JS).

---

## 2. Basics (Beginner)

### Variables & Data Types
```python
age = 25            # int
price = 99.99        # float
name = "Rahul"        # str
is_active = True      # bool
nothing = None         # NoneType

print(type(age))  # <class 'int'>
```

### Operators
```python
print(10 // 3)   # 3  (floor division)
print(10 % 3)    # 1  (modulus)
print(2 ** 3)    # 8  (exponent)
print(5 == 5.0)  # True
```

### Control Flow
```python
age = 20
if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teen")
else:
    print("Child")

for i in range(5):
    print(i)

for item in ["a", "b", "c"]:
    print(item)

i = 0
while i < 3:
    print(i)
    i += 1

# List comprehension (very Pythonic, frequently asked)
squares = [x**2 for x in range(10) if x % 2 == 0]
```

### Strings
```python
s = "Hello World"
print(s.lower(), s.upper(), s.strip(), s.split(" "))
print(s[0:5])        # slicing: "Hello"
print(s[::-1])        # reversed string
print(f"Name: {name}, Age: {age}")   # f-strings
```

---

## 3. Data Structures

### Lists, Tuples, Sets, Dicts (Core Interview Topic)
```python
# List - mutable, ordered
lst = [1, 2, 3]
lst.append(4)
lst.remove(2)
lst.sort(reverse=True)

# Tuple - immutable, ordered
t = (1, 2, 3)
# t[0] = 5  -> TypeError, tuples are immutable

# Set - mutable, unordered, unique elements
s = {1, 2, 2, 3}   # {1, 2, 3}
s.add(4)
s2 = {3, 4, 5}
print(s & s2, s | s2, s - s2)  # intersection, union, difference

# Dict - key-value pairs, mutable, ordered (since Python 3.7)
d = {"name": "Rahul", "age": 25}
d["city"] = "Pune"
for key, value in d.items():
    print(key, value)
```

**List vs Tuple vs Set vs Dict (frequently asked comparison):**
| Type | Mutable | Ordered | Duplicates | Use Case |
|---|---|---|---|---|
| List | Yes | Yes | Yes | General ordered collection |
| Tuple | No | Yes | Yes | Fixed data, dict keys, function returns |
| Set | Yes | No | No | Membership tests, dedup, math set ops |
| Dict | Yes | Yes (3.7+) | Keys unique | Key-value lookups |

### Slicing & Comprehensions
```python
nums = list(range(10))
print(nums[2:5])     # [2,3,4]
print(nums[::2])     # every 2nd element
squares_dict = {x: x**2 for x in range(5)}
unique_evens = {x for x in nums if x % 2 == 0}
gen = (x**2 for x in nums)   # generator expression - lazy evaluation
```

---

## 4. Functions & Functional Programming

```python
def add(a, b=10):          # default parameter
    return a + b

def total(*args, **kwargs):  # *args tuple, **kwargs dict
    print(args, kwargs)

square = lambda x: x**2

# map, filter, reduce
nums = [1,2,3,4,5]
doubled = list(map(lambda x: x*2, nums))
evens = list(filter(lambda x: x % 2 == 0, nums))
from functools import reduce
total_sum = reduce(lambda a,b: a+b, nums)
```

### Scope & Closures
```python
def outer():
    count = 0
    def inner():
        nonlocal count
        count += 1
        return count
    return inner

counter = outer()
print(counter())  # 1
print(counter())  # 2
```

### Args unpacking & Type Hints (Modern Python)
```python
def greet(name: str, age: int = 18) -> str:
    return f"{name} is {age} years old"

def total(*nums: int) -> int:
    return sum(nums)
```

---

## 5. OOP in Python

```python
class Animal:
    species_count = 0  # class variable

    def __init__(self, name, sound):
        self.name = name          # instance variable
        self.sound = sound
        Animal.species_count += 1

    def make_sound(self):
        return f"{self.name} says {self.sound}"

    def __str__(self):            # dunder / magic method
        return f"Animal({self.name})"

    @staticmethod
    def general_info():
        return "Animals are living organisms"

    @classmethod
    def create_dog(cls, name):
        return cls(name, "Woof")

class Dog(Animal):                # inheritance
    def __init__(self, name):
        super().__init__(name, "Woof")

    def make_sound(self):          # method overriding (polymorphism)
        return f"{self.name} barks loudly"

d = Dog("Rex")
print(d.make_sound())
```

### Four Pillars of OOP (Frequently Asked Theory)
- **Encapsulation** — bundling data + methods; using `_protected` / `__private` naming conventions.
- **Abstraction** — hiding implementation details, exposing only what's needed (via ABCs/interfaces).
- **Inheritance** — a class derives attributes/behavior from another class.
- **Polymorphism** — same interface, different implementations (method overriding, duck typing).

### Abstract Base Classes
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14 * self.r ** 2
```

### Dunder Methods (Common in Interviews)
```python
class Vector:
    def __init__(self, x, y): self.x, self.y = x, y
    def __add__(self, other): return Vector(self.x + other.x, self.y + other.y)
    def __repr__(self): return f"Vector({self.x}, {self.y})"
    def __eq__(self, other): return self.x == other.x and self.y == other.y
    def __len__(self): return 2
```

---

## 6. Exception Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (TypeError, ValueError) as e:
    print(f"Type/Value error: {e}")
else:
    print("Runs if no exception")
finally:
    print("Always runs (cleanup)")

# Custom exceptions
class InsufficientFundsError(Exception):
    def __init__(self, message="Not enough balance"):
        super().__init__(message)

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError()
    return balance - amount
```

---

## 7. File Handling & Context Managers

```python
# Reading/Writing files
with open("data.txt", "r") as f:
    content = f.read()
    lines = f.readlines()

with open("output.txt", "w") as f:
    f.write("Hello File\n")

# Custom context manager
class Timer:
    def __enter__(self):
        import time; self.start = time.time(); return self
    def __exit__(self, exc_type, exc_val, exc_tb):
        import time; print(f"Elapsed: {time.time() - self.start}s")

with Timer():
    sum(range(1000000))
```
**Theory:** `with` guarantees `__exit__`/cleanup (closing file handles, releasing locks) runs even if an exception occurs — avoids resource leaks, a common real-world bug source without it.

### Working with JSON/CSV (Very Common Real-World Task)
```python
import json, csv

with open("data.json") as f:
    data = json.load(f)
json_str = json.dumps({"a": 1}, indent=2)

with open("data.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)
```

---

## 8. Modules, Packages & Virtual Environments

```python
# mymodule.py
def greet(): return "hello"

# main.py
import mymodule
from mymodule import greet
from mymodule import greet as g
```

### Virtual Environments (Real-World Project Setup — Frequently Asked)
```bash
python -m venv venv
source venv/bin/activate      # Linux/Mac
venv\Scripts\activate         # Windows
pip install requests
pip freeze > requirements.txt
pip install -r requirements.txt
```
**Why:** Isolates project dependencies so different projects can use different package versions without conflicts.

---

## 9. Advanced Python

### Decorators (Very Common Interview Topic)
```python
def log_time(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time()-start:.4f}s")
        return result
    return wrapper

@log_time
def slow_function():
    import time; time.sleep(1)

# Decorator with arguments
def repeat(n):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hi(): print("Hi")
```

### Generators & Iterators (Common Interview Topic)
```python
def fibonacci(limit):
    a, b = 0, 1
    while a < limit:
        yield a
        a, b = b, a + b

for num in fibonacci(20):
    print(num)

# Custom iterator
class Counter:
    def __init__(self, limit): self.limit = limit; self.current = 0
    def __iter__(self): return self
    def __next__(self):
        if self.current >= self.limit: raise StopIteration
        self.current += 1
        return self.current
```
**Generators vs normal functions:** Generators use `yield` to produce values lazily (one at a time, pausing state between calls) instead of computing/returning everything at once — crucial for memory efficiency with large datasets.

### Multithreading vs Multiprocessing (GIL — Very Common Interview Theory)
```python
import threading, multiprocessing

def worker():
    print("Working")

# Threading - good for I/O-bound tasks (GIL released during I/O wait)
t = threading.Thread(target=worker)
t.start(); t.join()

# Multiprocessing - good for CPU-bound tasks (separate processes, bypasses GIL)
p = multiprocessing.Process(target=worker)
p.start(); p.join()
```
**Theory:** Because of the GIL, threading in Python doesn't give true parallel CPU execution — it's best for I/O-bound work (network calls, file I/O). For CPU-bound work (heavy computation), use `multiprocessing` (separate processes, separate memory, separate GIL each) or libraries like NumPy that release the GIL internally.

### Async Python (Modern, common in APIs)
```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)
    return "data"

async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())
```

### `*args`, `**kwargs`, `is` vs `==`, Mutable Default Args (Gotchas)
```python
# Common gotcha: mutable default argument
def add_item(item, items=[]):   # BAD - list persists across calls!
    items.append(item)
    return items

def add_item_fixed(item, items=None):  # GOOD
    if items is None: items = []
    items.append(item)
    return items

# is vs ==
a = [1,2,3]; b = [1,2,3]
print(a == b)  # True  (value equality)
print(a is b)  # False (identity/reference equality)
```

---

## 10. Python for Real-World Applications

### Web Development (Flask example)
```python
from flask import Flask, jsonify, request
app = Flask(__name__)

@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    return jsonify({"id": user_id, "name": "Rahul"})

@app.route('/users', methods=['POST'])
def create_user():
    data = request.get_json()
    return jsonify(data), 201

if __name__ == '__main__':
    app.run(debug=True)
```

### Data Handling (Pandas — extremely common in real jobs)
```python
import pandas as pd
df = pd.read_csv("data.csv")
df.head()
df.groupby("department")["salary"].mean()
df[df["salary"] > 50000]
df.to_csv("output.csv", index=False)
```

### Real-World Best Practices
- Use **PEP 8** style conventions (naming, spacing) for readable, maintainable code.
- Use **type hints** for clarity in larger codebases.
- Use **logging module** instead of `print()` in production code.
- Always use **virtual environments** per project.
- Write **unit tests** with `pytest` / `unittest`.
- Use **list/dict comprehensions** over manual loops for cleaner Pythonic code where readable.

```python
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
logger.info("Application started")
logger.error("Something went wrong")
```

---

## Quick Revision Cheat-Sheet
| Concept | One-Line Recall |
|---|---|
| GIL | Only one thread executes Python bytecode at a time |
| List vs Tuple | List mutable; Tuple immutable |
| `is` vs `==` | `is`=identity(same object); `==`=value equality |
| Generators | Lazy, memory-efficient, use `yield` |
| Decorators | Functions that wrap other functions to add behavior |
| Threading vs Multiprocessing | Threading=I/O-bound; Multiprocessing=CPU-bound (bypasses GIL) |
