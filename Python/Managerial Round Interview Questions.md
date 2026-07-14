# Python Developer Role — Managerial Round Interview Questions

## Project & Ownership

**Q1. Tell me about a Python project where performance or scale was a major challenge.**
Use STAR; discuss specific techniques (vectorization, caching, multiprocessing, query optimization) and measurable impact.

**Q2. Describe a production incident caused by a Python bug and how you resolved it.**
Emphasize systematic debugging (profiling, logs), the fix, and prevention steps (tests, monitoring) added afterward.

**Q3. Tell me about a time you designed the architecture for a new Python service/API.**
Discuss the requirements, the design decisions made (framework choice, sync vs async, data layer), and trade-offs considered.

## Decision-Making & Trade-offs

**Q4. How do you decide between using threads, processes, or asyncio for a given problem?**
Discuss the nature of the workload — I/O-bound favors asyncio/threading, CPU-bound favors multiprocessing — and give a concrete example from your experience.

**Q5. How do you balance rapid feature delivery with code quality/testing in Python projects?**
Discuss risk-based testing priorities, incremental refactoring, and negotiating time for tech debt with stakeholders using concrete impact data.

**Q6. How do you decide when to introduce a new dependency/library into a codebase?**
Discuss evaluating maintenance activity, security implications, team familiarity, and whether the problem justifies the added complexity versus writing it in-house.

## Team & Process

**Q7. How do you review a teammate's Python pull request?**
Check for: proper exception handling, test coverage of critical logic, adherence to style guidelines (PEP 8/linting), and any obvious performance/concurrency pitfalls (mutable defaults, blocking calls in async code).

**Q8. How do you mentor a junior developer who writes working but non-idiomatic Python?**
Pair programming, explain the "why" behind Pythonic patterns (comprehensions, context managers), share style guide references, and reinforce through code review.

**Q9. How do you handle a critical production incident involving a Python service at odd hours?**
Discuss following a runbook, prioritizing mitigation (rollback/hotfix) over root-cause analysis in the moment, clear stakeholder communication, and a blameless postmortem afterward.

## Tips for Managerial Round
- Use the STAR method for behavioral answers.
- Quantify impact (latency reduction, throughput improvement, bug/incident reduction).
- Show collaboration, ownership, and calm problem-solving under pressure.
- Be honest about past mistakes and what you changed as a result.
