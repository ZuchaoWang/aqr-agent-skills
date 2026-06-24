# Docs layout

The recommended `docs/` structure. The SKILL.md tree is the canonical reference; this doc explains the role of each section so the scaffolding agent can answer "does this doc belong here or somewhere else?" without re-reading the tree.

The skeleton ships a starter subset (a few files per section); the rest fill in as the project grows. Two subtrees — `implementation/[{{layer}}/]{{module_name}}/` and `data/{{dataset}}.md` — are created on demand, not scaffolded with placeholder files. Criteria for those live in the `aqr-content-criteria` skill.

## 1. Top-level files

### 1.1 docs/index.md

Documentation map. One section per top-level `docs/` subdirectory, listing each doc with a one-line role. New readers start here. Update this file whenever a doc is added, removed, or moves.

### 1.2 docs/project_layout.md

Top-level repo layout (one-line annotation per directory) plus a pointer to the `aqr-content-criteria` skill for module-doc criteria. Update when the top-level shape changes.

## 2. docs/project/

Project-level docs: mission, problem statement, scope, roadmap, milestones, client requirements, migration notes from prior projects. The "what and why" of the project as a whole, not any individual feature or issue.

Skeleton ships: `mission.md`, `roadmap.md`. Add per-project docs (`usage_scenarios.md`, ...) as the project grows. Verbatim client materials live under `client_docs/{{date}}/`; notes for migrating from a prior project live under `migration/{{old_project_name}}.md` (criteria in `aqr-content-criteria` §1.4).

## 3. docs/architecture/

System-level architecture: component boundaries, data flow, major design decisions. The shape of the system, not the shape of any single module. Per-module implementation detail lives under `implementation/`.

Skeleton ships: `design.md`. Common additions: `interface.md` (external API contract), `deploy.md` (deployment topology), `tech_stack.md` (language / framework / library rationale).

## 4. docs/implementation/

Per-module implementation reference. One folder per top-level source module; each folder ships `design.md` (conceptual review) and `interface.md` (file and signature contract).

Two layouts, picked at scaffolding time based on source-tree shape — both are first-class:

```
implementation/
  {{module_name}}/          # flat — single-stack projects
    design.md
    interface.md

  {{layer}}/                # layered — multi-stack projects (e.g., frontend/, backend/)
    {{module_name}}/
      design.md
      interface.md
```

The criteria for both docs live in `aqr-content-criteria` §§4.1–4.2. The skeleton ships the directory empty; the user adds a folder per module.

## 5. docs/research/

Background research, related-work comparisons, exploratory notes. Research docs inform decisions; they are not specs. Once a research note's conclusions are folded into `architecture/` or `implementation/`, move the note under `docs/archived/research/` and have the new doc reference it.

The skeleton ships the directory empty; the user adds a doc per topic (`background.md`, `related_works.md`, `brainstorm.md`, ...).

## 6. docs/data/

Per-dataset documentation. One `.md` per dataset. Criteria in `aqr-content-criteria` §5. The skeleton ships the directory empty; the user adds a doc per dataset.

## 7. docs/rules/

The project's standing rules. Files at scaffold time:

- `doc_markdown.md` — markdown doc style: headings, lists, lifecycle, archival.
- `report_ppt.md` — conventions for PowerPoint reports. Include only if the project ships decks.
- `frontend_js.md` — code + test rules for a JS / TS frontend (any framework). Include only if the project has a frontend.
- `backend_python.md` — code + test rules for a Python backend. Include only if the project has a Python service.
- `backend_jupyter.md` — code + test rules for a Jupyter backend. Include only if the project ships notebooks as a backend.

Content criteria for dynamic doc types (module design/interface, dataset, migration) live in the `aqr-content-criteria` skill — not in a project-local file.

## 8. When to add a new top-level section

Default: do not. The sections above cover the common cases. If a project feels it needs another (e.g., `docs/security/`, `docs/operations/`), the right move is usually:

1. Check whether the content actually belongs under an existing section (security notes often belong under `architecture/`; runbooks often belong under `implementation/`).
2. If it genuinely does not fit, add the new section, update `index.md` and `project_layout.md`, and add an entry here explaining the rationale.

## 9. What does not belong in docs/

- Issue records. Those live under `issues/`. `docs/` references issues by directory path when needed.
- Scratch notes, drafts, half-formed ideas. Those go in `tmp/` or in a personal notes repo.
- Generated API references. Those go in `implementation/[{{layer}}/]{{module_name}}/interface.md`, written by hand; do not commit generated HTML.
