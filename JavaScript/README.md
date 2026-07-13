# JavaScript — Zero to Hero: Complete Study Guide

## Table of Contents
1. Theory & Engine Fundamentals
2. Basics (Beginner)
3. Functions & Scope
4. Objects, Arrays & Prototypes
5. Asynchronous JavaScript
6. ES6+ Modern Features
7. DOM Manipulation & Events
8. Advanced Concepts (Closures, Event Loop, Design Patterns)
9. Error Handling & Debugging
10. User-Side / Real Project Practices

---

## 1. Theory & Engine Fundamentals

### What is JavaScript?
JavaScript is a **single-threaded, interpreted (JIT-compiled), dynamically-typed, prototype-based** language that runs in browsers (via engines like V8) and servers (Node.js).

### JS Engine Theory (Frequently Asked)
1. **Parsing** — code is parsed into an AST (Abstract Syntax Tree).
2. **Compilation** — JIT (Just-In-Time) compiler (e.g., V8's Ignition + TurboFan) converts to bytecode/optimized machine code.
3. **Execution Context** — created for global code and each function call; contains variable environment, scope chain, and `this`.
4. **Call Stack** — tracks execution contexts (LIFO).
5. **Memory Heap** — where objects are allocated.

### Single-Threaded + Event Loop (Core Interview Theory)
JavaScript has ONE call stack (single-threaded) but achieves asynchronous, non-blocking behavior via the **Event Loop**, **Callback Queue** (macrotasks), and **Microtask Queue** (Promises/async-await) — covered in depth in Section 5.

---

## 2. Basics (Beginner)

### Variables & Data Types
```javascript
let age = 25;              // block-scoped, reassignable
const name = "Rahul";      // block-scoped, cannot be reassigned
var oldWay = "avoid this"; // function-scoped, hoisting quirks

// Primitive types
let num = 42;               // number
let str = "hello";          // string
let bool = true;            // boolean
let n = null;                // null
let u;                        // undefined
let big = 10n;                // bigint
let sym = Symbol('id');       // symbol

// typeof
console.log(typeof num);  // "number"
console.log(typeof null); // "object" (famous JS quirk)
```

### var vs let vs const (Classic Interview Q)
| | Scope | Hoisting | Reassignable | Redeclare |
|---|---|---|---|---|
| var | function | Yes (initialized as undefined) | Yes | Yes |
| let | block | Yes (in Temporal Dead Zone) | Yes | No |
| const | block | Yes (TDZ) | No | No |

### Operators & Type Coercion
```javascript
console.log(1 == '1');   // true  (loose equality, coerces types)
console.log(1 === '1');  // false (strict equality, checks type too)
console.log(0 == false); // true
console.log(NaN === NaN); // false (NaN is never equal to itself)
console.log([] + []);     // "" (array to string coercion)
console.log([] + {});     // "[object Object]"
```
**Always prefer `===` over `==`** to avoid unpredictable coercion bugs — extremely common interview point.

### Control Flow
```javascript
if (age >= 18) { console.log("Adult"); } else { console.log("Minor"); }

for (let i = 0; i < 5; i++) { console.log(i); }
for (const item of [1,2,3]) { console.log(item); }   // iterates values
for (const key in {a:1,b:2}) { console.log(key); }   // iterates keys

let i = 0;
while (i < 3) { console.log(i); i++; }

switch(age) {
    case 18: console.log("Just adult"); break;
    default: console.log("Other");
}
```

---

## 3. Functions & Scope

```javascript
function add(a, b) { return a + b; }               // function declaration (hoisted)
const multiply = function(a, b) { return a * b; };  // function expression
const square = (x) => x * x;                        // arrow function
const greet = (name = "Guest") => `Hello ${name}`;   // default param
function sum(...nums) { return nums.reduce((a,b) => a+b, 0); } // rest params
```

### Arrow Functions vs Regular Functions (Very Common Interview Q)
- Arrow functions **do not have their own `this`** — they inherit `this` from the enclosing lexical scope.
- Arrow functions cannot be used as constructors (`new` throws an error).
- Regular functions have their own `this`, `arguments` object, and can be used as constructors.

### Closures (Extremely Common Interview Topic)
```javascript
function counter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}
const increment = counter();
console.log(increment()); // 1
console.log(increment()); // 2
```
**Theory:** A closure is a function that "remembers" the variables from its lexical scope even after the outer function has finished executing. Used for data privacy, currying, memoization, module patterns.

### `this` Keyword (Very Common Interview Topic)
```javascript
const obj = {
    name: "Rahul",
    regular: function() { console.log(this.name); },   // "Rahul" - this = obj
    arrow: () => { console.log(this.name); }             // undefined - this = outer scope
};

// call, apply, bind
function greet() { console.log(`Hi, ${this.name}`); }
greet.call({name: "A"});
greet.apply({name: "B"});
const bound = greet.bind({name: "C"});
bound();
```

---

## 4. Objects, Arrays & Prototypes

### Object Basics
```javascript
const person = { name: "Rahul", age: 25, greet() { return `Hi ${this.name}`; } };
const { name, age } = person;          // destructuring
const clone = { ...person, city: "Pune" };  // spread
console.log(Object.keys(person), Object.values(person), Object.entries(person));
```

### Array Methods (High-Frequency Interview + Daily Use)
```javascript
const nums = [1,2,3,4,5];
nums.map(n => n * 2);              // [2,4,6,8,10]
nums.filter(n => n % 2 === 0);     // [2,4]
nums.reduce((acc, n) => acc + n, 0); // 15
nums.find(n => n > 3);              // 4
nums.findIndex(n => n > 3);         // 3
nums.some(n => n > 4);              // true
nums.every(n => n > 0);             // true
nums.forEach(n => console.log(n));
nums.sort((a,b) => b - a);          // descending
nums.slice(1,3);                     // shallow copy subset, non-mutating
nums.splice(1,2, 'a','b');           // mutating insert/remove
[...nums].reverse();
```

### Prototypes & Prototypal Inheritance (Core Theory)
```javascript
function Animal(name) { this.name = name; }
Animal.prototype.speak = function() { return `${this.name} makes a sound`; };

function Dog(name) { Animal.call(this, name); }
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.speak = function() { return `${this.name} barks`; };

const d = new Dog("Rex");
console.log(d.speak());
```
**Theory:** Every JS object has an internal `[[Prototype]]` link (accessible via `__proto__` or `Object.getPrototypeOf`). Property lookups traverse this **prototype chain** until found or reaching `null`. ES6 `class` syntax is syntactic sugar over this prototype-based inheritance.

### ES6 Classes
```javascript
class Animal {
    constructor(name) { this.name = name; }
    speak() { return `${this.name} makes a sound`; }
    static create(name) { return new Animal(name); }
}
class Dog extends Animal {
    speak() { return `${this.name} barks`; }
}
const d = new Dog("Rex");
```

---

## 5. Asynchronous JavaScript

### Callbacks → Promises → Async/Await (Evolution, Common Interview Narrative)
```javascript
// Callback (old style, can lead to "callback hell")
function fetchData(callback) {
    setTimeout(() => callback("data"), 1000);
}

// Promise
function fetchDataPromise() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve("data"), 1000);
    });
}
fetchDataPromise().then(data => console.log(data)).catch(err => console.error(err));

// async/await (cleanest, modern standard)
async function getData() {
    try {
        const data = await fetchDataPromise();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}
```

### Promise Combinators
```javascript
Promise.all([p1, p2, p3]);        // waits for all; rejects if any rejects
Promise.allSettled([p1, p2, p3]); // waits for all; never short-circuits
Promise.race([p1, p2, p3]);       // resolves/rejects with the first settled
Promise.any([p1, p2, p3]);        // resolves with first fulfilled; rejects only if all reject
```

### Event Loop (Critical Interview Theory)
```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
// Output: 1, 4, 3, 2
```
**Why:** Synchronous code runs first (call stack). Then **microtasks** (Promises, `queueMicrotask`) run before **macrotasks** (`setTimeout`, `setInterval`, I/O). The event loop checks: is the call stack empty? → drain the entire microtask queue → then take one macrotask → repeat.

### Fetch API (Real-World Usage)
```javascript
async function getUsers() {
    const res = await fetch('https://api.example.com/users');
    if (!res.ok) throw new Error(`HTTP error: ${res.status}`);
    const data = await res.json();
    return data;
}
```

---

## 6. ES6+ Modern Features

```javascript
// Template literals
const msg = `Hello, ${name}! You are ${age} years old.`;

// Destructuring
const [a, b, ...rest] = [1,2,3,4,5];
const { x, y: renamed = 10 } = { x: 1 };

// Spread/Rest
const merged = { ...obj1, ...obj2 };
function sum(...nums) { return nums.reduce((a,b) => a+b); }

// Optional chaining & nullish coalescing
const city = user?.address?.city ?? "Unknown";

// Modules
export const PI = 3.14;
export default function main() {}
import main, { PI } from './module.js';

// Map & Set
const map = new Map([['a',1],['b',2]]);
const set = new Set([1,2,2,3]); // {1,2,3}

// Generators
function* gen() { yield 1; yield 2; yield 3; }
for (const val of gen()) console.log(val);
```

---

## 7. DOM Manipulation & Events

```javascript
const el = document.querySelector('.item');
const els = document.querySelectorAll('.item');
document.getElementById('main');

el.textContent = "New text";
el.innerHTML = "<b>Bold</b>";
el.classList.add('active');
el.classList.toggle('hidden');
el.setAttribute('data-id', '5');

const newDiv = document.createElement('div');
document.body.appendChild(newDiv);
el.remove();

// Events
el.addEventListener('click', (e) => {
    e.preventDefault();
    e.stopPropagation();
    console.log(e.target);
});
```

### Event Bubbling, Capturing & Delegation (Common Interview Topic)
```javascript
// Event delegation: attach one listener to a parent instead of many children
document.querySelector('.list').addEventListener('click', (e) => {
    if (e.target.matches('.list-item')) {
        console.log('Item clicked:', e.target.textContent);
    }
});
```
**Theory:** Events *capture* down from `window` to the target, then *bubble* back up. `addEventListener(type, fn, true)` listens during capture phase; default (`false`) listens during bubble phase. Delegation leverages bubbling for performance with dynamic lists (fewer listeners, works for elements added later).

---

## 8. Advanced Concepts

### Debounce & Throttle (Common Real-World/Interview Task)
```javascript
function debounce(fn, delay) {
    let timer;
    return (...args) => {
        clearTimeout(timer);
        timer = setTimeout(() => fn(...args), delay);
    };
}

function throttle(fn, limit) {
    let inThrottle;
    return (...args) => {
        if (!inThrottle) {
            fn(...args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```
**Use case:** Debounce for search-input API calls (wait until typing stops); throttle for scroll/resize handlers (limit execution rate).

### Currying & Memoization
```javascript
const curry = (fn) => (...args) =>
    args.length >= fn.length ? fn(...args) : (...more) => curry(fn)(...args, ...more);

function memoize(fn) {
    const cache = new Map();
    return (...args) => {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn(...args);
        cache.set(key, result);
        return result;
    };
}
```

### Design Patterns commonly asked
- **Module Pattern** — using closures/IIFE for encapsulation.
- **Singleton** — ensure only one instance exists.
- **Observer Pattern** — pub/sub, basis of event systems and frameworks like Vue's reactivity.
- **Factory Pattern** — function that creates objects without exposing instantiation logic.

---

## 9. Error Handling & Debugging

```javascript
try {
    JSON.parse("invalid json");
} catch (err) {
    console.error("Parse failed:", err.message);
} finally {
    console.log("Cleanup runs regardless");
}

class CustomError extends Error {
    constructor(message, code) {
        super(message);
        this.name = "CustomError";
        this.code = code;
    }
}
throw new CustomError("Something failed", 400);

// Global error handlers (browser)
window.onerror = (msg, url, line) => console.log(msg);
window.addEventListener('unhandledrejection', e => console.log(e.reason));
```

---

## 10. User-Side / Real Project Practices

- **Strict mode** (`'use strict'`) — catches silent errors, prevents accidental globals.
- **Modularize code** — small, single-responsibility functions/modules; avoid global namespace pollution.
- **Avoid memory leaks** — remove event listeners on component unmount, clear intervals/timeouts, avoid unbounded caches/closures holding large DOM references.
- **Immutable updates** — in frameworks like React, never mutate state directly; use spread/map to create new references so re-renders trigger correctly.
- **Linting/formatting** — ESLint + Prettier in real teams to enforce consistent code style and catch bugs early.
- **Testing** — unit tests (Jest/Vitest) for pure functions, mocking `fetch`/APIs in tests.

---

## Quick Revision Cheat-Sheet
| Concept | One-Line Recall |
|---|---|
| Closures | Function retains access to its lexical scope even after outer function returns |
| Event loop order | Sync code → microtasks (Promises) → macrotasks (setTimeout) |
| == vs === | == coerces types; === strict, no coercion |
| let/const vs var | let/const block-scoped + TDZ; var function-scoped + hoisted as undefined |
| Debounce vs Throttle | Debounce = wait for pause; Throttle = limit rate |
| Arrow vs regular fn `this` | Arrow inherits enclosing `this`; regular has its own |
