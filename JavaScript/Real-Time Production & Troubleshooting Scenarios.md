# JavaScript — Real-Time Production & Troubleshooting Scenarios

## Scenario 1: Memory leak causing browser tab to crash after extended use

- **Situation**: Users report the dashboard tab becomes sluggish and eventually crashes after being open for hours.
- **Investigation**: Used Chrome DevTools Memory tab, took heap snapshots over time, compared them to find growing object counts.
- **Root Cause**: A `setInterval` polling function was created on every component mount but never cleared on unmount, and each interval closure held a reference to stale component data, preventing garbage collection.
- **Fix**: Added proper cleanup (`clearInterval`) in the component's unmount lifecycle/`useEffect` cleanup function.
- **Prevention**: Code review checklist item: every `setInterval`/`setTimeout`/event listener must have a corresponding cleanup; add automated memory regression tests for long-running views.

## Scenario 2: Race condition causing stale data to overwrite fresh data

- **Situation**: A search-as-you-type feature occasionally shows results for a previous, already-abandoned query.
- **Investigation**: Logged request/response timestamps; found that a slower earlier request resolved *after* a faster later request.
- **Root Cause**: No request cancellation/sequencing — each keystroke fired a new API call, and responses weren't guaranteed to resolve in the order they were sent.
- **Fix**: Used `AbortController` to cancel in-flight requests when a new one starts, or tracked a request ID/sequence number and ignored responses that don't match the latest request.
- **Prevention**: Standardize a debounced-search + cancellation pattern as a shared utility/hook for all search inputs.

## Scenario 3: Production crash from an unhandled promise rejection

- **Situation**: An intermittent white-screen crash reported by a subset of users.
- **Investigation**: Checked error monitoring tool (Sentry); found an `UnhandledPromiseRejectionWarning` correlated with the crash timestamps.
- **Root Cause**: An async function call inside a React event handler wasn't wrapped in try/catch, and a network failure threw an unhandled rejection that broke rendering downstream.
- **Fix**: Added proper try/catch around all async operations in event handlers; added an error boundary component to prevent a single failure from crashing the whole app.
- **Prevention**: Lint rule requiring promise rejections to be handled; mandatory error boundaries around major app sections.

## Scenario 4: Checkout button double-charges customers

- **Situation**: Support team reports customers being charged twice for a single order.
- **Investigation**: Checked logs — found two POST requests to `/charge` within milliseconds of each other for the same order.
- **Root Cause**: The submit button wasn't disabled after the first click, and a slow network allowed a user (or an accidental double-click) to trigger the handler twice before the first request completed.
- **Fix**: Disabled the button immediately on click, added a loading state, and added server-side idempotency keys as a second layer of protection.
- **Prevention**: Standardize a "disable-on-submit" pattern for all payment/critical action buttons; enforce idempotency keys on the backend for financial operations.

## Scenario 5: Third-party script breaks the checkout flow after an update

- **Situation**: Checkout suddenly stops working after a third-party analytics script auto-updated.
- **Investigation**: Checked Console — new script version threw a JS error that halted execution of subsequent inline scripts on the page.
- **Root Cause**: Synchronous script tag loaded in `<head>` without error isolation; a JS error anywhere in that script's execution context blocked scripts queued after it.
- **Fix**: Loaded third-party scripts asynchronously, wrapped critical app logic to not depend on third-party script execution order, pinned the script version instead of auto-updating.
- **Prevention**: Policy: all third-party scripts load async/deferred and are isolated from critical business logic; monitor and pin versions.

## Scenario 6: Infinite re-render loop crashes a React page

- **Situation**: A specific page becomes unresponsive and the browser shows a "page unresponsive" warning.
- **Investigation**: React DevTools Profiler showed the same component re-rendering hundreds of times per second.
- **Root Cause**: A `useEffect` updated state that was also in its own dependency array without a proper guard condition, causing an infinite render loop.
- **Fix**: Corrected the dependency array and added a condition to only update state when the value actually changes.
- **Prevention**: ESLint `react-hooks/exhaustive-deps` rule enforced in CI; code review focus on effect dependency correctness.

## Scenario 7: Slow initial page load due to large JS bundle

- **Situation**: Time-to-interactive is very high on the main app entry point; users on slower connections bounce.
- **Investigation**: Ran a bundle analyzer; found a large charting library and an unused date library both bundled into the main chunk.
- **Root Cause**: No code-splitting — everything (including rarely-used admin features) was bundled into a single initial JS file.
- **Fix**: Introduced route-based code splitting (`React.lazy`/dynamic `import()`), moved the charting library to load only on pages that use it, removed the unused dependency.
- **Prevention**: Add bundle size budgets and analysis to CI; review new dependencies for size impact before merging.

## Scenario 8: Data corruption from concurrent tab edits

- **Situation**: A user editing the same document in two browser tabs loses changes from one tab.
- **Investigation**: Reproduced by opening two tabs, editing in both; found last-save-wins behavior overwriting concurrent edits silently.
- **Root Cause**: No optimistic concurrency control (no version check before saving) and no cross-tab awareness.
- **Fix**: Added a `version`/`updatedAt` check on save (reject/merge if the server version is newer than what the client last loaded); added a `BroadcastChannel`/`storage` event listener to warn users of edits happening in another tab.
- **Prevention**: Standardize optimistic concurrency checks for all collaborative-editing features going forward.
