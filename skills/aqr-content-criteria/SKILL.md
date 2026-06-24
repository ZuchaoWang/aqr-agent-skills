---
name: aqr-content-criteria
description: Provides content criteria for project documentation and universal code quality principles. Use when writing or reviewing any doc in the standard blueprint layout (design, interface, usage_scenarios, mission, roadmap, tech_stack, migration, background, dataset) or when applying baseline code quality rules that are not project-style choices. Companion to aqr-project-blueprint (layout) and aqr-issue-records (workflow).
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for the standard doc types and for code, independent of any project. These are criteria the agent applies when writing or reviewing; they are not copied into the project.

## Scope

- Content criteria for every doc type in the recommended docs layout.
- Universal code quality principles that apply regardless of language or project style.

This skill does **not** define:

- Project layout or which files to create — see `aqr-project-blueprint`.
- Project-specific style choices (formatting tools, linting config, framework mechanics) — those live in the project's `docs/rules/` files.
- Development workflow — see `aqr-issue-records`.

## When to apply

This skill is a router. Identify what you are writing or reviewing, then load the matching reference doc. Markdown format rules live in the project's `docs/rules/doc_markdown.md`; layout (which file goes where) lives in `aqr-project-blueprint`.

### Docs

Map the doc path to a reference file:

- `architecture/design.md` and `implementation/[{{layer}}/]{{module_name}}/design.md` → `reference/design.md`. Find the case in §1 for the section list, then apply the criteria in §2:
  - System-level, multiple components, cross-cutting data and workflow → §1.2 system architecture. Apply §2.1.
  - A backend or source module (submodules, data model, algorithms) → §1.3 non-visual module. This is the default for `implementation/.../design.md`. Apply §2.1.
  - A frontend (component tree, state, rendering boundaries) → §1.4 UI. Apply §2.1 and §2.2.
  - A single chart or visualization component → §1.5 visualization. Apply §2.1 and §2.3.
  - A given `design.md` may combine cases — include the sections from each case that applies. When an `implementation/.../design.md` describes a visualization, use §1.5 instead of §1.3.
  - Record decisions per §2.4 in every design.md.
- `architecture/interface.md` and `implementation/[{{layer}}/]{{module_name}}/interface.md` → `reference/interface.md`. Use the external API addendum for `architecture/interface.md` and the module public-surface addendum for `implementation/.../interface.md`.
- `project/usage_scenarios.md` → `reference/usage-scenarios.md`.
- `project/mission.md`, `project/roadmap.md`, `architecture/tech_stack.md`, `project/migration/{{old_project_name}}.md`, and the project's active concept set → `reference/project.md`.
- `research/background.md`, `research/related_works.md`, `research/brainstorm.md` → `reference/research.md`.
- `data/{{dataset}}.md` → `reference/dataset.md`.

Every `design.md` and `interface.md` must carry two things: the criteria of good design/contract, and a recorded set of the decisions behind it (the decision-record format defined in `reference/design.md` §2.4, reused by `interface.md`).

### Code

- When writing or reviewing code: load `reference/code-criteria.md` for baseline principles first, then apply the project's per-stack rule files (`docs/rules/frontend_js.md`, `docs/rules/backend_python.md`, `docs/rules/backend_jupyter.md`) on top.
- When reviewing docs or code written by others: use the matching reference doc as the checklist.

## Reference docs

- `reference/design.md` — contents by case (system architecture, non-visual module, UI, visualization) in §1, and design criteria (universal decomposition, UI, visualization, decision recording) in §2.
- `reference/interface.md` — criteria for every `interface.md` (the contract between design and code), with an external API addendum and a module public-surface addendum.
- `reference/usage-scenarios.md` — criteria for `project/usage_scenarios.md`.
- `reference/project.md` — criteria for `mission.md`, `roadmap.md`, `tech_stack.md`, `migration/{{old_project_name}}.md`, and the project's active concept set.
- `reference/research.md` — criteria for `background.md` (full) and minimal stubs for `related_works.md` and `brainstorm.md`.
- `reference/dataset.md` — criteria for `data/{{dataset}}.md`.
- `reference/code-criteria.md` — universal code quality principles.
