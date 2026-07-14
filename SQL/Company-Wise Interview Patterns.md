# SQL — Company-Wise Interview Patterns

> Note: exact questions vary by year/interviewer. This describes typical **patterns and focus areas** reported by candidates for each company category, not confirmed leaked question banks.

## Service-Based Companies (TCS, Infosys, Wipro, Cognizant, Accenture, Capgemini)
- **Focus**: Fundamentals — joins, subqueries, normalization, basic DDL/DML, constraints.
- **Style**: Written test (MCQ + query writing) followed by 1–2 technical rounds.
- **Typical asks**: Write a query for Nth highest salary, explain joins with examples, explain normalization forms, simple aggregate queries, difference between DELETE/TRUNCATE/DROP.
- **Tip**: These interviews reward being able to clearly explain basics and write clean, correct syntax under time pressure.

## Product-Based Companies (Amazon, Flipkart, Microsoft, Google, Uber, Ola, Swiggy, Zomato)
- **Focus**: Query optimization, indexing, real-world schema design, window functions, handling scale.
- **Style**: Live coding on a shared editor/whiteboard, sometimes combined with system design.
- **Typical asks**: Design a schema for a ride-booking/food-delivery system, write a query using window functions (top-N per group), explain how you'd optimize a slow query, discuss indexing trade-offs, discuss handling millions of concurrent transactions.
- **Tip**: Expect follow-ups on "how would this scale to 10M rows / 100K QPS" — always mention indexing and query plan reasoning, not just correct syntax.

## Fintech / Banking (Paytm, PhonePe, Razorpay, Goldman Sachs, JPMorgan, banks)
- **Focus**: Transactions, ACID, isolation levels, data integrity, concurrency, precision (DECIMAL vs FLOAT for money).
- **Typical asks**: Explain ACID with a money-transfer example, how do you prevent double-spending/duplicate payment processing (idempotency), explain isolation levels and dirty reads, why not use FLOAT for currency.
- **Tip**: Be ready to reason about correctness and consistency guarantees in detail — these interviews probe deeper into transaction behavior than product companies typically do.

## Analytics / Data Companies (Deloitte, EY, data analyst roles, MAANG data teams)
- **Focus**: Complex aggregations, window functions, CTEs, business-metric queries (retention, cohort analysis, funnel analysis).
- **Typical asks**: Calculate month-over-month growth, write a cohort retention query, find users who purchased in month 1 but not month 2, running totals with window functions.
- **Tip**: Practice writing multi-step CTEs that mirror real business questions (churn, retention, conversion funnels).

## Startups (Series A–C product companies)
- **Focus**: Practical, pragmatic — can you get a working, reasonably efficient query fast; some system design mixed in.
- **Typical asks**: Take-home SQL assignment on a sample schema, live query writing against a real-ish dataset, discussion of trade-offs (SQL vs NoSQL for their use case).
- **Tip**: Communicate your thought process out loud; startups value speed + pragmatism over textbook-perfect answers.

## General Pattern Across All Companies
1. **Round 1** — Online assessment / written test: MCQs + 2–3 query-writing problems.
2. **Round 2** — Technical interview: live query writing, conceptual questions, sometimes a mini schema design.
3. **Round 3** (product/senior roles) — Query optimization + system design overlap.
4. **Round 4** — Managerial round.
5. **Round 5** — HR round.

## What Interviewers Commonly Evaluate
- Correct, syntactically clean queries under time pressure
- Ability to explain *why*, not just *what* (e.g. why an index helps, not just that it does)
- Structured thinking — breaking a complex ask into CTEs/subqueries
- Awareness of performance/scale implications
- Communication — explaining your query logic clearly
