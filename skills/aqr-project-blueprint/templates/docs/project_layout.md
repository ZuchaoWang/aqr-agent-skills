# Project layout

Top-level layout of this repository and the criteria for per-module implementation docs. Update this doc whenever the top-level shape changes.

## 1. Top-level layout

One-line annotation per top-level directory and root file.

```
{{root tree here, e.g.:

backend/                # Python service
  src/
  tests/
frontend/               # React + TypeScript client
  src/
docs/                   # Documentation — start at docs/index.md
issues/                 # Issue records (one directory per issue, grouped by YYYYMM)
tmp/                    # Local scratch; not committed
}}
```

## 2. docs/ layout

One-line role per file and folder under `docs/`.

```
docs/
  index.md                # documentation map; links to every other doc
  project_layout.md       # this file — repo layout and module-doc criteria

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

  implementation/
    # Two layouts, picked at scaffolding time based on source-tree shape — see §3:

    {{module_name}}/      # flat — single-stack projects
      design.md
      interface.md

    {{layer}}/            # layered — multi-stack projects (e.g., frontend/, backend/)
      {{module_name}}/    # one folder per top-level source module
        design.md
        interface.md

  data/
    {{dataset}}.md        # one per dataset

  rules/
    code_general.md       # cross-language code rules; per-stack files layer on top
    doc_markdown.md       # markdown doc style: headings, lists, lifecycle, archival
    report_ppt.md         # conventions for PowerPoint reports the project produces
    frontend_js.md        # code + test rules for a JS/TS frontend (any framework)
    backend_python.md     # code + test rules for a Python backend
    backend_jupyter.md    # code + test rules for a Jupyter backend
```

## 3. Module doc criteria

Each module folder under `implementation/` (path: `{{module_name}}/` or `{{layer}}/{{module_name}}/`) ships two docs — `design.md` (conceptual review) and `interface.md` (file and signature contract). Content criteria for both live in the `aqr-content-criteria` skill.

## 4. Layout rules

- `docs/` is spec-driven: code is written to satisfy documented specifications, not the other way around.
- `issues/` holds issue records in the `aqr-issue-records` format. One directory per issue, grouped by `YYYYMM`.
- `tmp/` is local scratch; nothing in `tmp/` is committed.
