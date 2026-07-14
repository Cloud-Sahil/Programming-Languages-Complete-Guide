# JavaScript — Latest Market Standard Questions (2026–2027)

## Modern JavaScript Engineering

**Q1. What are the differences between CommonJS and ES Modules, and why does it matter today?**
CommonJS (`require`/`module.exports`) is synchronous, Node's traditional module system. ES Modules (`import`/`export`) are the standard now, support static analysis (tree-shaking), async loading, and work natively in both browsers and modern Node — most 2026 projects default to ESM.

**Q2. What is tree-shaking and how does your code need to be written to support it?**
Bundlers eliminate unused exports from the final bundle — requires ES Module syntax (static `import`/`export`, not dynamic `require`), and avoiding side-effects in module top-level code.

**Q3. Explain how modern frameworks use fine-grained reactivity (Signals) vs Virtual DOM.**
Virtual DOM (React's traditional model): re-renders a component tree and diffs against the previous version. Signals (used in Solid, Vue 3's reactivity, Angular Signals, and increasingly React via compiler-based optimizations): track exact dependencies and update only the specific DOM nodes affected, avoiding unnecessary re-renders/diffing overhead.

**Q4. What is the difference between Server Components and Client Components (React Server Components model)?**
Server Components render on the server, ship zero JS to the client for that component, can directly access backend resources (DB, filesystem). Client Components run in the browser, support interactivity/state/effects, hydrate on the client.

**Q5. What are Web Workers vs Worker Threads (Node) used for in 2026 apps?**
Offloading CPU-intensive work (image processing, large data parsing, AI model inference) off the main thread to keep the UI responsive; Worker Threads is the Node.js equivalent for backend CPU-bound tasks.

## Performance & Modern Patterns

**Q6. What is `AbortController` and why is it considered essential now?**
Standard API to cancel in-flight fetch requests or other async operations — critical for avoiding race conditions in search-as-you-type, tab navigation cancelling stale requests, and cleanup in component unmounts.
```js
const controller = new AbortController();
fetch(url, { signal: controller.signal });
controller.abort();
```

**Q7. What are Signals (as a general JS pattern, not tied to one framework)?**
A reactive primitive that holds a value and notifies subscribers precisely when it changes, enabling fine-grained updates without a virtual DOM diff — increasingly common across frontend frameworks in 2025-2026.

**Q8. What is `structuredClone()` and why replace `JSON.parse(JSON.stringify())` with it?**
Native deep-clone API supporting more data types (Dates, Maps, Sets, circular references) that `JSON.stringify` can't handle correctly; standard, built-in, no library needed.

**Q9. What's the role of Edge Functions / Edge Runtime in modern JS deployment?**
JS/TS functions deployed to CDN edge locations (Cloudflare Workers, Vercel Edge Functions) for lower latency — runs a restricted JS runtime (no full Node APIs), important to know the runtime constraints when writing code targeting the edge.

**Q10. What are the trade-offs of using Bun/Deno vs Node.js in 2026?**
Bun/Deno offer faster startup, built-in TypeScript support, modern APIs by default; Node still has the largest ecosystem/maturity — discuss choosing based on project needs (ecosystem compatibility vs performance/DX).

## AI-Adjacent / Emerging

**Q11. How would you stream an LLM API response into a UI in real time?**
Use the Streams API / `ReadableStream` from `fetch`, read chunks incrementally, update UI state progressively (common pattern for chat interfaces / AI product UIs).
```js
const response = await fetch(url);
const reader = response.body.getReader();
while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  // process chunk
}
```

**Q12. What is WebAssembly (Wasm) and when would you use it alongside JavaScript?**
A binary instruction format allowing near-native performance code (compiled from Rust/C++/Go) to run in the browser — used for compute-heavy tasks (image/video processing, ML inference, games) where pure JS would be too slow.

## Type Safety & Tooling

**Q13. Why has TypeScript become close to a default expectation in 2026 job postings?**
Reduces runtime type errors, improves refactor safety and IDE tooling/autocomplete, aids large-team collaboration by making contracts explicit (interfaces, generics).

**Q14. What is a linter/formatter pipeline expected in a modern JS project?**
ESLint (code quality/bugs) + Prettier (formatting) + TypeScript (type checking) + pre-commit hooks (Husky/lint-staged) — standard baseline in most professional 2026 codebases.

**Q15. What's the difference between unit, integration, and e2e testing in a JS context, and what tools are standard now?**
Unit: isolated function/component logic (Vitest/Jest). Integration: multiple units together (Testing Library). E2E: full user flow in a real browser (Playwright, increasingly preferred over Cypress for speed and multi-browser support in 2026).
