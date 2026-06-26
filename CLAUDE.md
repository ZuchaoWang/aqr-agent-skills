# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This repository holds custom, opinionated AI-coding skills. Each skill is a self-contained directory with a `SKILL.md`, a `templates/` tree of starting-point files, and a `reference/` tree of explanatory docs. The skills are designed to compose with execution methodology skills (e.g. Superpowers) but are not tied to any specific one.

All skills in this repo are **doc only**. No skill ships executable code. Their effect comes from being read and applied.

## Layout

```
skills/<skill-name>/
  SKILL.md              # skill definition: YAML frontmatter + body
  templates/            # starting-point files copied into target projects
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

- `aqr-project-blueprint` — Scaffolds or compares a project against the recommended project/doc/code rule blueprint. Defines project shape: root files, docs layout, code/doc rules. Does not mention issues or workflow.
- `aqr-doc-criteria` — Content criteria for every standard doc type in the blueprint layout (design, interface, project, research, dataset). Applied when writing or reviewing docs; criteria are not copied into the project.
- `aqr-code-criteria` — Universal code quality principles that apply regardless of language or stack. Applied when writing or reviewing code; criteria are not copied into the project.
- `aqr-project-principle` — Working principles that define quality standards for any work on a project: clarify before committing, documentation as project memory, optimize for human review, keep the solution simple, verify before completion. Applied during work; not copied into the project.

All four skills are auto-invocable — the host may invoke any of them based on context, and the user can also name one directly via slash command.

The split is deliberate: project shape (`aqr-project-blueprint`), doc content (`aqr-doc-criteria`), code content (`aqr-code-criteria`), and working principles (`aqr-project-principle`) are independent concerns. A project can adopt any combination.

## Conventions inside skill files

- Markdown files start with a top-level heading (`# Title`), no `Status:` header. Lifecycle signals live in filesystem location (e.g. `docs/archived/`) or git history, not in a header.
- Templates use HTML comments (`<!-- ... -->`) as inline guidance for the user filling them in. Comments do not render in markdown preview and can be stripped before commit.
- Reference docs use numbered headings and `-` bullets. No `---` horizontal separators.
- `SKILL.md` files use YAML frontmatter — they are skill definitions, distinct from project docs.

## Editing skills

When editing a skill:

1. Read the existing `SKILL.md` first to understand the skill's scope and invocation policy.
2. Match the conventions of existing files in the same skill (numbered headings, `-` bullets, no `---`, no `Status:` header).
3. Cross-check the split: `aqr-project-blueprint` covers layout only; `aqr-doc-criteria` covers doc content quality only; `aqr-code-criteria` covers code content quality only; `aqr-project-principle` covers working standards only. None should overlap.
4. If a template or reference change affects invocation behavior, update `SKILL.md` accordingly.

## Installing a skill

These skills are source; installation is by copy or symlink into the host agent's skills directory. For Claude Code:

```
~/.claude/skills/<skill-name>/   # user-level skills
<project>/.claude/skills/<skill-name>/   # project-level skills
```

Copy the skill directory verbatim. The host agent reads `SKILL.md` to decide when to invoke; nothing else needs configuration.

## Verifying changes

There is no test suite. Verification is by inspection:

- `find skills -type f | sort` — check the file tree is intact.
- `grep -rn "^Status:" skills/` — should return nothing. Markdown files start with a `#` title, not a `Status:` header.
- `grep -n "^---$" skills/*/SKILL.md` — confirm every SKILL.md has YAML frontmatter (two `---` lines).
- Read each `SKILL.md` end-to-end before considering the skill ready.
