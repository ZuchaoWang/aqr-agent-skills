---
name: aqr-project-principle
description: Use when doing any non-trivial work on a project — making changes, writing or updating docs, investigating. Defines the quality standards the work should meet: clarify intent before committing to significant changes, treat documentation as the project's durable memory, optimize work for human review, keep the solution simple, and verify before claiming completion.
disable-model-invocation: false
---

# aqr-project-principle

These principles define the quality standards for work on a project. They describe **what good work looks like**, not a fixed workflow — the agent may choose any execution strategy that satisfies them.

## 1. Clarify Before Committing

Never silently invent important requirements or design decisions.

Before making significant semantic or architectural changes:

- clarify ambiguous requirements
- investigate existing code and documentation when appropriate
- surface important assumptions and trade-offs
- obtain agreement when uncertainty materially affects the solution

The objective is to discover incorrect assumptions as early as possible, before implementation.

## 2. Documentation Is the Project Memory

Project documentation records durable project knowledge.

Documentation should contain long-lived information such as:

- architecture decisions
- interface contracts
- module responsibilities
- project conventions

Follow documented decisions unless intentionally changing them.

Do not preserve temporary reasoning or planning unless it provides long-term value.

## 3. Optimize for Human Review

Documentation exists primarily for human review.

Therefore:

- keep documents concise
- write only information worth reviewing
- prefer updating existing documents over creating new ones
- avoid duplication and unnecessary detail

Every document should remain practical to review.

## 4. Keep Solution Simple

Prefer the simplest complete solution.

Avoid:

- over-engineering
- speculative abstractions
- unnecessary complexity

At the same time, do not artificially limit the scope of changes. Make the smallest set of changes that completely and cleanly solves the intended problem.

## 5. Verify Before Completion

Before considering work complete:

- verify important behavior
- verify documentation consistency
- run appropriate tests when practical
- honestly report remaining limitations and uncertainties

Never claim verification that was not actually performed.
