# Python — Rapid Fire Questions

1. **Is Python statically or dynamically typed?** Dynamically typed.
2. **What does GIL stand for?** Global Interpreter Lock.
3. **Which is immutable: list or tuple?** Tuple.
4. **What does `len()` do?** Returns the length of a sequence/collection.
5. **Which keyword defines a function?** `def`.
6. **Which keyword defines an anonymous function?** `lambda`.
7. **What does `*args` collect?** Extra positional arguments as a tuple.
8. **What does `**kwargs` collect?** Extra keyword arguments as a dict.
9. **Which module provides true parallelism bypassing the GIL?** `multiprocessing`.
10. **Which library is used for async programming?** `asyncio`.
11. **What does `self` represent in a class method?** The instance calling the method.
12. **What decorator defines a class method?** `@classmethod`.
13. **What decorator defines a static method?** `@staticmethod`.
14. **What does `__init__` do?** Initializes a new instance (constructor).
15. **What does `__str__` control?** The human-readable string representation of an object.
16. **What is a list comprehension?** A concise way to create lists: `[x for x in iterable]`.
17. **What does `yield` do?** Pauses a generator function, returning a value, and resumes later.
18. **What is PEP 8?** Python's official style guide.
19. **Which function checks an object's type?** `isinstance()`.
20. **What does `is` compare?** Object identity (same memory reference).
21. **What does `==` compare?** Value equality.
22. **What's the difference between `.append()` and `.extend()` on a list?** `append` adds one element; `extend` adds each element of an iterable individually.
23. **Which data structure ensures unique elements?** `set`.
24. **What module handles regular expressions?** `re`.
25. **What is a virtual environment used for?** Isolating project dependencies.
26. **Which tool manages Python dependencies with a lockfile?** `poetry`/`pip-tools`/`uv`.
27. **What does `with open(...) as f:` provide?** Automatic resource cleanup (context manager) — file closes automatically.
28. **What exception is raised for division by zero?** `ZeroDivisionError`.
29. **What exception is raised for a missing dict key?** `KeyError`.
30. **What exception is raised for an out-of-range list index?** `IndexError`.
31. **Which library is standard for testing?** `pytest`.
32. **What does `@property` do?** Lets a method be accessed like an attribute.
33. **What is duck typing?** Behavior-based typing — "if it behaves like X, treat it as X."
34. **What does `copy.deepcopy()` do?** Recursively copies an object and all nested objects.
35. **Which framework is popular for building async APIs quickly with validation?** FastAPI.
