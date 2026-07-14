# Python — Company-Wise Interview Patterns

> Reflects general patterns reported for each company category, not confirmed question banks.

## Service-Based Companies (TCS, Infosys, Wipro, Cognizant, Accenture)
- **Focus**: Core syntax, OOP basics, data structures, simple scripting tasks.
- **Style**: MCQ/written round + a technical interview with basic coding problems.
- **Typical asks**: Difference between list/tuple, OOP concepts with examples, simple string/list manipulation coding problems, explain exception handling.

## Product-Based Companies (Amazon, Google, Microsoft, Flipkart, Uber)
- **Focus**: DSA in Python (often language-agnostic problems solved in Python), plus deeper language internals (GIL, memory management) for backend-focused roles.
- **Style**: Live coding rounds (LeetCode-style), a dedicated Python fundamentals round for backend roles, system design for 3+ years experience.
- **Typical asks**: Solve algorithmic problems, explain the GIL and multiprocessing vs threading, design a rate limiter/cache using Python, discuss Python's memory management.

## Data/AI/ML-Focused Companies
- **Focus**: pandas/NumPy proficiency, understanding of vectorization, familiarity with ML libraries and pipeline tools.
- **Typical asks**: Clean and transform a messy dataset using pandas, explain vectorization vs loops, discuss a past ML/data project end-to-end, write efficient data processing code.

## Fintech / Backend-Heavy Companies (Razorpay, Paytm, banks)
- **Focus**: Concurrency correctness, transaction safety, API design, testing discipline.
- **Typical asks**: How do you prevent race conditions in a Python service, design a REST API for a given use case, explain idempotency in payment processing, discuss testing strategy for critical financial code.

## Startups
- **Focus**: Full-stack pragmatism — can you build and ship a working feature/API quickly with reasonable code quality.
- **Typical asks**: Build a small CRUD API (often FastAPI/Flask) live or as a take-home, general problem-solving discussion, questions about your past project architecture.

## General Pattern Across All Companies
1. **Round 1** — Online assessment: MCQs + DSA coding problems.
2. **Round 2** — Live coding: algorithmic problem-solving + Python fundamentals questions.
3. **Round 3** — Deeper technical (framework-specific, system design for senior roles): API design, concurrency, database interaction, testing.
4. **Round 4** — Managerial round.
5. **Round 5** — HR round.

## What Interviewers Commonly Evaluate
- Clean, idiomatic ("Pythonic") code — using comprehensions, context managers, appropriate data structures
- Understanding of Python internals (GIL, memory model) beyond surface syntax
- Problem-solving process — breaking down a problem before coding
- Awareness of production concerns: error handling, testing, performance, concurrency correctness
- Communication while coding — explaining trade-offs and edge cases
