---
name: aqr-content-criteria
description: Provides content criteria for common project documentation types and universal code quality principles. Use when writing or reviewing design docs (system architecture, module, UI/frontend, visualization), interface docs (external API, module public surface), usage scenarios, mission/roadmap/tech-stack/migration docs, research docs (background, related works, brainstorm), dataset docs, or when applying baseline code quality rules.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for common documentation types and for code. Criteria the agent applies when writing or reviewing.

## Reference files to look at

Identify what you are writing or reviewing, load the matching reference doc(s), apply their criteria. A task may match several.

- **Design doc** → `reference/design.md`. Any doc describing how a system or unit is structured. §1 picks the section list by case — system architecture, non-visual module, UI/frontend, or visualization — and §2 gives the criteria. A single design doc may combine cases.

- **Interface doc** → `reference/interface.md`. A contract between a system/module and its callers: external API (endpoints, shapes, error envelope, versioning, pagination, idempotency) or module public surface (files, signatures, what is hidden). Reuses the decision-record format from `reference/design.md` §2.4.

- **Usage scenarios** → `reference/usage-scenarios.md`. Concrete user-facing situations as observable behavior, one per scenario: situation, what the user does, what the system does in response.

- **Project docs** → `reference/project.md`. Lighter what-and-why docs: mission, roadmap, tech stack, migration, and active concepts (the complement to background).

- **Research docs** → `reference/research.md`. Docs that inform decisions, not specs: background (full), related works (minimal), brainstorm (minimal). Archive once conclusions fold into a design or implementation doc.

- **Dataset doc** → `reference/dataset.md`. One per dataset: contents, source and provenance, schema, format, quality notes, usage — enough to decide use without opening it.

- **Code** → `reference/code-criteria.md`. Universal code-quality floor for any language. Apply first, then layer the project's per-stack style rules.
