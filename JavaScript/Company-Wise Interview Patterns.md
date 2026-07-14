# JavaScript — Company-Wise Interview Patterns

> Reflects general patterns reported for each company category, not confirmed question banks.

## Service-Based Companies (TCS, Infosys, Wipro, Cognizant, Accenture)
- **Focus**: Core fundamentals — closures, hoisting, `this`, ES6 features, basic DOM manipulation.
- **Style**: MCQ/written test + a technical round with theory-heavy questions.
- **Typical asks**: Explain closures with an example, var vs let vs const, event loop basics, difference between `==` and `===`.

## Product-Based Companies (Amazon, Flipkart, Microsoft, Google, Uber, Swiggy)
- **Focus**: Deep JS fundamentals + DSA-style coding in JS + system design overlap for senior roles.
- **Style**: Live coding rounds (LeetCode-style problems solved in JS), plus a dedicated JS fundamentals/frontend round.
- **Typical asks**: Implement debounce/throttle/curry from scratch, explain event loop with a tricky code-output question, polyfills for array methods, discuss performance optimization of a given component/app.

## Fintech (Paytm, Razorpay, PhonePe, banks' digital teams)
- **Focus**: Correctness under concurrency, idempotency in async flows, security awareness (XSS, CSRF basics).
- **Typical asks**: How do you prevent double form submission/double charge, explain race conditions and how you'd avoid them, basic security questions relevant to JS (XSS prevention, safe use of `innerHTML`, CSP).

## Frontend-Specialist / Design-Heavy Companies
- **Focus**: Deep framework knowledge (React/Vue) layered on JS fundamentals, performance profiling.
- **Typical asks**: Explain React's reconciliation/rendering, debug a memory leak or a re-render performance issue, discuss code-splitting and bundle optimization.

## Startups
- **Focus**: Practical, full-stack-leaning — can you ship a working feature quickly, reasonable code quality.
- **Typical asks**: Build a small feature end-to-end (frontend + calling an API), general problem-solving discussion, take-home coding assignment.

## General Pattern Across All Companies
1. **Round 1** — Online assessment: MCQs + coding problems (often DSA in JS).
2. **Round 2** — Live coding: JS fundamentals questions + a coding problem, sometimes framework-specific.
3. **Round 3** — Deeper technical/system design (for 2+ years experience): architecture, performance, real-world debugging scenario.
4. **Round 4** — Managerial round.
5. **Round 5** — HR round.

## What Interviewers Commonly Evaluate
- Solid grasp of closures, `this`, event loop, and async patterns (not just syntax memorization)
- Ability to write clean, working code under time pressure
- Debugging instincts — can you reason through a "what's the output" tricky snippet
- Awareness of performance and real-world production concerns (not just textbook answers)
- Communication — explaining trade-offs while coding
