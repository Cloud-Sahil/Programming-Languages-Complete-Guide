# CSS — Zero to Hero: Complete Study Guide

## Table of Contents
1. Theory & Fundamentals
2. Basics (Beginner)
3. Box Model & Layout Basics
4. Flexbox
5. Grid
6. Positioning & Stacking
7. Responsive Design
8. Advanced CSS (Animations, Variables, Preprocessors)
9. CSS Architecture & Real Project Practices

---

## 1. Theory & Fundamentals

### What is CSS?
CSS (Cascading Style Sheets) describes how HTML elements should be **displayed** — layout, color, spacing, typography, responsiveness, animation.

### The "Cascade" (Why it's called Cascading — common Q)
When multiple rules apply to the same element, CSS resolves conflicts using, in order:
1. **Importance** (`!important` beats everything, avoid overusing it).
2. **Specificity** (ID > Class/Attribute/Pseudo-class > Element/Pseudo-element).
3. **Source order** (later rules override earlier ones if specificity is equal).

### Specificity Calculation (Frequently Asked)
```
Inline style        = 1000
ID selector          = 100
Class/Attr/Pseudo    = 10
Element/Pseudo-elem  = 1

Example: #nav .item a:hover → 100 + 10 + 1 + 10 = 121
```

### Ways to Apply CSS
```html
<!-- Inline -->
<p style="color:red;">Text</p>

<!-- Internal -->
<style> p { color: red; } </style>

<!-- External (best practice) -->
<link rel="stylesheet" href="styles.css">
```

---

## 2. Basics (Beginner)

### Selectors
```css
* { margin: 0; }                  /* universal */
h1 { color: navy; }               /* element */
.card { padding: 10px; }          /* class */
#header { background: #333; }     /* id */
a:hover { color: red; }           /* pseudo-class */
p::first-line { font-weight: bold; }  /* pseudo-element */
input[type="text"] { border: 1px solid gray; }  /* attribute */
div > p { color: green; }         /* direct child */
div p { color: blue; }            /* descendant */
h1 + p { margin-top: 0; }         /* adjacent sibling */
h1 ~ p { color: gray; }           /* general sibling */
```

### Colors, Units, Typography
```css
p {
    color: #333333;
    background-color: rgba(0,0,0,0.1);
    font-family: 'Segoe UI', sans-serif;
    font-size: 16px;    /* absolute */
    line-height: 1.5;
    font-weight: 600;
}
/* Relative units */
.container { width: 90%; padding: 2rem; margin: 1em; }
```
**px vs em vs rem (common interview Q):**
- `px` — fixed, absolute.
- `em` — relative to the **parent's** font-size (compounds with nesting).
- `rem` — relative to the **root** (`<html>`) font-size (predictable, preferred for scalability).

---

## 3. Box Model & Layout Basics

### The Box Model (Fundamental Theory — Always Asked)
Every element is a box made of: **Content → Padding → Border → Margin** (innermost to outermost).

```css
.box {
    width: 300px;
    padding: 20px;
    border: 5px solid black;
    margin: 10px;
    box-sizing: border-box; /* width includes padding & border, NOT content-box default */
}
```
`box-sizing: border-box` vs default `content-box` is one of the most common practical interview questions — border-box makes `width` the *total* width including padding/border, avoiding layout surprises.

### Display Property
```css
.a { display: block; }    /* full width, new line */
.b { display: inline; }   /* no width/height control, flows inline */
.c { display: inline-block; } /* inline flow but width/height respected */
.d { display: none; }     /* removed from layout entirely */
```

---

## 4. Flexbox

```css
.container {
    display: flex;
    flex-direction: row;        /* row | column | row-reverse | column-reverse */
    justify-content: center;    /* main-axis alignment: flex-start, center, space-between, space-around */
    align-items: center;        /* cross-axis alignment */
    flex-wrap: wrap;
    gap: 10px;
}
.item {
    flex: 1;                    /* flex-grow flex-shrink flex-basis shorthand */
    order: 2;
    align-self: flex-end;
}
```
**Theory:** Flexbox is a **one-dimensional** layout system (row OR column) designed for distributing space among items in a container, even when their size is unknown.

---

## 5. Grid

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: 100px auto;
    gap: 20px;
    grid-template-areas:
        "header header header"
        "sidebar content content";
}
.header { grid-area: header; }
.item {
    grid-column: 1 / 3;   /* span from line 1 to 3 */
    grid-row: span 2;
}
```
**Theory:** Grid is a **two-dimensional** layout system (rows AND columns simultaneously) — ideal for full page layouts, whereas Flexbox suits single-axis component layouts (a very common "Flexbox vs Grid" interview question).

---

## 6. Positioning & Stacking

```css
.static   { position: static; }    /* default, normal flow */
.relative { position: relative; top: 10px; }  /* offset from its own normal position, still takes space */
.absolute { position: absolute; top: 0; right: 0; }  /* removed from flow, positioned relative to nearest positioned ancestor */
.fixed    { position: fixed; bottom: 0; }   /* relative to viewport, stays on scroll */
.sticky   { position: sticky; top: 0; }     /* toggles between relative & fixed based on scroll */
```

### z-index & Stacking Context
```css
.modal { position: fixed; z-index: 1000; }
```
**Theory:** `z-index` only works on positioned elements (not `static`). A new **stacking context** is created by properties like `position` + `z-index`, `opacity < 1`, `transform`, or `filter` — a subtle but common bug source ("why isn't my z-index working?").

---

## 7. Responsive Design

### Media Queries
```css
/* Mobile-first approach (recommended) */
.container { width: 100%; }

@media (min-width: 768px) {
    .container { width: 750px; }
}
@media (min-width: 1024px) {
    .container { width: 970px; }
}
```

### Responsive Units & Techniques
```css
.hero { font-size: clamp(1.5rem, 4vw, 3rem); }  /* fluid typography */
img { max-width: 100%; height: auto; }
.grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); }
```

---

## 8. Advanced CSS

### CSS Variables (Custom Properties)
```css
:root {
    --primary-color: #3498db;
    --spacing: 16px;
}
.btn {
    background: var(--primary-color);
    padding: var(--spacing);
}
```

### Transitions & Animations
```css
.btn {
    transition: background-color 0.3s ease, transform 0.2s;
}
.btn:hover { background-color: darkblue; transform: scale(1.05); }

@keyframes slideIn {
    from { transform: translateX(-100%); opacity: 0; }
    to   { transform: translateX(0); opacity: 1; }
}
.card { animation: slideIn 0.5s ease-out forwards; }
```

### Transform
```css
.box {
    transform: translate(10px, 20px) rotate(45deg) scale(1.2);
}
```

### Pseudo-classes/elements for interactivity
```css
.input:focus { outline: 2px solid blue; }
.item:nth-child(odd) { background: #f5f5f5; }
.item:first-child { margin-top: 0; }
.tooltip::before { content: "Info"; }
```

### CSS Preprocessors (Theory — SASS/SCSS often mentioned in interviews)
```scss
$primary: #3498db;
.card {
    color: $primary;
    &:hover { color: darken($primary, 10%); }
    .title { font-weight: bold; }  // nesting
}
```
**Why use SASS?** Variables (pre-CSS-vars era), nesting, mixins, functions, partial imports — compiles down to plain CSS.

---

## 9. CSS Architecture & Real Project Practices

### BEM Naming Convention (Common in Production Codebases)
```css
/* Block__Element--Modifier */
.card { }
.card__title { }
.card__title--large { }
.card--highlighted { }
```

### Common Real-World Issues & Fixes
- **Collapsing margins** — adjacent vertical margins of block elements merge; fixed by padding, border, or `display: flow-root`.
- **Specificity wars** — overusing `!important` and deep nesting causes unmaintainable CSS; solved with BEM/utility classes and consistent architecture.
- **Layout shift (CLS)** — images/ads without reserved space cause visual jank; fix with explicit `width`/`height` or `aspect-ratio`.
- **Overflow issues** — `overflow: hidden`, `auto`, `scroll` used to control content that exceeds a container.

### CSS Frameworks (Awareness)
Tailwind CSS (utility-first), Bootstrap (component-based) — commonly asked about in interviews for comparison/tradeoffs (utility classes = faster dev + smaller learning curve for design consistency vs custom CSS = full control but more time).

---

## Quick Revision Cheat-Sheet
| Concept | One-Line Recall |
|---|---|
| Box model | content → padding → border → margin |
| Flexbox vs Grid | Flex = 1D (row/col); Grid = 2D (rows+cols) |
| position: sticky | Toggles relative↔fixed based on scroll position |
| Specificity order | inline > ID > class/attr/pseudo-class > element |
| em vs rem | em relative to parent; rem relative to root html |
