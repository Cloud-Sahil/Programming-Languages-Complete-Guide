# JavaScript / Frontend-Backend Role — Managerial Round Interview Questions

## Project & Ownership

**Q1. Tell me about a challenging bug you debugged in production JavaScript code.**
Use STAR; emphasize systematic debugging (reproducing, isolating, monitoring tools), the fix, and prevention steps put in place afterward.

**Q2. Describe a time you improved application performance (load time, responsiveness).**
Mention concrete techniques (code-splitting, debouncing, memoization, Web Workers) and measurable results.

**Q3. Tell me about a project where you had to make an architectural decision (e.g. state management approach, async pattern).**
Discuss the trade-offs you weighed and why you chose the approach you did.

## Decision-Making & Trade-offs

**Q4. How do you decide between different async patterns (callbacks, promises, async/await) for a given problem?**
Discuss readability, error handling ergonomics, and how async/await is generally preferred for sequential logic while Promise combinators (`Promise.all`) suit parallel work.

**Q5. How do you approach technical debt in a fast-moving JS codebase?**
Discuss quantifying impact (bug frequency, dev velocity), incremental refactoring alongside feature work, and negotiating dedicated time when debt blocks delivery.

**Q6. How do you balance shipping quickly vs writing well-tested, maintainable JS code?**
Discuss risk-based testing (critical paths get more coverage), incremental improvement rather than big rewrites, and communicating trade-offs to stakeholders.

## Team & Process

**Q7. How do you review a teammate's JavaScript pull request?**
Check for: proper error handling (especially async), memory leak risks (uncleaned listeners/timers), code readability, test coverage of critical logic.

**Q8. How do you mentor a junior developer who struggles with async/await or closures?**
Pair on real examples, use visual/step-through debugging to show execution order, share small practice problems, and reinforce with code review feedback.

**Q9. How do you handle a critical production incident caused by a JS bug?**
Discuss triage/mitigation first (rollback/hotfix), clear stakeholder communication, and a blameless postmortem focused on process improvement.

## Tips for Managerial Round
- Use the STAR method for behavioral answers.
- Quantify impact (load time reduction, bug reduction, deployment frequency).
- Show ownership and calm problem-solving under pressure.
- Be honest about past mistakes and the lessons learned.
