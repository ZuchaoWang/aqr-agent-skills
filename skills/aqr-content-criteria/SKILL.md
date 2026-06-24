---
name: aqr-content-criteria
description: Provides content criteria for common project documentation types and universal code quality principles. Use when writing or reviewing design docs (system architecture, module, UI/frontend, visualization), interface docs (external API, module public surface), usage scenarios, mission/roadmap/tech-stack/migration docs, research docs (background, related works, brainstorm), dataset docs, or when applying baseline code quality rules.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for common documentation types and for code. Criteria the agent applies when writing or reviewing.

## Router

Identify what you are writing or reviewing, load the matching reference doc, apply its criteria.

- **Design doc** → `reference/design.md`. Covers any doc that describes how a system or unit is structured. Find the case in §1 for the section list, then apply the criteria in §2.
  - *System architecture* — the whole system: major components, their boundaries, data flow, control and workflow flow, and the architecture-shaping decisions. Typical for a system-level design.
  - *Non-visual module* — a single backend or source module: submodules and interactions, data model, key algorithms, testing approach. The default for a per-module design.
  - *UI / frontend* — a frontend app or module: component tree and decomposition, presentational vs. container responsibility, state ownership, prop discipline, interaction boundaries.
  - *Visualization* — a single chart or visualization component: data shape to chart-type mapping, marks and channels, visual encoding, accessibility, when a table beats a chart. Use instead of non-visual module when the module is itself a viz.
  - A single design doc may combine cases — include the relevant sections from each.

- **Interface doc** → `reference/interface.md`. Covers any doc that states a contract between a system and its callers, or between a module and its callers. Sits between design and code; both must honor it.
  - *External API* — endpoints and entry points, request/response shapes, error envelope, versioning, pagination, idempotency, naming, auth. Addendum §2.
  - *Module public surface* — files with one-line roles, public signatures with a one-sentence role each, what is deliberately hidden. Addendum §3.
  - Every interface doc also records the decisions behind its contract (envelope shape, what to expose) using the decision-record format from `reference/design.md` §2.4.

- **Usage scenarios** → `reference/usage-scenarios.md`. Concrete user-facing situations the system must support, described as observable behavior — e.g. "a researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds," not "support large files." One subsection per scenario: situation, what the user does, what the system must do in response.

- **Project docs** → `reference/project.md`. The lighter, what-and-why docs at the project level:
  - *Mission* — goal, problem statement, scope (in/out), stakeholders.
  - *Roadmap* — milestones (referenced, not re-described), decisions log.
  - *Tech stack* — languages/runtimes, frameworks/libraries and why chosen, toolchain.
  - *Migration* — what carried over, what was dropped and why, old→new mapping, rationale.
  - *Active concepts* — domain concepts the project actively uses, each with a definition and where it shows up (complement to background).

- **Research docs** → `reference/research.md`. Docs that inform decisions but are not specs; archive once conclusions fold into a design or implementation doc.
  - *Background* (full) — domain knowledge and motivations, including concepts considered but not used.
  - *Related works* (minimal) — landscape summary and per-entry relationship (similar, complementary, superseded).
  - *Brainstorm* (minimal) — design question, options with trade-offs, decision or open question.

- **Dataset doc** → `reference/dataset.md`. One doc per dataset: what it contains, source and provenance, schema, format, quality notes, usage. Enough to decide whether and how to use the dataset without opening it.

- **Code** → `reference/code-criteria.md`. Universal code-quality floor for any language: public surface integrity, naming and constants, comments, secrets and config, error handling, file and module hygiene, testing, pre-commit checks. Apply first, then layer the project's per-stack style rules on top.
