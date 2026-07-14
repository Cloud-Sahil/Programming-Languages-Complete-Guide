# CSS — Company-Wise Interview Patterns

> Reflects general patterns reported for each company category, not confirmed question banks.

## Service-Based Companies (TCS, Infosys, Wipro, Cognizant, Accenture)
- **Focus**: Box model, selectors, basic Flexbox, specificity fundamentals.
- **Style**: MCQ round + short live styling task.
- **Typical asks**: Center a div, explain box model, difference between Flexbox and Grid at a basic level, specificity rules.

## Product-Based Companies (Amazon, Flipkart, Microsoft, Swiggy, Zomato, Myntra)
- **Focus**: Real layout implementation from a design mock, responsive design, performance, and reasoning about trade-offs (Flexbox vs Grid, animation performance).
- **Style**: Live coding — implement a given UI, often timed, sometimes paired with a take-home.
- **Typical asks**: Build a responsive card grid, implement a sticky header with `position: sticky`, debug a provided broken CSS snippet, discuss `content-visibility`/performance of large lists.

## Design-System / Platform Teams (larger orgs building internal component libraries)
- **Focus**: CSS architecture, theming, scalability, accessibility of design tokens.
- **Typical asks**: How would you architect CSS for a design system used by 50+ engineers; how do you implement theming/dark mode at scale; explain `@layer` and cascade layers usage.

## Startups
- **Focus**: Speed and pragmatism — Tailwind/utility-first familiarity is often valued.
- **Typical asks**: Build a landing page section quickly from a Figma link; discuss trade-offs of utility-first CSS vs traditional CSS.

## General Pattern Across All Companies
1. **Round 1** — Assessment/take-home: implement a design from a mock.
2. **Round 2** — Live coding: build or fix a component (often combined with HTML/JS).
3. **Round 3** — Deeper technical: responsive strategy, performance, accessibility, architecture reasoning.
4. **Round 4** — Managerial round.
5. **Round 5** — HR round.

## What Interviewers Commonly Evaluate
- Correct, efficient use of Flexbox/Grid for the right use case
- Understanding of specificity/cascade (not just "add !important")
- Responsive, mobile-first thinking
- Awareness of performance implications (animating transform vs layout properties)
- Clean, maintainable CSS architecture (naming conventions, avoiding overly specific selectors)
