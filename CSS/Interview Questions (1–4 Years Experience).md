# CSS — Interview Questions (1–4 Years Experience)

## Conceptual

**Q1. Explain the CSS box model.**
Content → padding → border → margin. `box-sizing: border-box` includes padding+border within the specified width/height; `content-box` (default) adds them on top.

**Q2. Difference between `relative`, `absolute`, `fixed`, `sticky` positioning?**
See theory file — relative offsets from normal position (space preserved); absolute removes from flow, positions relative to nearest positioned ancestor; fixed relative to viewport; sticky toggles between relative and fixed based on scroll position.

**Q3. Difference between Flexbox and Grid — when do you use which?**
Flexbox: 1-dimensional layout (row or column) — good for navbars, button groups, aligning items along one axis. Grid: 2-dimensional layout (rows and columns together) — good for overall page layout, card grids, dashboards.

**Q4. What is specificity? How is it calculated?**
Determines which CSS rule wins when multiple rules target the same element. Calculated as (inline, IDs, classes/attributes/pseudo-classes, elements), compared in that order.

**Q5. What's the difference between `em` and `rem`?**
`em` is relative to the parent element's font size (compounds when nested); `rem` is relative to the root (`html`) font size (predictable, doesn't compound).

**Q6. What causes margin collapsing? How do you prevent it?**
Adjacent vertical margins between block elements collapse into the larger single margin. Prevent with padding/border on the parent, `overflow: hidden`/`display: flow-root` on parent, or flex/grid layout (which doesn't have margin collapse).

**Q7. What is the difference between `visibility: hidden` and `display: none`?**
`display: none` removes the element from layout entirely (no space taken). `visibility: hidden` hides it visually but preserves its layout space.

**Q8. What are pseudo-classes vs pseudo-elements?**
Pseudo-class (`:hover`, `:nth-child`) targets an element in a particular state/position. Pseudo-element (`::before`, `::after`) targets a virtual sub-part of an element, often used to insert generated content.

## Practical

**Q9. Center a div both horizontally and vertically.**
```css
/* Flexbox approach */
.parent { display: flex; justify-content: center; align-items: center; height: 100vh; }

/* Grid approach */
.parent { display: grid; place-items: center; height: 100vh; }

/* Absolute positioning approach */
.child {
  position: absolute; top: 50%; left: 50%;
  transform: translate(-50%, -50%);
}
```

**Q10. Build a responsive 3-column layout that collapses to 1 column on mobile.**
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}
@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
```

**Q11. Create a sticky header that stays on top while scrolling.**
```css
header {
  position: sticky;
  top: 0;
  z-index: 100;
}
```

**Q12. How would you implement a CSS-only tooltip?**
```css
.tooltip { position: relative; }
.tooltip::after {
  content: attr(data-tooltip);
  position: absolute;
  bottom: 100%; left: 50%;
  transform: translateX(-50%);
  opacity: 0;
  transition: opacity 0.2s;
}
.tooltip:hover::after { opacity: 1; }
```

**Q13. How do you make text truncate with an ellipsis after one line?**
```css
.truncate {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

**Q14. How would you implement dark mode?**
Use CSS custom properties for theme colors, toggle a class/attribute on `<html>`, or respect `prefers-color-scheme` media query as default.
```css
:root { --bg: white; --text: black; }
[data-theme="dark"] { --bg: black; --text: white; }
body { background: var(--bg); color: var(--text); }
```

## Scenario-Based

**Q15. A flex item won't shrink below its content size even though the container is narrower. Why, and how do you fix it?**
Default `min-width: auto` on flex items prevents shrinking below content size. Fix: add `min-width: 0` (or `overflow: hidden`) on the item.

**Q16. How would you debug a `z-index` that isn't working?**
Check the element has `position` set (not static), and check ancestors for properties (`transform`, `opacity`, `filter`) that create a new stacking context and trap the z-index locally.

**Q17. How do you make a website accessible for both LTR and RTL languages using CSS?**
Use logical properties (`margin-inline-start` instead of `margin-left`, `padding-inline-end` instead of `padding-right`) which automatically adapt to text direction.
