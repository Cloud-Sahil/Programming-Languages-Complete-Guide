# JavaScript — Interview Questions (1–4 Years Experience)

## Conceptual

**Q1. Difference between `var`, `let`, and `const`?**
`var`: function-scoped, hoisted with `undefined`, re-declarable. `let`: block-scoped, hoisted but in temporal dead zone, reassignable. `const`: block-scoped, cannot be reassigned (though object/array contents can still mutate).

**Q2. What is a closure? Give a practical use case.**
A function that retains access to its outer scope's variables even after the outer function returns. Used for: private counters, memoization, module patterns, currying.

**Q3. Explain the event loop and how async code executes.**
Single call stack executes sync code; async operations (timers, I/O, promises) are handled outside the stack, and their callbacks are queued (microtasks run before macrotasks) and processed once the stack is empty.

**Q4. What is the difference between `==` and `===`?**
`==` performs type coercion before comparing; `===` compares both value and type without coercion. Always prefer `===`.

**Q5. Explain `this` in different contexts.**
Regular function: determined by how it's called (implicit binding via object, explicit via call/apply/bind, or global/undefined). Arrow function: lexically inherits `this` from enclosing scope. Class method: `this` refers to the instance when called via the instance.

**Q6. What is hoisting?**
Variable and function declarations are conceptually moved to the top of their scope before execution; `var` is hoisted and initialized as `undefined`, `let`/`const` are hoisted but stay in the "temporal dead zone" until declared.

**Q7. Difference between `null` and `undefined`?**
`undefined`: a variable declared but not assigned a value (or a missing property/argument). `null`: an intentional assignment representing "no value."

**Q8. What are Promises? Explain states.**
An object representing the eventual completion/failure of an async operation. States: pending, fulfilled, rejected.

**Q9. Explain event bubbling and capturing.**
Bubbling: event propagates from the target element up to ancestors. Capturing: propagates from the top ancestor down to the target. Default `addEventListener` uses bubbling phase unless the third argument is `true`.

**Q10. What's the difference between synchronous and asynchronous code?**
Synchronous: executes line by line, blocking further execution until done. Asynchronous: allows other code to run while waiting for an operation (network call, timer) to complete, via callbacks/promises/async-await.

## Practical / Coding

**Q11. Implement a debounce function.**
```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

**Q12. Flatten a nested array.**
```js
function flatten(arr) {
  return arr.reduce((acc, val) =>
    Array.isArray(val) ? acc.concat(flatten(val)) : acc.concat(val), []);
}
// or: arr.flat(Infinity);
```

**Q13. Remove duplicates from an array.**
```js
const unique = [...new Set(arr)];
```

**Q14. Deep clone an object.**
```js
const clone = structuredClone(obj); // modern, handles most cases
// or (loses functions/undefined): JSON.parse(JSON.stringify(obj))
```

**Q15. Write a polyfill for `Array.prototype.map`.**
```js
Array.prototype.myMap = function(callback) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(callback(this[i], i, this));
  }
  return result;
};
```

**Q16. Explain and implement currying.**
```js
const curry = fn => (...args) =>
  args.length >= fn.length ? fn(...args) : (...more) => curry(fn)(...args, ...more);
```

**Q17. Find the first non-repeating character in a string.**
```js
function firstUnique(str) {
  for (const char of str) {
    if (str.indexOf(char) === str.lastIndexOf(char)) return char;
  }
  return null;
}
```

**Q18. Convert a callback-based function to return a Promise.**
```js
function promisify(fn) {
  return (...args) => new Promise((resolve, reject) => {
    fn(...args, (err, result) => err ? reject(err) : resolve(result));
  });
}
```

## Scenario-Based

**Q19. How would you optimize a page with a slow, janky scroll event handler?**
Throttle the handler; avoid layout-triggering reads/writes inside it (batch DOM reads and writes); consider `IntersectionObserver` instead of scroll listeners for visibility-based logic.

**Q20. How do you handle an API call that might fail intermittently?**
Wrap in try/catch, implement retry logic with exponential backoff, show appropriate user feedback (loading/error states), consider request timeout handling with `AbortController`.

**Q21. How would you prevent a form from being submitted multiple times on double-click?**
Disable the submit button immediately on first click / use a loading state flag to block subsequent submissions until the request resolves.

**Q22. Explain how you'd debug a memory leak in a long-running single-page app.**
Use Chrome DevTools Memory tab, take heap snapshots over time, look for detached DOM nodes and growing object counts, check for uncleaned event listeners/timers/subscriptions.
