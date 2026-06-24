---
name: aqr-content-criteria
description: Provides content criteria for common project documentation types and universal code quality principles. Use when writing or reviewing design docs (system architecture, module, UI/frontend, visualization), interface docs (external API, module public surface), usage scenarios, mission/roadmap/tech-stack/migration docs, research docs (background, related works, brainstorm), dataset docs, or when applying baseline code quality rules.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for common documentation types and for code, independent of any project or layout. These are criteria the agent applies when writing or reviewing; they are not copied into the project.

## Scope

- Content criteria for common doc types: design, interface, usage scenarios, mission, roadmap, tech stack, migration, research, dataset.
- Universal code quality principles that apply regardless of language or project style.

This skill does **not** define:

- Where docs live in a project or which files to create — that is a layout choice.
- Project-specific style choices (formatting tools, linting config, framework mechanics) — those live in the project's own rule files.
- Development workflow.

## When to apply

This skill is a router. Identify which doc type you are writing or reviewing, then load the matching reference doc and apply its criteria. Where a doc lives in the project and how it is formatted are layout and style choices outside this skill.

### Docs

- **Design doc** → `reference/design.md`. Find the case in §1 for the section list (system architecture, non-visual module, UI/frontend, visualization), then apply the criteria in §2.
- **Interface doc** → `reference/interface.md` — external API or module public surface.
- **Usage scenarios doc** → `reference/usage-scenarios.md`.
- **Mission, roadmap, tech-stack, migration docs, and active concept set** → `reference/project.md`.
- **Research doc** (background, related works, brainstorm) → `reference/research.md`.
- **Dataset doc** → `reference/dataset.md`.

Every design doc and interface doc carries two things: the criteria of good design/contract, and a recorded set of the decisions behind it (the decision-record format defined in `reference/design.md` §2.4, reused by `reference/interface.md`).

### Code

- When writing or reviewing code: load `reference/code-criteria.md` for baseline principles first, then apply the project's per-stack style rules on top.
- When reviewing docs or code written by others: use the matching reference doc as the checklist.

## Reference docs

- `reference/design.md` — contents by case (system architecture, non-visual module, UI, visualization) in §1, and design criteria (universal decomposition, UI, visualization, decision recording) in §2.
- `reference/interface.md` — universal contract criteria with an external API addendum and a module public-surface addendum.
- `reference/usage-scenarios.md` — usage scenarios criteria.
- `reference/project.md` — mission, roadmap, tech stack, migration, and active concepts.
- `reference/research.md` — background (full) and minimal stubs for related works and brainstorm.
- `reference/dataset.md` — dataset criteria.
- `reference/code-criteria.md` — universal code quality principles.
