# HTML — Interview Questions (1–4 Years Experience)

## Conceptual

**Q1. What is semantic HTML? Why does it matter?**
Tags that convey meaning about their content (`header`, `nav`, `article`, `footer` vs generic `div`). Improves accessibility, SEO, and code readability.

**Q2. Difference between `<div>` and `<span>`?**
`<div>` is a block-level element (own line, full width by default); `<span>` is inline (flows within text, only as wide as content).

**Q3. Difference between `id` and `class` attributes?**
`id` must be unique per page, used for a single specific element (also used as JS/CSS hook and anchor target). `class` can be reused across multiple elements for shared styling/behavior.

**Q4. What's the difference between GET and POST in a form?**
GET appends data to the URL (visible, cacheable, has length limits, idempotent — good for search/retrieval). POST sends data in the request body (not visible in URL, used for creating/submitting data, not idempotent by default).

**Q5. What is the DOM?**
Document Object Model — a tree-like, in-memory representation of the HTML document that JavaScript can read/manipulate.

**Q6. What are void/self-closing elements? Give examples.**
Elements with no closing tag/content: `<img>`, `<br>`, `<hr>`, `<input>`, `<meta>`, `<link>`.

**Q7. Difference between `<script>`, `<script async>`, and `<script defer>`?**
- Default: blocks HTML parsing, downloads and executes immediately.
- `async`: downloads in parallel, executes as soon as ready (order not guaranteed) — good for independent scripts (analytics).
- `defer`: downloads in parallel, executes after HTML parsing completes, in order — good for scripts depending on DOM.

**Q8. What is the difference between `localStorage`, `sessionStorage`, and cookies?**
| | Persistence | Sent to server | Size |
|---|---|---|---|
| localStorage | Until cleared | No | ~5-10MB |
| sessionStorage | Until tab closes | No | ~5-10MB |
| Cookies | Configurable (expiry) | Yes, with every request | ~4KB |

**Q9. What is an iframe and what are its risks?**
Embeds another HTML document within the current page. Risks: clickjacking, performance impact, security (mitigate with `sandbox` attribute and `X-Frame-Options`/CSP headers).

**Q10. What's the difference between `<meta charset="UTF-8">` and other encodings?**
UTF-8 supports virtually all characters/languages and is the web standard; older encodings (like ISO-8859-1) support fewer characters and can cause garbled text for non-Latin scripts.

## Practical

**Q11. How do you make an image responsive?**
```html
<img src="pic.jpg" style="max-width:100%; height:auto;" alt="...">
<!-- or with srcset for different resolutions -->
<img srcset="small.jpg 480w, large.jpg 1080w" sizes="(max-width:600px) 480px, 1080px" src="large.jpg" alt="...">
```

**Q12. How do you group form controls semantically?**
```html
<fieldset>
  <legend>Personal Info</legend>
  <label for="name">Name</label>
  <input id="name" name="name" type="text">
</fieldset>
```

**Q13. How would you build an accessible custom dropdown vs using native `<select>`?**
Prefer `<select>` when possible (built-in a11y). If custom UI is required, add `role="listbox"`, `aria-expanded`, keyboard handlers (arrow keys, Enter, Escape), and manage focus manually.

**Q14. How do you embed video with fallback text for unsupported browsers?**
```html
<video controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.webm" type="video/webm">
  Your browser does not support the video tag.
</video>
```

**Q15. How would you improve the SEO of a landing page using only HTML?**
Single meaningful `<h1>`, descriptive `<title>` and `<meta description>`, semantic structure, `alt` text on images, canonical tag, structured data (JSON-LD), fast-loading optimized assets.

## Scenario-Based

**Q16. A form isn't submitting data for a field — what would you check first?**
Check the `name` attribute exists and is correctly spelled, check it's inside the `<form>` tags, check it's not `disabled`, inspect the Network tab payload.

**Q17. How do you ensure your website works well for screen reader users?**
Use semantic tags, proper heading order, `alt` text, labeled form fields, ARIA only where native semantics fall short, and test with an actual screen reader (NVDA/VoiceOver).

**Q18. How would you lazy-load images on a long product listing page?**
```html
<img src="product.jpg" loading="lazy" alt="Product name">
```
Combined with `srcset` for responsive sizing and a CDN for delivery.
