# SQL — Managerial Round Interview Questions

Managerial rounds test judgment, ownership, communication, and how you apply SQL/DB knowledge in real project/team contexts — not raw syntax.

## Project & Ownership

**Q1. Tell me about a time you optimized a slow-performing query or database in production.**
Structure your answer: Situation → what was slow and why it mattered → your investigation (EXPLAIN, indexing) → the fix → measurable impact (e.g. "reduced query time from 8s to 200ms").

**Q2. Describe a data-related production incident you handled. What was your role?**
Use a real or realistic scenario (see Real-Time Production Scenarios file) — emphasize calm diagnosis, communication with stakeholders, and post-incident prevention steps.

**Q3. How do you decide when to denormalize a schema?**
Discuss trade-offs: read performance vs write complexity/data integrity risk; give a concrete example (e.g. storing a computed order total to avoid repeated joins in a high-read reporting table).

**Q4. How do you ensure data integrity across a growing team writing to the same database?**
Talk about code review for migrations, constraints/foreign keys as guardrails, staging environment testing, and access control (no direct prod writes).

**Q5. Tell me about a time you disagreed with a teammate's schema/query design. How did you resolve it?**
Use STAR format; focus on data-driven reasoning (benchmarks, EXPLAIN output) rather than opinion, and on how you reached consensus.

## Decision-Making & Trade-offs

**Q6. How do you decide between SQL and NoSQL for a new project?**
Discuss data structure (relational vs flexible), consistency needs, query patterns, team familiarity, and scaling requirements — show it's a deliberate trade-off, not a default choice.

**Q7. How would you handle a situation where a critical query needs to run faster, but adding an index would slow down a high-write table?**
Discuss weighing read vs write frequency, considering a read replica for reporting, or a covering/partial index, or caching layer instead of blindly indexing.

**Q8. How do you balance shipping fast vs writing "correct" database schema/migrations?**
Talk about the expand-contract migration pattern, feature flags, and incremental schema evolution rather than big-bang rewrites.

**Q9. How do you prioritize technical debt related to database design against new feature work?**
Discuss quantifying impact (incident frequency, query cost, developer time lost), and negotiating dedicated time with product/stakeholders using data.

## Team & Process

**Q10. How do you mentor a junior engineer who keeps writing inefficient SQL?**
Discuss pairing, code review with explanation (not just "fix this"), sharing EXPLAIN output walkthroughs, and building a shared checklist/style guide.

**Q11. What's your process for reviewing a teammate's database migration before it goes to production?**
Check for: backward compatibility, index impact, lock behavior on large tables, rollback plan, tested on staging with realistic data volume.

**Q12. How do you handle a production database emergency at 2 AM?**
Talk about following a runbook, checking monitoring/dashboards first, communicating status to stakeholders early, prioritizing mitigation over root-cause-analysis in the moment, and a blameless postmortem afterward.

## Tips for Managerial Round
- Use the **STAR method** (Situation, Task, Action, Result) for behavioral answers.
- Always quantify impact where possible (time saved, downtime avoided, cost reduced).
- Show collaboration and communication, not just technical correctness.
- Be honest about mistakes — talk about what you learned, not just successes.
