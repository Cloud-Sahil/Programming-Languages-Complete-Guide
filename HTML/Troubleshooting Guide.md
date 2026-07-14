# HTML — Troubleshooting Guide (Common Issues & Fixes)

## 1. Layout Rendering Issues

**Symptom**: Page layout looks broken / elements overlapping
- Cause: missing `<!DOCTYPE html>` (triggers quirks mode), unclosed tags, missing viewport meta tag.
- Fix: always start with `<!DOCTYPE html>`; validate HTML (W3C validator); add `<meta name="viewport" content="width=device-width, initial-scale=1.0">`.

**Symptom**: Images not displaying
- Cause: wrong path (case-sensitive on Linux servers), missing file, wrong `src`, CORS blocking cross-origin image, or slow/failed load with no fallback.
- Fix: verify path/case, check browser dev tools Network tab for 404, use relative vs absolute paths correctly, add `onerror` fallback if needed.

**Symptom**: Fonts/icons not showing (boxes/question marks instead)
- Cause: missing charset declaration, wrong font file path, font not loaded before render (FOUT/FOIT).
- Fix: add `<meta charset="UTF-8">`; verify font `@font-face` src paths; use `font-display: swap`.

## 2. Form Issues

**Symptom**: Form submits but server receives no data / wrong data
- Cause: missing `name` attribute on inputs (only `id` won't submit), wrong `method`, nested forms (invalid), disabled inputs not submitted.
- Fix: ensure every field has a unique `name`; check `method`/`action`; never nest `<form>` tags; remove `disabled` if the field should submit (use `readonly` instead if needed).

**Symptom**: Browser validation not triggering (e.g. `required` ignored)
- Cause: `novalidate` attribute present on form, or validation bypassed via JS `submit()` call instead of user-triggered submit.
- Fix: remove `novalidate` unless intentional; trigger `form.reportValidity()` if submitting programmatically.

**Symptom**: Enter key doesn't submit the form
- Cause: no `<button type="submit">` or `<input type="submit">` present; using `type="button"` for the only button.
- Fix: ensure at least one true submit control exists in the form.

## 3. Accessibility Issues

**Symptom**: Screen reader skips or misreads content
- Cause: using `div`/`span` instead of semantic tags, missing `alt` text, unlabeled form inputs, improper heading order (h1 → h3 skipping h2).
- Fix: use semantic HTML, add meaningful `alt`, use `<label for>`, maintain logical heading hierarchy.

**Symptom**: Keyboard users can't navigate interactive elements
- Cause: using `div`/`span` with `onclick` instead of `<button>`/`<a>`, missing `tabindex`.
- Fix: use native interactive elements (`button`, `a`, `input`) which are keyboard-accessible by default; add `tabindex="0"` only when a custom widget genuinely needs it, plus proper `role`/`aria-*`.

## 4. Cross-Browser Issues

**Symptom**: Works in Chrome, breaks in Safari/Firefox
- Cause: use of non-standard/experimental HTML features, vendor-specific quirks in form controls (`<input type="date">` rendering differs), inconsistent default styling.
- Fix: check caniuse.com before using newer features; use a CSS reset/normalize; test across browsers (BrowserStack or real devices); provide fallbacks for unsupported input types.

**Symptom**: Special characters showing as garbled text (e.g. "â€™" instead of an apostrophe)
- Cause: missing or mismatched `<meta charset>`, file saved in wrong encoding.
- Fix: always declare `<meta charset="UTF-8">` as the first element in `<head>`; save files as UTF-8.

## 5. Performance Issues

**Symptom**: Page loads slowly, high "Largest Contentful Paint"
- Cause: large unoptimized images, render-blocking scripts/styles in `<head>`, too many synchronous third-party scripts, no lazy loading.
- Fix: use `loading="lazy"`, `srcset` for responsive images, `defer`/`async` on non-critical scripts, minimize third-party embeds, use resource hints (`preconnect`/`preload`).

**Symptom**: Content shifts around while loading (bad CLS — Cumulative Layout Shift)
- Cause: images/iframes/ads without explicit width/height reserving space, web fonts causing reflow.
- Fix: always set `width`/`height` (or `aspect-ratio` via CSS) on media elements; use `font-display: swap` with size-matched fallback fonts.

## 6. SEO / Meta Issues

**Symptom**: Page not indexed properly / poor search ranking
- Cause: missing/duplicate `<title>`, missing meta description, multiple `h1` tags or none, missing canonical tag on duplicate content, blocked by `robots.txt`/`noindex` unintentionally.
- Fix: unique title/description per page, single meaningful `h1`, canonical tags for duplicate/paginated content, verify `robots.txt` and meta robots tags.

## 7. Nesting / Validation Errors

**Symptom**: Unexpected rendering, browser "fixes" your HTML silently
- Cause: invalid nesting (`<p>` inside `<p>`, block element inside inline element like `<div>` inside `<span>`, `<a>` wrapping block elements incorrectly in older HTML), unclosed tags.
- Fix: run through the W3C Markup Validator; understand block vs inline content models; close all tags properly (even ones the browser tolerates unclosed).

## 8. Quick Diagnostic Checklist
1. Open browser DevTools → Elements tab: is the DOM structured as expected?
2. Check Console for errors/warnings.
3. Check Network tab for failed resource loads (404/CORS).
4. Validate HTML via W3C validator.
5. Test with screen reader / keyboard-only navigation for a11y bugs.
6. Test across at least 2 browsers and one mobile viewport.
