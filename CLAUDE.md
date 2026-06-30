# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repository holds custom AI-coding skills — two universal (an engineering and working floor) and three opinionated (doc and style taste). Each skill is a self-contained directory with a `SKILL.md`, and optionally a `reference/` tree of explanatory docs. The skills are designed to compose with execution methodology skills (e.g. Superpowers) but are not tied to any specific one.

All skills in this repo are **doc only**. No skill ships executable code. Their effect comes from being read and applied.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition: YAML frontmatter + body
  reference/            # explanatory docs loaded on demand
```

Each `SKILL.md` starts with YAML frontmatter:

```yaml
---
name: <skill-name>
description: <one-paragraph description used by the host agent to decide invocation>
disable-model-invocation: <true | false | omit>
---
```

`disable-model-invocation: true` marks a skill manual-only (the host agent will not auto-invoke it based on context; the user must name it explicitly). Omit the field or set it to `false` for auto-invocable skills.

## Skills currently in this repo

**Universal** — engineering and working floor, not taste:

- `aqr-code-criteria` — Universal code quality principles that apply regardless of language or stack. Applied when writing or reviewing code; criteria are not copied into the project.
- `aqr-project-principle` — Working principles that define the quality bar for project work: what good work looks like, not a fixed workflow. Applied during work; not copied into the project.

**Opinionated** — taste-based choices:

- `aqr-doc-blueprint` — Reference for the recommended docs layout: the `docs/` tree plus the root entry points that route into it. Describes what docs a project should have and where; not how to write them. Does not mention issues or workflow.
- `aqr-doc-content` — Content criteria for every standard doc type in the blueprint layout (design, project, research, dataset). Applied when writing or reviewing docs; criteria are not copied into the project.
- `aqr-style-rules` — Opinionated, stack-specific style defaults (code, tests, notebooks, presentations, doc formatting) layered on top of the code and doc criteria. Applied when writing or reviewing code, tests, notebooks, or decks; not copied into the project.

All five skills are auto-invocable — the host may invoke any of them based on context, and the user can also name one directly via slash command.

The split is deliberate: docs layout (`aqr-doc-blueprint`), doc content (`aqr-doc-content`), code content (`aqr-code-criteria`), working principles (`aqr-project-principle`), and opinionated style taste (`aqr-style-rules`) are independent concerns. A project can adopt any combination.

## Conventions inside skill files

- Markdown files start with a top-level heading (`# Title`).
- Reference docs use numbered headings and `-` bullets. No `---` horizontal separators.
- `SKILL.md` files use YAML frontmatter — they are skill definitions, distinct from project docs.

## Editing skills

When editing a skill:

1. Read the existing `SKILL.md` first to understand the skill's scope and invocation policy.
2. Match the conventions of existing files in the same skill (numbered headings, `-` bullets, no `---`, no `Status:` header).
3. Cross-check the split: `aqr-doc-blueprint` covers docs layout only; `aqr-doc-content` covers doc content quality only; `aqr-code-criteria` covers code content quality only; `aqr-project-principle` covers working standards only; `aqr-style-rules` covers opinionated style taste on top of the code and doc criteria only. None should overlap.
4. If a reference change affects invocation behavior, update `SKILL.md` accordingly.

## Installing a skill

These skills are opt-in. Install them **per project** — not at user scope — only in projects that want them. For Claude Code, copy or symlink each skill directory into the project's skills folder:

```
<project>/.claude/skills/<skill-name>/
```

Copy the skill directory verbatim. The host agent reads `SKILL.md` to decide when to invoke; nothing else needs configuration.

Skill auto-invocation is unreliable on its own, so the adopting project should also add a directive to its own `CLAUDE.md` telling the agent to use them. Example:

```md
For software-development work, use the AQR skills under `.claude/skills/` and invoke the one that matches the task (do not invoke all of them):

- `aqr-project-principle` — any non-trivial work (the quality bar for the work)
- `aqr-code-criteria` — writing or reviewing source code
- `aqr-doc-content` — writing or reviewing docs (design, project, research, dataset)
- `aqr-doc-blueprint` — laying out or auditing the `docs/` tree
- `aqr-style-rules` — formatting and style for code, tests, notebooks, decks, docs
```

## Verifying changes

There is no test suite. Verification is by inspection:

- `find skills -type f | sort` — check the file tree is intact.
- `grep -rn "^Status:" skills/` — should return nothing. Markdown files start with a `#` title, not a `Status:` header.
- `grep -n "^---$" skills/*/SKILL.md` — confirm every SKILL.md has YAML frontmatter (two `---` lines).
- Read each `SKILL.md` end-to-end before considering the skill ready.
