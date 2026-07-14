# JavaScript — Troubleshooting Guide (Common Errors & Fixes)

## 1. Common Runtime Errors

**Error**: `Uncaught TypeError: Cannot read properties of undefined (reading 'x')`
- Cause: accessing a property on `undefined`/`null` (API response not yet loaded, wrong object path, typo in key name).
- Fix: use optional chaining (`obj?.x`), guard with conditional checks, verify API response shape, add default values.

**Error**: `Uncaught ReferenceError: x is not defined`
- Cause: variable used before declaration (outside its scope), typo, or missing import.
- Fix: check scope/declaration, check import statements, check for typos.

**Error**: `Uncaught SyntaxError: Unexpected token`
- Cause: missing bracket/comma/semicolon, using newer syntax in an unsupported environment, invalid JSON.parse input.
- Fix: check syntax carefully, verify transpilation/build config (Babel) covers the syntax used, validate JSON before parsing.

**Error**: `Maximum call stack size exceeded`
- Cause: infinite recursion — missing or incorrect base case in a recursive function.
- Fix: verify base case terminates correctly, add recursion depth safeguards, convert to an iterative approach if needed.

## 2. Async / Promise Issues

**Symptom**: `UnhandledPromiseRejectionWarning` or silent failures
- Cause: missing `.catch()` on a promise chain, or missing `try/catch` around an `await`.
- Fix: always handle rejections; add a global `unhandledrejection` listener as a safety net during development.

**Symptom**: Code runs in the wrong order / data isn't ready yet
- Cause: not awaiting an async function, treating an async call as synchronous.
- Fix: use `await` inside an `async` function, or `.then()` chaining; verify the calling function is also `async` if using `await`.

**Symptom**: `await` used outside an `async` function — syntax error
- Fix: wrap in an `async` function, or use an IIFE: `(async () => { await foo(); })();`

**Symptom**: Loop with `await` doesn't run in parallel as expected
- Cause: using `await` inside a `for` loop runs sequentially, not in parallel.
- Fix: use `Promise.all(items.map(item => asyncFn(item)))` for true parallel execution.

## 3. `this` Binding Issues

**Symptom**: `this` is `undefined` or wrong inside a callback
- Cause: regular function passed as a callback loses its original `this` context (e.g. passed to `setTimeout`, event listener, or array method).
- Fix: use an arrow function (lexical `this`), or `.bind(this)` explicitly.

```js
class Timer {
  constructor() { this.count = 0; }
  start() {
    setTimeout(function() {
      this.count++; // 'this' is undefined/window here, NOT the Timer instance
    }, 1000);
  }
}
// Fix: use arrow function
setTimeout(() => { this.count++; }, 1000);
```

## 4. Closures & Loop Pitfalls

**Symptom**: All callbacks in a loop reference the same final value
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // prints 3, 3, 3
}
```
- Cause: `var` is function-scoped, so all closures share the same `i`.
- Fix: use `let` (block-scoped, new binding per iteration) — prints 0, 1, 2.

## 5. Memory Leaks

**Symptom**: App gets slower over time / memory usage keeps growing
- Cause: event listeners not removed, timers not cleared, detached DOM nodes still referenced, growing global caches/arrays.
- Fix: `removeEventListener` on cleanup, `clearInterval`/`clearTimeout`, use `WeakMap`/`WeakSet` for caches keyed by objects, review component unmount/cleanup logic (React `useEffect` cleanup function).

## 6. Equality / Type Coercion Bugs

**Symptom**: `if (value == 0)` behaves unexpectedly (matches `""`, `false`, `null` too loosely)
- Fix: always use `===`/`!==`; be explicit about type checks.

**Symptom**: `NaN !== NaN` causing broken comparisons
- Fix: use `Number.isNaN(value)` to check for NaN, never `value === NaN`.

**Symptom**: Array/object comparison with `===` always false
- Cause: `===` compares object references, not deep structural equality.
- Fix: use a deep-equality utility (`lodash.isEqual`) or `JSON.stringify()` comparison for simple cases (careful with key order).

## 7. Array/Object Mutation Bugs

**Symptom**: Original array/object changes unexpectedly after "copying" it
- Cause: `const copy = original;` copies the reference, not the value (for objects/arrays).
- Fix: use spread (`{...obj}`, `[...arr]`) for shallow copy, or `structuredClone(obj)` for deep copy.

**Symptom**: React/Vue component doesn't re-render after updating state
- Cause: mutating state directly instead of creating a new reference (e.g. `state.items.push(x)` instead of `setState([...state.items, x])`).
- Fix: always create new array/object references when updating state in reactive frameworks.

## 8. Event Handling Issues

**Symptom**: Click handler fires multiple times
- Cause: event listener attached multiple times (e.g. on every re-render without cleanup), or event bubbling triggering parent and child handlers both.
- Fix: remove old listeners before adding new ones, or use `e.stopPropagation()` where appropriate, verify listener attachment logic runs only once.

**Symptom**: Event delegation not catching dynamically added elements
- Cause: listener attached directly to child elements added before the dynamic ones existed.
- Fix: attach listener to a stable parent container and use `e.target.closest(selector)` to identify the actual clicked element.

## 9. Performance Issues

**Symptom**: UI freezes/janks during heavy computation
- Cause: long synchronous task blocking the single main thread.
- Fix: break work into chunks (`setTimeout`/`requestIdleCallback`), or offload to a Web Worker.

**Symptom**: Scroll/resize handlers causing lag
- Cause: expensive logic running on every single event firing (scroll fires very frequently).
- Fix: debounce or throttle the handler.

## 10. Debugging Toolkit
1. `console.log` strategically, or use `debugger;` + browser DevTools breakpoints.
2. Check the Network tab for failed/slow API calls.
3. Use the Sources tab to step through code line-by-line.
4. Check the Console for stack traces — read from the top (innermost call) down.
5. Use React/Vue DevTools for component state/props inspection.
6. Add source maps in production builds for readable stack traces from minified code.
