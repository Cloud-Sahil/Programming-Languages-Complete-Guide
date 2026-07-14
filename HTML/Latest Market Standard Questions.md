# HTML — Latest Market Standard Questions (2026–2027)

## Modern Web Standards

**Q1. What are Web Components and why are they relevant today?**
A set of standards (Custom Elements, Shadow DOM, HTML Templates) allowing framework-agnostic reusable components that work natively in the browser without React/Vue/Angular.

```html
<template id="my-card">
  <style>.card{border:1px solid #ccc;}</style>
  <div class="card"><slot></slot></div>
</template>
```

**Q2. What is the Shadow DOM?**
An encapsulated DOM subtree attached to an element, isolating its styles/markup from the rest of the page — used heavily by Web Components and browser-native elements (e.g. `<video>` controls).

**Q3. What's the difference between `<picture>` and `srcset` on `<img>`?**
`srcset` lets the browser choose the best resolution of the *same* image. `<picture>` lets you serve *entirely different* images/formats based on conditions (art direction, format support like AVIF/WebP with fallback).

```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="...">
</picture>
```

**Q4. What are modern image formats you should know (2026 standard)?**
AVIF and WebP — significantly smaller file sizes than JPEG/PNG with comparable/better quality; use `<picture>` with fallback for older browser support.

**Q5. What is Core Web Vitals and which HTML choices affect it?**
Google's UX metrics: LCP (loading), INP (interaction responsiveness, replaced FID), CLS (visual stability). HTML impacts: image dimensions/lazy-loading affect LCP/CLS; script loading strategy affects INP.

## Accessibility & Compliance (increasingly enforced in 2026)

**Q6. What is WCAG and what level do most companies target?**
Web Content Accessibility Guidelines; most companies target **WCAG 2.1/2.2 Level AA** as the legal/industry baseline (used in ADA/EN 301 549 compliance).

**Q7. What's new in accessibility-related HTML attributes you should know?**
`inert` attribute (disables interaction/focus for a subtree, e.g. background content behind a modal), `popover` attribute (native popover/modal support without heavy JS), `<dialog>` element for native modals.

```html
<dialog id="modal" open>
  <p>Native modal content</p>
  <button onclick="document.getElementById('modal').close()">Close</button>
</dialog>
```

## Performance & Modern Loading Strategies

**Q8. What is the `fetchpriority` attribute?**
Hints to the browser which resources are most important to load first — e.g. `<img fetchpriority="high">` for the LCP hero image.

**Q9. What are resource hints and when do you use each?**
- `preconnect` — establish early connection to a domain you'll fetch from soon.
- `preload` — fetch a critical resource needed for this page ASAP.
- `prefetch` — fetch a resource likely needed for the *next* navigation.
- `dns-prefetch` — resolve DNS early for a third-party domain.

**Q10. How does Server-Side Rendering / streaming HTML relate to modern frameworks (Next.js, Remix, etc.)?**
Modern frameworks stream HTML progressively from the server (using `<template>`/`Suspense` boundaries) so users see content faster, then hydrate with JS — understanding raw HTML streaming/rendering fundamentals is now expected even in framework-heavy roles.

## Practical / Modern Engineering

**Q11. How do you handle Content Security Policy (CSP) considerations in your HTML?**
Avoid inline scripts/styles where possible (CSP often blocks `unsafe-inline`); use nonces/hashes for necessary inline scripts; understand `<meta http-equiv="Content-Security-Policy">` vs server header approach.

**Q12. What is the `loading="lazy"` limitation you should know?**
Shouldn't be used on above-the-fold/LCP images — it can delay the largest contentful paint; use it only for below-the-fold, offscreen content.

**Q13. How do you make a page work well with AI-based browser agents/crawlers (an emerging 2026 concern)?**
Use clean semantic HTML and structured data (schema.org/JSON-LD) so both search engines and AI agents parsing the page (for summarization, task automation, shopping agents, etc.) can reliably extract content and actions.

**Q14. What is the `<search>` element (newer HTML5 addition)?**
A semantic landmark for a search form/section, similar in spirit to `<nav>`/`<main>`, improving both accessibility tooling and document structure clarity.
