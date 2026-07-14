# JavaScript — Rapid Fire Questions

1. **Is JavaScript single-threaded?** Yes (one call stack; async handled via event loop).
2. **`typeof null`?** `"object"` (a well-known historical quirk).
3. **`typeof undefined`?** `"undefined"`.
4. **Which keyword creates block-scoped, reassignable variables?** `let`.
5. **Which keyword creates block-scoped, non-reassignable variables?** `const`.
6. **What does `NaN === NaN` return?** `false`.
7. **How do you check for NaN correctly?** `Number.isNaN(value)`.
8. **What does `[] == false` evaluate to?** `true` (loose equality coercion).
9. **What is a closure in one line?** A function that remembers variables from its outer scope.
10. **What runs first: microtasks or macrotasks?** Microtasks (e.g. Promises) before macrotasks (e.g. setTimeout).
11. **Which array method transforms each element and returns a new array?** `.map()`.
12. **Which array method removes elements based on a condition?** `.filter()`.
13. **Which array method reduces an array to a single value?** `.reduce()`.
14. **Which method converts an array-like/iterable to a true array?** `Array.from()`.
15. **What does the spread operator do?** Expands an iterable into individual elements.
16. **What does the rest parameter do?** Collects remaining arguments into an array.
17. **Which operator falls back only on null/undefined?** `??` (nullish coalescing).
18. **What does `?.` do?** Optional chaining — safely access nested properties without throwing on null/undefined.
19. **What is the default value of an uninitialized `var`?** `undefined`.
20. **Which keyword declares a class in ES6?** `class`.
21. **What does `bind()` do?** Returns a new function with `this` permanently bound.
22. **Difference between `call()` and `apply()`?** `call` takes arguments individually; `apply` takes an array of arguments.
23. **What is event delegation?** Attaching one listener to a parent to handle events from its children.
24. **What is debouncing used for?** Delaying execution until activity stops (e.g. search input).
25. **What is throttling used for?** Limiting execution to at most once per interval (e.g. scroll handler).
26. **What does `Promise.all()` do if one promise rejects?** The whole `Promise.all` rejects immediately.
27. **Which Promise method never rejects, giving status of all promises?** `Promise.allSettled()`.
28. **What is a generator function syntax?** `function* name() { yield value; }`.
29. **What does `JSON.stringify()` do?** Converts a JS object/value into a JSON string.
30. **What's the modern native deep-clone function?** `structuredClone()`.
31. **What does `Array.isArray()` check?** Whether a value is an array.
32. **What's the difference between `Object.freeze()` and `const`?** `const` prevents reassignment of the variable; `Object.freeze()` prevents mutation of the object's properties.
33. **What is `strict mode` and how do you enable it?** `"use strict";` — enforces safer JS behavior, catches silent errors.
34. **What API cancels an in-flight fetch request?** `AbortController`.
35. **What does `WeakMap` allow that `Map` doesn't?** Garbage collection of keys with no other references.
