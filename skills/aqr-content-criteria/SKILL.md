---
name: aqr-content-criteria
description: Provides content criteria for common project documentation types and universal code quality principles. Use when writing or reviewing design docs (system architecture, module, UI/frontend, visualization), interface docs (external API, module public surface), usage scenarios, mission/roadmap/tech-stack/migration docs, research docs (background, related works, brainstorm), dataset docs, or when applying baseline code quality rules.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for common documentation types and for code. Criteria the agent applies when writing or reviewing.

## Reference files to look at

A task may match several files; load the ones whose doc type fits.

- **Design doc** → `reference/design.md`. A doc describing how something is structured: the system's components and data flow, a backend module's submodules and algorithms, a frontend's component tree and state ownership, or a single chart's visual encoding.

- **Interface doc** → `reference/interface.md`. A contract between a system/module and its callers: an external HTTP API's endpoints and error envelope, or a module's exported functions and what it keeps private.

- **Usage scenarios** → `reference/usage-scenarios.md`. Concrete user-facing situations as observable behavior — e.g. "a researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds."

- **Project docs** → `reference/project.md`. The lighter what-and-why docs: a mission's goal and scope, a roadmap's milestones, a tech-stack rationale, a migration's old→new mapping, or the project's active domain concepts.

- **Research docs** → `reference/research.md`. Docs that inform decisions but are not specs: domain background, related-work comparisons, or a brainstorm of design options.

- **Dataset doc** → `reference/dataset.md`. One per dataset: what it contains, where it came from, its schema, format, and known quality gaps.

- **Code** → `reference/code-criteria.md`. Universal code-quality floor for any language: typing, naming, error handling, hygiene, testing.
