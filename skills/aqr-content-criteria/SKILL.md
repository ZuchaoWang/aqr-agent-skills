---
name: aqr-content-criteria
description: Provides content criteria for common project documentation types and universal code quality principles. Use when writing or reviewing design docs (system architecture, module, UI/frontend, visualization), interface docs (external API, module public surface), usage scenarios, mission/roadmap/tech-stack/migration docs, research docs (background, related works, brainstorm), dataset docs, or when applying baseline code quality rules.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for common documentation types and for code. Criteria the agent applies when writing or reviewing; not copied into the project. Does not define layout, project-specific style, or workflow.

## Router

Identify what you are writing or reviewing, load the reference doc, apply its criteria.

| Doc / code type | Examples | Reference |
| - | - | - |
| Design doc | system architecture; non-visual module; UI/frontend; visualization | `reference/design.md` |
| Interface doc | external API contract; module public surface | `reference/interface.md` |
| Usage scenarios | user-facing situations the system must support | `reference/usage-scenarios.md` |
| Project docs | mission; roadmap; tech stack; migration; active concepts | `reference/project.md` |
| Research docs | background; related works; brainstorm | `reference/research.md` |
| Dataset doc | one per dataset | `reference/dataset.md` |
| Code | any language, baseline floor | `reference/code-criteria.md` |
