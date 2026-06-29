---
name: aqr-doc-blueprint
description: Reference for the recommended docs layout — the `docs/` tree and the root entry points that route into it. Tells what docs a project should have and where; not how to write them. Use when laying out a project's docs, deciding where a doc belongs, or checking a docs tree for drift.
disable-model-invocation: false
---

# aqr-doc-blueprint

A recommended docs layout: what docs a project should have and where. It covers layout only — not how to write the content. Use it when creating or moving a doc to decide its name and location, and to see which docs a project needs.

## Root entry points

Two root files route readers and agents into the docs:

```
README.md                # what the project is and how to start; points at docs/index.md
CLAUDE.md                # agent instructions and toolchain specifics; also a brief top-level directory map
```

Top-level repo orientation — what each top-level directory is for — belongs here in CLAUDE.md/README, not in a separate layout doc. Stack-specific root files (version pins, manifests, editor and lint config) are per-stack conventions, outside this layout.

## Recommended docs structure

`docs/index.md` is the ground truth for a project's docs — the map of what actually exists. The shape below is a reference: use it to bootstrap a new docs tree or to audit an existing one for drift. It is not a prescription — do not impose it on a project that has diverged; report drift instead.

```
docs/
  index.md                # documentation map: one section per top-level docs/ subdirectory

  project/
    mission.md            # what the project is for; problem statement and scope
    usage_scenarios.md    # concrete user-facing scenarios the project must support
    roadmap.md            # the sequence of development objectives: order, target dates, owners
    concepts.md           # active domain vocabulary: concepts the project uses, with definitions

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

Not every project needs every file; add what applies.
