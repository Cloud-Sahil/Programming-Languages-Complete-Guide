# CSS / Frontend Styling — Managerial Round Interview Questions

## Project & Ownership

**Q1. Tell me about a time you had to fix a widespread visual bug across many pages.**
Structure with STAR; discuss root-causing a shared style/token issue rather than patching each page individually, and how you verified the fix broadly.

**Q2. Describe a time you improved page load performance through CSS changes.**
Mention specifics: reducing bundle size, replacing layout-triggering animations with transform/opacity, using `content-visibility`, and the measurable before/after impact.

**Q3. Tell me about building or contributing to a design system / component library.**
Discuss establishing consistent tokens (spacing, color, typography), documenting usage, and driving adoption across teams.

## Decision-Making & Trade-offs

**Q4. How do you decide on a CSS architecture (BEM, utility-first, CSS Modules, CSS-in-JS) for a new project?**
Discuss factors: team size/experience, framework in use, performance requirements, long-term maintainability — show it's a deliberate decision, not a default habit.

**Q5. How do you balance design fidelity against browser support constraints?**
Discuss using `@supports` feature queries for progressive enhancement, negotiating with design on acceptable degradation for older browsers.

**Q6. A designer wants an experience that conflicts with accessibility best practices (e.g. removing focus outlines). How do you handle it?**
Explain the accessibility risk clearly, propose an alternative that satisfies both aesthetics and usability (custom focus styles instead of removing them entirely).

## Team & Process

**Q7. How do you review CSS in a pull request?**
Check for: unnecessary specificity/`!important`, consistent use of design tokens/variables, responsive behavior, and adherence to team conventions.

**Q8. How do you prevent CSS bloat/dead code as a codebase and team grow?**
Discuss periodic coverage audits, code-splitting per route/component, enforcing scoped styles (CSS Modules) over global stylesheets, and bundle size budgets in CI.

**Q9. How do you mentor a junior developer struggling with layout bugs (Flexbox/Grid)?**
Pair on debugging with DevTools, explain the mental model (main/cross axis, box model), and share reusable patterns/snippets for common layouts.

## Tips for Managerial Round
- Use the STAR method for behavioral answers.
- Quantify impact where possible (bundle size reduction, Lighthouse score, time saved).
- Emphasize collaboration with design and cross-team consistency.
- Be honest about a past architecture decision that didn't scale well and what you learned.
