---
name: aqr-project-blueprint
description: Scaffold a new project or compare an existing repo against the recommended project/doc/code rule blueprint. Use only when explicitly invoked — this skill is opinionated and is not applied automatically.
disable-model-invocation: true
---

# aqr-project-blueprint

Provides a reusable project scaffold and alignment guide. Recommends root files, docs layout, rule docs, and code/doc style defaults. Used to scaffold a new project or compare an existing project against the recommended structure.

## Scope

This skill defines **project shape and rules only**. It does not define the development workflow.

## What this skill is for

Use it when explicitly asked to:

- Scaffold a new project from the recommended blueprint.
- Add recommended project docs or rules to an existing repo.
- Compare an existing repo against the recommended blueprint and report drift.
- Propose a migration plan from an existing layout to the blueprint.

## What this skill does not do

- Its files are **not** authoritative for any project. They are starter material; once copied into a project, the project-local copies are ground truth and may be edited, overridden, or deleted. The skill does not re-impose its defaults on a project that has diverged.
- It does **not** rewrite existing project structure automatically. For existing projects, first produce a comparison and a proposal; the user decides what to apply.
- It does **not** create technology-specific files without first asking or inferring. See "Inferring tech choices" below.
- It does **not** define the development workflow. Pair with `aqr-issue-records` and an execution skill (e.g. Superpowers) for that.

## Recommended root files

Copied verbatim from `templates/` into the project root when scaffolding:

```
README.md
CLAUDE.md
.editorconfig
.gitignore
.gitattributes
.markdownlint.json
```

Technology-specific files are added only after the relevant choice is known:

```
.nvmrc                    # when the project uses Node
.python-version           # when the project uses Python
package.json              # when the project uses Node
pyproject.toml            # when the project uses Python
```

See `reference/root-files.md` for the role of each file and when to include or skip it.

## Recommended docs structure

```
docs/
  index.md                # documentation map
  project-layout.md       # repo layout guide

  project/
    overview.md
    roadmap.md

  architecture/
    overview.md

  implementation/
    overview.md

  research/
    overview.md

  data/
    overview.md

  rules/
    code-style.md
    documentation.md
    frontend-react.md
    backend-python.md
    testing.md
```

See `reference/docs-layout.md` for the meaning of each directory and what belongs in each.

## Inferring tech choices

Before creating technology-specific files, confirm or infer each of the following. Default to **asking** when starting from a blank slate; default to **inferring from existing files** when extending an existing repo.

- Python version (write to `.python-version` and `pyproject.toml`)
- Node version (write to `.nvmrc` and `package.json` `engines`)
- Frontend framework (default: React + TypeScript)
- Backend framework (default: FastAPI for Python services)
- Package manager for Node (npm / pnpm / yarn)
- Python package manager (pip / uv / poetry / conda)
- Monorepo layout (single tree vs. `frontend/` + `backend/`)
- Lint / format / type-check tools

Reference docs for the common stacks:

- `reference/python-project.md` — `pyproject.toml`, `.python-version`, requirements files, ruff/pyright config
- `reference/node-project.md` — `package.json`, `.nvmrc`, lockfiles, ESLint/Prettier config
- `reference/backend-python.md` — FastAPI + Pydantic rules
- `reference/frontend-react.md` — React + TypeScript + Vite rules

## Scaffolding workflow

When invoked to scaffold a new project:

1. Ask the user the tech-choice questions above (or accept their stated answers).
2. Copy the root files from `templates/` to the project root.
3. Copy the docs skeleton from `templates/docs/` to `<project>/docs/`.
4. Add technology-specific files for each confirmed stack.
5. Stop. Do not populate the docs skeleton with substantive content — that is later doc-update work, not scaffolding.

When invoked to compare an existing repo:

1. Read the existing root files and docs tree.
2. Map each existing artifact to a blueprint slot.
3. Report three buckets: **matches** (blueprint satisfied), **drift** (blueprint recommends something different), **extra** (project has files the blueprint does not mention).
4. Propose changes; do not apply them unless the user asks.

## After scaffolding

Once scaffolding completes, the project-local files are ground truth. In practice this means:

- Do not re-copy skill templates over existing project files.
- Do not "fix" drift by editing project files to match the skill.
- If the user later asks to compare against the blueprint, produce a drift report — do not apply changes unless asked.
