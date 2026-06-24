---
name: aqr-content-criteria
description: Criteria for writing source code and common documentation types (design, interface, usage scenarios, project, research, dataset). Use when writing or reviewing any such code for doc.
disable-model-invocation: false
---

# aqr-content-criteria

Defines **what good content looks like** for code and for common documentation types. Criteria the agent applies when writing or reviewing.

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
| Soruce code | general coding style regardless of language | `reference/code-criteria.md` |
