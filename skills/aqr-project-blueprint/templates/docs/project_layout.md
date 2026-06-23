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
    {{module}}/           # one folder per top-level source module — see §3
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

Under `docs/implementation/`, create one folder per top-level source module. Each module folder ships two docs: `design.md` (conceptual review) and `interface.md` (file and signature contract). These are load-bearing — they are how a reviewer understands the module without reading the code.

### 3.1 design.md

Purpose: a reviewer reads it in ~10 minutes and understands the module's role, internal structure, data, algorithms, and testing approach.

Sections (omit any that do not apply):

1. **Summary** — one paragraph: what this module does and what it explicitly does not do; its conceptual role in the larger system.
2. **Submodules and interactions** — for each submodule, a one-line role and how it interacts with the others. Omit if the module is flat.
3. **Data model** — persistent structures at a conceptual level (entity names, relationships, key fields). Not a full SQL DDL. Omit if the module has no persistent state.
4. **Key algorithms** — brief pseudocode for any non-obvious algorithm. Only the ones a reviewer needs to understand the design. Omit if the module is straightforward.
5. **Testing approach** — how this module should be tested: behaviors to cover, edge cases that matter, what to mock vs. use real. Conceptual, not a list of test cases.

Constraints: target ~1 page. Conceptual only — implementation detail lives in code. Must be reviewable without reading the code.

### 3.2 interface.md

Purpose: a reviewer reads it in ~5 minutes and spots missing pieces, wrong shapes, unclear naming, over-coupling.

Sections:

1. **Files** — one-line role per file in the module.
2. **Public surface** — for each public function / class / type / endpoint: signature on one line, one-sentence role on the next. Skip internal helpers, trivial accessors, and full type bodies (point at the file instead).

Constraints: target ~1 page for the whole module. The one-sentence role under each signature is the load-bearing piece.

## 4. Layout rules

- `docs/` is spec-driven: code is written to satisfy documented specifications, not the other way around.
- `issues/` holds issue records in the `aqr-issue-records` format. One directory per issue, grouped by `YYYYMM`.
- `tmp/` is local scratch; nothing in `tmp/` is committed.
