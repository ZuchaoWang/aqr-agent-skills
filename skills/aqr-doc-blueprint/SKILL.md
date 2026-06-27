---
name: aqr-doc-blueprint
description: Reference for the recommended docs layout — the `docs/` tree and the root entry points that route into it. Tells what docs a project should have and where; not how to write them. Use when laying out a project's docs, deciding where a doc belongs, or checking a docs tree for drift.
disable-model-invocation: false
---

# aqr-doc-blueprint

A reference for the recommended docs layout: how a project's `docs/` tree is laid out, plus the root entry points that route readers and agents into it. It describes the layout — it does not prescribe how to write the content. For content quality, pair with the other skills: `aqr-doc-criteria` (what each doc type should contain), `aqr-code-criteria` (universal code quality floor), and `aqr-style-rules` (opinionated code, test, notebook, and doc style taste).

## Scope

This skill defines the **docs layout only** — which docs exist and where, plus the root files that point into them. It does not define doc content quality, code quality, style taste, or development workflow.

## What this skill is for

Use it to:

- Lay out a new project's docs tree from the recommended shape.
- Decide where a given doc belongs.
- Compare an existing docs tree against the recommended shape and report drift.

The recommended shape is a reference, not authority for a project that has diverged. When comparing, report drift; do not rewrite a project's structure unless asked.

## Root entry points

Two root files route readers and agents into the docs:

```
README.md                # entry pointer: what the project is and how to start; points at docs/index.md
CLAUDE.md                # top-level instructions for AI coding agents; carries toolchain specifics
```

Stack-specific root files (version pins, manifests, editor and lint config) are not docs layout — they are per-stack conventions covered by `aqr-style-rules`.

## Recommended docs structure

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory
  project_layout.md       # top-level repo layout, one-line annotation per directory

  project/
    mission.md            # what the project is for; problem statement and scope
    usage_scenarios.md    # concrete user-facing scenarios the project must support
    roadmap.md            # the sequence of milestones: order, target dates, owners

  milestones/             # one file per short-term development goal — the working contract for the next change
    {{index}}-{{name}}.md # what should change: user intent, agreed decisions, acceptance criteria

  client_docs/            # verbatim requirements and feedback from the client
    {{date}}/             # snapshot of client materials received on that date

  migration/              # notes for migrating from a prior project
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

Not every project needs every file; add what applies. Content criteria for each doc type (design, interface, project, research, dataset, milestone) live in `aqr-doc-criteria`.
