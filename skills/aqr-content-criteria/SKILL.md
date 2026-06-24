---
name: aqr-content-criteria
description: Provides content criteria for project documentation and universal code quality principles. Use when writing or reviewing any doc in the standard blueprint layout (design, interface, usage_scenarios, research, dataset, migration) or when applying baseline code quality rules that are not project-style choices. Companion to aqr-project-blueprint (layout) and aqr-issue-records (workflow).
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for the standard doc types and for code, independent of any project. These are criteria the agent applies when writing or reviewing; they are not copied into the project.

## Scope

- Content criteria for every doc type in the recommended docs layout.
- Universal code quality principles that apply regardless of language or project style.

This skill does **not** define:

- Project layout or which files to create — see `aqr-project-blueprint`.
- Project-specific style choices (formatting tools, linting config) — those live in the project's `docs/rules/` files.
- Development workflow — see `aqr-issue-records`.

## When to apply

- When writing or updating any doc in `docs/`: use `reference/doc-criteria.md` for the relevant doc type.
- When writing or reviewing code: use `reference/code-criteria.md` for baseline principles before applying project-specific style rules.
- When reviewing docs or code written by others: use both reference docs as the checklist.

## Reference docs

- `reference/doc-criteria.md` — content criteria for every standard doc type.
- `reference/code-criteria.md` — universal code quality principles.
