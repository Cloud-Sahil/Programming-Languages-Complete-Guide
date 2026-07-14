# CSS — Latest Market Standard Questions (2026–2027)

## Modern Layout & Selectors

**Q1. What is the `:has()` selector and why is it a big deal?**
The long-awaited "parent selector" — lets you style an element based on its descendants.
```css
.card:has(img) { border: 2px solid gold; }
form:has(input:invalid) { border-color: red; }
```
Enables patterns previously requiring JavaScript.

**Q2. What are Container Queries and how are they different from Media Queries?**
Media queries respond to the *viewport* size; container queries respond to a *parent container's* size — enabling truly reusable, context-aware components (a card can look different in a sidebar vs a full-width section).
```css
.card-wrapper { container-type: inline-size; }
@container (min-width: 400px) { .card { flex-direction: row; } }
```

**Q3. What are CSS Nesting rules (native, no preprocessor needed)?**
Modern browsers support native nesting similar to SASS:
```css
.card {
  padding: 16px;
  & .title { font-weight: bold; }
  &:hover { box-shadow: 0 0 8px #ccc; }
}
```

**Q4. What is `@layer` (Cascade Layers)?**
Lets you explicitly control cascade order between groups of styles (e.g. reset, base, components, utilities, overrides) regardless of specificity or source order — solves long-standing "utility class vs component class" conflicts.
```css
@layer reset, base, components, utilities;
@layer base { body { margin: 0; } }
```

**Q5. What is `:where()` vs `:is()`?**
Both group selectors, but `:where()` always has zero specificity while `:is()` takes the specificity of its most specific argument — `:where()` is preferred in design systems/libraries to keep specificity low and easily overridable.

## Modern Layout Techniques

**Q6. What is `subgrid` and when would you use it?**
Lets a nested grid item align its rows/columns to the parent grid's tracks — useful for aligning card contents (titles, footers) across a grid of cards regardless of content length.
```css
.card { display: grid; grid-template-rows: subgrid; grid-row: span 3; }
```

**Q7. What are the modern viewport units `svh`, `lvh`, `dvh`?**
Solve the classic mobile browser "100vh includes the address bar" problem:
- `svh` — small viewport height (address bar visible)
- `lvh` — large viewport height (address bar hidden)
- `dvh` — dynamic, adjusts live as the browser UI shows/hides

**Q8. What is `aspect-ratio` and what problem does it solve?**
Sets a fixed width-to-height ratio without needing padding-hack tricks (the old "padding-top: 56.25%" trick for 16:9 video embeds).
```css
.video-embed { aspect-ratio: 16 / 9; }
```

## Performance & Modern Practices

**Q9. What is `content-visibility` and how does it improve performance?**
Skips rendering work for offscreen content until it's needed, dramatically speeding up initial render for long pages.
```css
.section { content-visibility: auto; contain-intrinsic-size: 500px; }
```

**Q10. How do design tokens and CSS custom properties relate to design systems in 2026?**
Design tokens (color, spacing, typography scales) are commonly implemented as CSS custom properties, often generated from a single source (Figma/Style Dictionary) and consumed across web/mobile — ensuring visual consistency and easy theming (including dark mode).

**Q11. What is `color-mix()`?**
Native CSS function to blend two colors without a preprocessor:
```css
.button { background: color-mix(in srgb, blue 70%, white); }
```

**Q12. How does Tailwind CSS (or utility-first CSS) compare to traditional CSS architecture in interviews today?**
Be ready to discuss trade-offs: utility-first speeds up development and keeps specificity flat/low, but can bloat HTML markup and reduce semantic clarity; many 2026 teams combine utility classes with component abstraction (React/Vue components) to get both benefits.

**Q13. What accessibility-related CSS features are commonly tested now?**
`prefers-reduced-motion` (respecting users who want less animation), `prefers-color-scheme`, ensuring focus states aren't removed (`outline: none` without a replacement is a common accessibility anti-pattern flagged in reviews).
```css
@media (prefers-reduced-motion: reduce) {
  * { animation: none !important; transition: none !important; }
}
```
