# HTML — Zero to Hero: Complete Study Guide

## Table of Contents
1. Theory & Fundamentals
2. Basics (Beginner)
3. Forms & Input Handling
4. Semantic HTML & Accessibility
5. Advanced HTML (Media, Canvas, SVG, Web Components)
6. HTML5 APIs
7. Performance & SEO
8. User-Side / Real Project Integration

---

## 1. Theory & Fundamentals

### What is HTML?
HTML (HyperText Markup Language) is the standard **markup language** used to structure content on the web. It is not a programming language — it has no logic/loops — it describes structure via elements (tags).

### How a Browser Renders HTML (Important Theory)
1. Browser parses HTML → builds the **DOM (Document Object Model)** tree.
2. Browser parses CSS → builds the **CSSOM** tree.
3. DOM + CSSOM combine → **Render Tree**.
4. Browser calculates **Layout** (geometry: position & size of each element).
5. Browser **Paints** pixels to the screen.
6. **Compositing** — layers are combined for final display.

This DOM/CSSOM/Render Tree/Layout/Paint pipeline is a very common interview question ("What happens when you type a URL and hit enter" / "How does a browser render a page").

### HTML Document Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Page</title>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
```
**Theory:** `<!DOCTYPE html>` tells the browser to render in **standards mode** (vs quirks mode) per HTML5 spec.

---

## 2. Basics (Beginner)

### Headings, Paragraphs, Text Formatting
```html
<h1>Main Heading</h1>
<h2>Sub Heading</h2>
<p>This is a <strong>bold</strong> and <em>italic</em> paragraph.</p>
<br>
<hr>
```

### Lists
```html
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
</ul>

<ol>
    <li>Step 1</li>
    <li>Step 2</li>
</ol>
```

### Links & Images
```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">Visit Site</a>
<img src="photo.jpg" alt="A description for accessibility & SEO" width="300" height="200">
```
**Theory:** `alt` attribute is critical for accessibility (screen readers) and SEO — a very common code-review/interview point.

### Tables
```html
<table>
    <thead>
        <tr><th>Name</th><th>Age</th></tr>
    </thead>
    <tbody>
        <tr><td>Rahul</td><td>25</td></tr>
    </tbody>
</table>
```

### div vs span (Common Interview Q)
- `<div>` — block-level generic container (takes full width, starts new line).
- `<span>` — inline generic container (only takes needed width, no new line).

---

## 3. Forms & Input Handling

```html
<form action="/submit" method="POST">
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required placeholder="Enter name">

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>

    <label for="pwd">Password:</label>
    <input type="password" id="pwd" name="pwd" minlength="8">

    <label for="age">Age:</label>
    <input type="number" id="age" name="age" min="18" max="60">

    <select name="country">
        <option value="in">India</option>
        <option value="us">USA</option>
    </select>

    <input type="radio" name="gender" value="m"> Male
    <input type="radio" name="gender" value="f"> Female

    <input type="checkbox" name="subscribe" checked> Subscribe

    <textarea name="message" rows="4" cols="30"></textarea>

    <input type="file" name="upload">
    <input type="date" name="dob">
    <button type="submit">Submit</button>
</form>
```

### GET vs POST (Frequently Asked)
- **GET** — data appended to URL, visible, cacheable, used for retrieving data, has length limits.
- **POST** — data sent in request body, not visible in URL, used for submitting/creating data, no strict length limit.

### Form Validation
- **Client-side (HTML5):** `required`, `pattern`, `min`, `max`, `minlength`, `type="email"`.
- **Server-side:** always required too — client-side validation can be bypassed, so never trust the frontend alone (important real-world/security point).

---

## 4. Semantic HTML & Accessibility

### Semantic Tags
```html
<header>Site Header</header>
<nav>Navigation Links</nav>
<main>
    <article>
        <section>Section content</section>
    </article>
    <aside>Sidebar</aside>
</main>
<footer>Site Footer</footer>
```
**Theory:** Semantic tags convey *meaning* to browsers, screen readers, and search engines — improving SEO and accessibility versus using generic `<div>` for everything.

### Accessibility (a11y) — Common in Modern Interviews
```html
<button aria-label="Close menu">×</button>
<img src="chart.png" alt="Sales grew 20% year over year">
<input type="text" aria-required="true" aria-describedby="name-help">
<div role="alert">Form submitted successfully!</div>
```
- Use proper heading hierarchy (h1 → h2 → h3), don't skip levels.
- Ensure sufficient color contrast (handled with CSS but tested via accessibility tools).
- All interactive elements must be keyboard-navigable (tabindex, focus states).

---

## 5. Advanced HTML

### Multimedia
```html
<video src="movie.mp4" controls width="400" autoplay muted loop></video>
<audio src="song.mp3" controls></audio>
```

### Canvas (used for drawing/graphics via JS)
```html
<canvas id="myCanvas" width="200" height="100"></canvas>
<script>
    const ctx = document.getElementById('myCanvas').getContext('2d');
    ctx.fillStyle = 'blue';
    ctx.fillRect(10, 10, 100, 50);
</script>
```

### SVG (Scalable Vector Graphics)
```html
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" stroke="green" fill="yellow"/>
</svg>
```
**SVG vs Canvas (interview Q):** SVG is DOM-based/vector (scales infinitely, each shape is an element you can style/animate with CSS); Canvas is pixel-based/raster (drawn imperatively via JS, better for complex/frequent redraws like games).

### iframe
```html
<iframe src="https://example.com" width="600" height="400" title="embedded content"></iframe>
```

### Web Components (Advanced/Modern)
```html
<template id="my-template">
    <style>.box { border: 1px solid #333; }</style>
    <div class="box"><slot></slot></div>
</template>

<script>
class MyBox extends HTMLElement {
    constructor() {
        super();
        const tmpl = document.getElementById('my-template');
        this.attachShadow({mode: 'open'}).appendChild(tmpl.content.cloneNode(true));
    }
}
customElements.define('my-box', MyBox);
</script>

<my-box>Hello from a custom element!</my-box>
```

---

## 6. HTML5 APIs (Used Constantly in Real Applications)

```html
<!-- Local Storage -->
<script>
localStorage.setItem('theme', 'dark');
const theme = localStorage.getItem('theme');

sessionStorage.setItem('session_id', 'abc123'); // cleared when tab closes

// Geolocation
navigator.geolocation.getCurrentPosition(pos => {
    console.log(pos.coords.latitude, pos.coords.longitude);
});

// Drag and Drop
document.querySelector('.drag').addEventListener('dragstart', e => {
    e.dataTransfer.setData('text', e.target.id);
});
</script>

<!-- Data attributes -->
<div id="product" data-id="123" data-price="499.00"></div>
<script>
const price = document.getElementById('product').dataset.price;
</script>
```
**localStorage vs sessionStorage vs cookies (common interview Q):**
| Feature | localStorage | sessionStorage | Cookies |
|---|---|---|---|
| Expiry | Never (until cleared) | On tab close | Configurable |
| Size | ~5-10MB | ~5-10MB | ~4KB |
| Sent to server | No | No | Yes, with every HTTP request |

---

## 7. Performance & SEO

- Use `loading="lazy"` on images below the fold.
- Minimize render-blocking resources; put `<script>` at the end of `<body>` or use `defer`/`async`.
- Use proper `<meta name="description">` and semantic tags for SEO.
- Use responsive images with `srcset`:
```html
<img src="small.jpg" srcset="small.jpg 480w, large.jpg 1024w" sizes="(max-width: 600px) 480px, 1024px" alt="Responsive image">
```

### Script Loading (async vs defer — Frequently Asked)
```html
<script src="a.js" async></script>   <!-- downloads in parallel, executes ASAP (order not guaranteed) -->
<script src="b.js" defer></script>   <!-- downloads in parallel, executes after HTML parsing, in order -->
```

---

## 8. User-Side / Real Project Integration

### Meta Viewport for Responsive/Mobile
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
Without this, mobile browsers render the page at desktop width and scale down — a very common real-world bug ("site looks zoomed out on mobile").

### Progressive Enhancement in Production
- Build a working baseline with pure HTML (forms submit normally even if JS fails).
- Layer CSS for style, then JS for enhanced interactivity — ensures the app degrades gracefully if JS fails to load (real production resilience pattern).

### Common Real-World Bugs
- Missing `alt` text → accessibility audit failures.
- Forgetting `for`/`id` pairing on `<label>` → clicking label doesn't focus input.
- Not closing tags properly → unpredictable rendering in older browsers.
- Using `<div>` for buttons instead of `<button>` → breaks keyboard accessibility and form submission behavior.

---

## Quick Revision Cheat-Sheet
| Concept | One-Line Recall |
|---|---|
| DOM | Tree representation of HTML document created by browser |
| Semantic tags | Convey meaning; improve SEO/accessibility |
| async vs defer | async = execute ASAP any order; defer = execute in order after parse |
| GET vs POST | GET=URL params/idempotent; POST=body/for creating/updating |
| localStorage vs cookies | localStorage=client-only,large; cookies=sent to server,small |
