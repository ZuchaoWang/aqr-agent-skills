---
name: aqr-content-criteria
description: Provides content criteria for common project documentation types and universal code quality principles. Use when writing or reviewing design docs (system architecture, module, UI/frontend, visualization), interface docs (external API, module public surface), usage scenarios, mission/roadmap/tech-stack/migration docs, research docs (background, related works, brainstorm), dataset docs, or when applying baseline code quality rules.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for common documentation types and for code. Criteria the agent applies when writing or reviewing.

## Reference files to look at

A task may match several files; load the ones whose doc type fits.

| Doc / code type | What it covers | Reference |
| - | - | - |
| Design doc | module decomposition, workflow, style, visual encoding, key algorithm, database schema | `reference/design.md` |
| Interface doc | frontend/backend API, module's public functions and classes | `reference/interface.md` |
| Usage scenarios | user goal, steps, system response, observable behavior | `reference/usage-scenarios.md` |
| Project docs | mission goal and scope, roadmap milestones, tech-stack rationale, migration old→new mapping, active domain concepts | `reference/project.md` |
| Research docs | domain background, related-work comparison, design-option brainstorm | `reference/research.md` |
| Dataset doc | data acquisition, processing and description | `reference/dataset.md` |
| Code | general coding style regardless of language | `reference/code-criteria.md` |
