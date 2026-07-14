# HTML — Real-Time Production & Troubleshooting Scenarios

## Scenario 1: SEO traffic drops sharply after a redesign

- **Situation**: Organic traffic dropped 40% after a marketing site redesign went live.
- **Investigation**: Compared old vs new HTML source; checked Google Search Console for crawl errors.
- **Root Cause**: New template used `<div class="heading">` styled to look like headings instead of real `<h1>`–`<h3>` tags; also missing meta descriptions on key pages.
- **Fix**: Restored proper semantic heading hierarchy; re-added unique meta titles/descriptions per page.
- **Prevention**: Add semantic HTML + meta-tag checks to the pre-launch QA checklist; use Lighthouse SEO audit in CI.

## Scenario 2: Accessibility lawsuit risk flagged by legal/compliance

- **Situation**: Company received an accessibility complaint; audit needed urgently.
- **Investigation**: Ran automated a11y scan (axe, Lighthouse) plus manual keyboard/screen-reader testing.
- **Root Cause**: Form inputs without labels, images without alt text, custom dropdown built with `div`s with no keyboard support or ARIA roles.
- **Fix**: Added proper `<label>` associations, meaningful `alt` text, replaced custom widgets with native `<select>`/`<button>` where possible, added ARIA roles/states where a native element wasn't feasible.
- **Prevention**: Add automated a11y testing (axe-core) to CI pipeline; accessibility review as part of design/dev handoff.

## Scenario 3: Mobile users report broken layout / zoomed-in page

- **Situation**: Site looks fine on desktop but broken/zoomed on mobile devices.
- **Investigation**: Inspected `<head>` — found missing viewport meta tag.
- **Root Cause**: `<meta name="viewport">` was missing entirely, so mobile browsers rendered the page at desktop width and scaled it down.
- **Fix**: Added `<meta name="viewport" content="width=device-width, initial-scale=1.0">`.
- **Prevention**: Include viewport meta tag in the base HTML template/boilerplate used for all new pages.

## Scenario 4: Slow page load flagged in Core Web Vitals report

- **Situation**: Google Search Console flags poor LCP (Largest Contentful Paint) and CLS (Cumulative Layout Shift) scores.
- **Investigation**: Ran Lighthouse audit; found large above-the-fold hero image with no `width`/`height`, and multiple blocking `<script>` tags in `<head>`.
- **Root Cause**: Unoptimized hero image causing slow LCP; missing dimensions causing layout shift as image loads; render-blocking scripts delaying first paint.
- **Fix**: Compressed and used `srcset` for the hero image; added explicit `width`/`height`; moved non-critical scripts to `defer`; added `preload` for the hero image.
- **Prevention**: Add Lighthouse CI checks with performance budgets to the deployment pipeline.

## Scenario 5: Form data not reaching the backend in production only

- **Situation**: Signup form works in staging but fails silently in production.
- **Investigation**: Checked Network tab — found the POST request wasn't firing at all.
- **Root Cause**: A build/minification step in production stripped an inline `onsubmit` handler that wasn't properly escaped, or the submit button had `type="button"` instead of `type="submit"` and relied on a JS handler that failed to attach due to a script loading order issue.
- **Fix**: Used a proper `type="submit"` button and an `addEventListener` based handler instead of inline handlers; fixed script load order.
- **Prevention**: Avoid inline event handlers; test the production build (not just dev server) before release; add e2e test coverage for critical forms.

## Scenario 6: Third-party widget breaks the whole page

- **Situation**: A marketing team embeds a third-party chat widget `<script>`/`<iframe>`; page starts crashing/freezing for some users.
- **Investigation**: Checked Console — third-party script threw errors and blocked the main thread; iframe had no `sandbox` attribute.
- **Root Cause**: Synchronous, render-blocking third-party script with no error isolation; no sandboxing on the iframe increased both performance and security risk.
- **Fix**: Loaded the widget script with `async`/`defer`, wrapped in error boundaries where possible, added `sandbox` attributes to the iframe with only necessary permissions.
- **Prevention**: Establish a policy: all third-party embeds must load asynchronously and be sandboxed; review new embeds before adding to production.

## Scenario 7: Duplicate content / canonical issues across staging and prod

- **Situation**: Search engines indexing the staging subdomain, causing duplicate content penalties.
- **Investigation**: Checked `robots.txt` and meta tags on staging.
- **Root Cause**: Staging environment had no `noindex` meta tag or `robots.txt` disallow rule, so it got crawled and indexed alongside production.
- **Fix**: Added `<meta name="robots" content="noindex, nofollow">` to all non-production environments; added canonical tags pointing to the production URL.
- **Prevention**: Bake `noindex` into the staging deployment config by default, only removed for production builds.

## Scenario 8: Broken layout in older browser used by a large client

- **Situation**: A key enterprise client reports the internal dashboard is broken in their locked-down older browser version.
- **Investigation**: Checked which HTML5/CSS features were used; tested in the specific browser version via BrowserStack.
- **Root Cause**: Used a newer input type (`<input type="date">` with custom features) unsupported in the older browser, causing form fields to render as plain text inputs with no fallback handling.
- **Fix**: Added a JS-based polyfill/fallback datepicker for unsupported browsers; used feature detection instead of assuming support.
- **Prevention**: Define a supported browser matrix with the client/stakeholders; test critical flows against that matrix before release.
