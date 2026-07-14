# CSS — Real-Time Production & Troubleshooting Scenarios

## Scenario 1: Layout completely breaks only in production build

- **Situation**: Site looks fine locally but layout is broken after production deploy.
- **Investigation**: Compared local vs production CSS output; checked build/minification config.
- **Root Cause**: CSS purge tool (e.g. PurgeCSS/Tailwind JIT) removed classes it thought were unused because they were added dynamically via JS string concatenation (not detected by static analysis).
- **Fix**: Added a safelist for dynamically generated class names; refactored to use full static class names instead of concatenation.
- **Prevention**: Document dynamic class usage patterns; add safelist config to CI; visually test production build before every release.

## Scenario 2: Random layout shift causing poor CLS score

- **Situation**: Core Web Vitals report flags high Cumulative Layout Shift on product pages.
- **Investigation**: Recorded a Performance trace in DevTools; saw layout shifting when web fonts loaded.
- **Root Cause**: Fallback system font had different metrics (width/height) than the custom web font, causing text reflow once the custom font loaded.
- **Fix**: Used `font-display: swap` with a closely metric-matched fallback font (using `size-adjust`/`ascent-override` font descriptors), and reserved space with consistent line-height.
- **Prevention**: Add font-loading strategy review to the performance checklist; monitor CLS via RUM (Real User Monitoring) tools.

## Scenario 3: z-index war between teams' components

- **Situation**: A modal from Team A is appearing behind a dropdown from Team B, despite a much higher z-index value.
- **Investigation**: Inspected the DOM tree and ancestor styles.
- **Root Cause**: The modal's parent container had `transform: translateY(0)` applied for an unrelated animation, creating a new stacking context that capped the modal's effective z-index within its parent, regardless of the large value set directly on the modal.
- **Fix**: Rendered the modal via a portal at the document body level (common in React/Vue) to escape the constrained stacking context, rather than trying to out-number z-index values.
- **Prevention**: Establish a documented z-index scale/convention across teams; render overlays (modals, toasts, dropdowns) via portals by default.

## Scenario 4: Site unusable on a client's older Safari version

- **Situation**: Enterprise client reports broken layout on Safari 14 (older macOS).
- **Investigation**: Tested via BrowserStack; found `gap` property not applying inside flex containers (older Safari support gap for grid, not flex).
- **Root Cause**: Used `gap` in a flex layout without a fallback, relying on browser support that didn't extend to the client's Safari version.
- **Fix**: Added margin-based spacing as a fallback using `@supports not (gap: 1rem)`.
- **Prevention**: Define and test against the actual supported browser matrix for enterprise clients; use `@supports` feature queries for progressive enhancement.

## Scenario 5: CSS bundle size ballooning, hurting load time

- **Situation**: CSS bundle grew to 800KB+, page load time degrading.
- **Investigation**: Analyzed bundle with a CSS coverage tool in DevTools; found ~70% of shipped CSS unused on any given page.
- **Root Cause**: Global stylesheet accumulated styles for every page/feature over years with no cleanup process; no code-splitting per route.
- **Fix**: Introduced CSS purging tied to actual usage, split styles per route/component, removed dead code identified via coverage tooling.
- **Prevention**: Add bundle size budgets to CI; run CSS coverage audits periodically; adopt component-scoped CSS (CSS Modules) going forward to prevent unbounded global growth.

## Scenario 6: Dark mode toggle causes flash of wrong theme (FOUC)

- **Situation**: Users briefly see light mode flash before dark mode applies on page load.
- **Investigation**: Checked how theme was applied — found it was set via JS after page render, reading from localStorage.
- **Root Cause**: Theme class was applied after the CSSOM/DOM painted, causing a visible flash before JS executed.
- **Fix**: Added a small inline blocking script in `<head>` (before CSS loads) that reads the stored theme preference and sets the class/attribute immediately, and used CSS custom properties keyed off that attribute (`[data-theme="dark"] { --bg: #000; }`).
- **Prevention**: Document the theme-flash prevention pattern for any future theming work; consider `prefers-color-scheme` media query as the default with override support.

## Scenario 7: Print stylesheet missing, broken PDF export

- **Situation**: Users report the printed/PDF-exported version of an invoice page looks broken (nav bars, buttons showing up).
- **Investigation**: No `@media print` rules existed.
- **Root Cause**: No dedicated print stylesheet; screen-only UI elements were being included in print output.
- **Fix**: Added `@media print { .no-print { display: none; } }` and adjusted layout/colors for print (removing backgrounds, adjusting margins).
- **Prevention**: Include print stylesheet review for any page marked as printable/exportable in requirements.

## Scenario 8: Accessibility audit fails on color contrast in production theme

- **Situation**: Automated accessibility audit flags low contrast text across the site.
- **Investigation**: Ran axe/Lighthouse; found gray-on-white text combinations below WCAG AA contrast ratio (4.5:1).
- **Root Cause**: Design system tokens (CSS custom properties) for muted text were chosen for aesthetics without contrast validation.
- **Fix**: Adjusted the affected color tokens to meet contrast ratios, verified with a contrast checker, and updated tokens centrally so all components using them were fixed at once.
- **Prevention**: Validate all design tokens against WCAG contrast requirements before adding to the design system; add automated contrast checks to CI.
