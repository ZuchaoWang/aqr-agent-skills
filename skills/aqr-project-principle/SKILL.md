---
name: aqr-project-principle
description: Use when doing any non-trivial work on a project — making changes, writing or updating docs, investigating. Defines the working principles and quality standards the work should meet.
disable-model-invocation: false
---

# aqr-project-principle

These principles define the quality standards for work on a project. They describe **what good work looks like**, not a fixed workflow — the agent may choose any execution strategy that satisfies them.

## 1. Batch Requirements

Confirm the complete task scope before planning.

- ask whether more requirements remain
- encourage related requirements to be submitted together
- hold planning until the scope is confirmed

Proceed without confirmation only when explicitly instructed.

## 2. Clarify Before Committing

Never silently invent important requirements or design decisions. Clarifying means both asking about specific uncertainties and confirming the spec or plan before acting on it.

Before making significant semantic or architectural changes:

- clarify ambiguous requirements
- surface important assumptions and trade-offs
- confirm the spec or plan, and obtain agreement when the change is significant

Once clarified, announce that autonomous execution is beginning. Interrupt only for unforeseen blockers or decisions that materially affect the solution.

## 3. Documentation Is the Project Memory

Project documentation records durable project knowledge.

Documentation should contain long-lived information such as:

- architecture decisions
- interface contracts
- module responsibilities

Follow documented decisions unless intentionally changing them.

Do not preserve temporary reasoning or planning unless it provides long-term value.

## 4. Optimize Documentation for Human Review

Documentation exists primarily for human review.

Therefore:

- keep documents concise
- write only information worth reviewing
- prefer updating existing documents over creating new ones

Every document should remain practical to review.

## 5. Keep Solution Simple

Prefer the simplest complete solution.

Avoid:

- over-engineering
- speculative abstractions
- unnecessary complexity

At the same time, do not artificially limit the scope of changes. Make the smallest set of changes that completely and cleanly solves the intended problem.

## 6. Verify Before Completion

Before considering work complete:

- verify important behavior
- verify documentation consistency
- run appropriate tests when practical
- honestly report remaining limitations and uncertainties

Never claim verification that was not actually performed.
