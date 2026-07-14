# CSS — Troubleshooting Guide (Common Issues & Fixes)

## 1. Specificity & Cascade Issues

**Symptom**: "My CSS isn't applying / gets overridden"
- Cause: another rule has higher specificity, appears later in cascade order, or `!important` is used elsewhere.
- Fix: use browser DevTools "Computed" and "Styles" panel to see which rule wins (strikethrough = overridden); increase specificity thoughtfully or restructure selectors; avoid `!important` except as an absolute last resort.

**Symptom**: Styles work in isolation but break when combined with a library/framework (Bootstrap, MUI)
- Cause: framework's default styles have higher specificity or load order conflict.
- Fix: check load order (your CSS should generally load after the framework's), use scoped/module CSS, or increase specificity minimally.

## 2. Box Model / Sizing Issues

**Symptom**: Element is wider/taller than expected, breaking layout
- Cause: `box-sizing: content-box` (default) adds padding/border on top of the set width.
- Fix: apply `box-sizing: border-box` globally.

**Symptom**: Unexpected gap/margin around an element
- Cause: **margin collapsing** — adjacent vertical margins between block elements collapse into the larger one.
- Fix: understand margin collapsing rules; use padding on parent, or `display: flow-root`/`overflow: hidden` on parent to prevent collapse, or use flex/grid gap instead of margins.

## 3. Flexbox / Grid Issues

**Symptom**: `justify-content: center` not centering items
- Cause: container isn't `display: flex`, or the main axis is the wrong direction (justify-content works on main axis, align-items on cross axis).
- Fix: verify `display: flex` is set; swap `justify-content`/`align-items` if direction is `column`.

**Symptom**: Flex items not shrinking/growing as expected
- Cause: `flex-shrink`/`flex-grow`/`flex-basis` defaults not overridden, or `min-width: auto` default preventing shrink below content size.
- Fix: set `flex: 1 1 0` explicitly; add `min-width: 0` (or `min-height: 0` for column) to allow shrinking below content size — extremely common real-world bug with text/images overflowing flex children.

**Symptom**: Grid items overflowing their tracks
- Cause: content (like long text or images) doesn't respect `1fr` track sizing.
- Fix: add `min-width: 0` to grid items; use `minmax()` in track definitions.

## 4. Position & z-index Issues

**Symptom**: `z-index` not working even with a high value
- Cause: element isn't positioned (`position` is `static`), or a parent creates its own stacking context (via `opacity`, `transform`, `filter`) that traps the z-index within it.
- Fix: set `position: relative/absolute/fixed/sticky` on the element; check ancestor elements for properties creating unintended stacking contexts.

**Symptom**: `position: sticky` not working
- Cause: an ancestor has `overflow: hidden/auto/scroll`, or missing a defined `top`/`bottom` value, or the container doesn't have enough height for sticking to be visible.
- Fix: remove/adjust overflow on ancestors; ensure `top` (or relevant offset) is set; verify container is tall enough.

## 5. Responsive Design Issues

**Symptom**: Layout breaks at specific screen widths
- Cause: missing/incorrect media query breakpoints, fixed `px` widths instead of relative units, missing viewport meta tag.
- Fix: use relative units (`%`, `rem`, `fr`, `vw`), test with DevTools responsive mode across common breakpoints, verify `<meta viewport>` exists.

**Symptom**: Horizontal scrollbar appears unexpectedly on mobile
- Cause: an element wider than viewport (often due to fixed width, unaccounted padding/margin, or an image without `max-width: 100%`).
- Fix: add `img, video { max-width: 100%; height: auto; }`; audit fixed-width elements; use `overflow-x: hidden` only as a last-resort band-aid (better to find and fix the actual overflowing element).

## 6. Animation / Transition Issues

**Symptom**: Transition not animating
- Cause: transitioning a property that can't be animated (e.g. `display`), or the property change happens in the same frame the transition is applied (no initial state to transition from), or `transition` not defined on the correct selector/state.
- Fix: animate `opacity`/`visibility` instead of `display`; ensure the initial state is rendered before triggering the change (may need a frame delay via JS); double-check the `transition` property is on the base state, not just `:hover`.

**Symptom**: Animation is janky / low frame rate
- Cause: animating layout-triggering properties (`width`, `top`, `left`, `margin`) instead of GPU-accelerated ones.
- Fix: use `transform` (`translate`, `scale`) and `opacity` instead; add `will-change` sparingly for heavy animations.

## 7. Font / Text Issues

**Symptom**: Custom font not displaying (falls back to default)
- Cause: wrong `@font-face` path, unsupported font format for the browser, font file not deployed to production, CORS blocking cross-origin font load.
- Fix: verify file path and deployment, include multiple formats (`woff2`, `woff`), check CORS headers if loading fonts from a CDN.

**Symptom**: Text overflowing its container
- Cause: no `overflow-wrap`/`word-break`, fixed-width container with long unbroken text (like a URL).
- Fix: `overflow-wrap: break-word;` or `word-break: break-all;` as needed; use `text-overflow: ellipsis` with `overflow: hidden; white-space: nowrap;` for truncation.

## 8. Cross-Browser Issues

**Symptom**: Works in Chrome, broken in Safari
- Cause: Safari has stricter/different support for some flexbox/grid edge cases, or missing vendor prefixes for newer features.
- Fix: check caniuse.com, add vendor prefixes (or use Autoprefixer in the build pipeline), test in real Safari/WebKit, not just Chrome DevTools emulation.

## 9. Quick Diagnostic Checklist
1. Open DevTools → inspect the element → check "Computed" styles for the actual resolved value.
2. Check for overridden (strikethrough) rules in "Styles" panel.
3. Toggle `box-sizing`, check box model diagram in DevTools.
4. Check for unexpected stacking contexts (`position`, `opacity`, `transform` on ancestors).
5. Test responsiveness across real breakpoints, not just resizing the window.
6. Validate CSS (no typos, missing semicolons/braces) using a linter.
