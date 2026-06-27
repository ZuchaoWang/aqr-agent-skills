---
name: aqr-project-blueprint
description: Reference for the recommended project shape — root files and docs layout. Tells what files a project should have and where; not how to write them. Use when starting a new project, laying out its root files or docs tree, deciding where a file belongs, or checking a repo's shape for drift.
disable-model-invocation: false
---

# aqr-project-blueprint

A reference for the recommended project shape: which root files a project should carry and how its `docs/` tree is laid out. It describes the shape — it does not prescribe how to write the content. For content quality, pair with the other skills: `aqr-doc-criteria` (what each doc type should contain), `aqr-code-criteria` (universal code quality floor), and `aqr-style-rules` (opinionated code, test, notebook, and doc style taste).

## Scope

This skill defines **project shape only** — what files exist and where. It does not define doc content quality, code quality, style taste, or development workflow.

## What this skill is for

Use it to:

- Lay out a new project's root files and docs tree from the recommended shape.
- Decide where a given doc or file belongs.
- Compare an existing repo against the recommended shape and report drift.

The recommended shape is a reference, not authority for a project that has diverged. When comparing, report drift; do not rewrite a project's structure unless asked.

## Recommended root files

Stack-agnostic files recommended for every project:

```
README.md                # entry pointer: what the project is and how to start; points at docs/index.md
CLAUDE.md                # top-level instructions for AI coding agents; carries toolchain specifics
.editorconfig            # editor-agnostic style defaults: line endings, encoding, indent per language
.gitignore               # common ignore patterns: local env, editor state, caches, build output
.gitattributes           # forces LF on checkout; marks binary file types so they are not diffed as text
.markdownlint.json       # disables markdownlint rules incompatible with the project doc style
```

Technology-specific files are added once the relevant choice is known:

```
.nvmrc                    # Node version pin (single line, no `v` prefix) — when the project uses Node
.python-version           # Python version pin — when the project uses Python
package.json              # npm manifest — when the project uses Node
pyproject.toml            # Python project metadata, build, and tool config — when the project uses Python
```

## Recommended docs structure

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory
  project_layout.md       # top-level repo layout, one-line annotation per directory

  project/
    mission.md            # what the project is for; problem statement and scope
    usage_scenarios.md    # concrete user-facing scenarios the project must support
    roadmap.md            # milestones, target dates, owners
    client_docs/          # verbatim requirements and feedback from the client
      {{date}}/           # snapshot of client materials received on that date
    migration/            # notes for migrating from a prior project
      {{old_project_name}}.md  # what carried over and what changed from the prior project

  architecture/
    design.md             # system-level design: components, data flow, key decisions
    interface.md          # external API contract: endpoints, request/response, error envelope
    deploy.md             # deployment topology, runtime environment, ops notes
    tech_stack.md         # languages, frameworks, libraries, and rationale

  research/
    background.md         # background knowledge for concepts and motivations
    related_works.md      # existing works related
    brainstorm.md         # discussion of possible designs

  implementation/
    # Two layouts, chosen based on source-tree shape:

    {{module_name}}/      # flat — single-stack projects
      design.md           # module-level design: components, data flow
      interface.md        # module file and signature contract

    {{layer}}/            # layered — multi-stack projects (e.g., frontend/, backend/)
      {{module_name}}/    # one folder per top-level source module
        design.md         # module-level design: components, data flow
        interface.md      # module file and signature contract

  data/
    {{dataset}}.md        # one doc per dataset
```

Not every project needs every file; add what applies. Content criteria for each doc type (design, interface, project, research, dataset) live in `aqr-doc-criteria`.
