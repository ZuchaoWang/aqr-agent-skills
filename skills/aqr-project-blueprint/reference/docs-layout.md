# Docs layout

The recommended `docs/` structure. Six sections, each with its own role. Projects add per-section docs as the project grows; they do not create new top-level sections without good reason. The skeleton ships a minimal starter set (one or two docs per section); the rest fill up as the project grows.

## 1. Sections

### 1.1 docs/index.md

Documentation map. One section per top-level `docs/` subdirectory, listing each doc with a one-line role. New readers start here. Update this file whenever a doc is added, removed, or moves.

Treat the index as live from the moment the project has more than one doc. An empty index is worse than no index because it implies there is nothing to read.

### 1.2 docs/project/

Project-level docs: mission, problem statement, scope, roadmap, milestones, client requirements. The "what and why" of the project as a whole, not any individual feature or issue.

Skeleton ships: `mission.md`, `roadmap.md`. Add per-project docs (`usage_scenarios.md`, `result.md`, `workflow.md`, ...) as the project grows.

### 1.3 docs/architecture/

System-level architecture: component boundaries, data flow, major design decisions. The shape of the system, not the shape of any single module. Per-module implementation detail lives under `implementation/`.

Skeleton ships: `design.md`. Add per-concern docs (`api.md`, `tech_stack.md`, `deploy.md`, ...) as the project grows.

### 1.4 docs/implementation/

Per-module implementation reference. One `.md` per top-level source module. Each module doc covers: description, files and folders, public surface (with full type signatures), and tests.

This is the layer that "guides implementation by removing ambiguity" — the layer the executing agent reaches for when writing code in a specific module. The skeleton ships the directory empty; the user adds a doc per module.

### 1.5 docs/research/

Background research, related-work comparisons, exploratory notes. Research docs inform decisions; they are not specs. Once a research note's conclusions are folded into `architecture/` or `implementation/`, move the note under `docs/archived/research/` and have the new doc reference it.

The skeleton ships the directory empty; the user adds a doc per topic (`background.md`, `related_work.md`, `approach_rationale.md`, ...).

### 1.6 docs/data/

Per-dataset documentation. One `.md` per dataset. Each dataset doc covers: source, schema, refresh cadence, known issues.

The skeleton ships the directory empty; the user adds a doc per dataset.

### 1.7 docs/rules/

The project's standing rules for code, docs, and tests. Includes:

- `project-layout.md` — top-level repo layout, one-line annotation per directory.
- `code-style.md` — language-agnostic rules; per-language rules in `frontend-react.md` / `backend-python.md`.
- `documentation.md` — doc style and lifecycle (headings, list style, archival, etc.).
- `testing.md` — what to test, what not to test, test organization, test commands.
- `frontend-react.md` — include only if the project has a React frontend.
- `backend-python.md` — include only if the project has a Python backend.

Projects add other per-language rule docs as needed.

## 2. When to add a new top-level section

Default: do not. The six sections cover the common cases. If a project feels it needs a seventh (e.g. `docs/security/`, `docs/operations/`), the right move is usually:

1. Check whether the content actually belongs under an existing section (security notes often belong under `architecture/`; runbooks often belong under `implementation/`).
2. If it genuinely does not fit, add the new section, update `index.md`, and add an entry to this doc explaining the rationale.

## 3. What does not belong in docs/

- Issue records. Those live under `issues/`. `docs/` references issues by directory path when needed.
- Scratch notes, drafts, half-formed ideas. Those go in `tmp/` or in a personal notes repo.
- Generated API references. Those go in `docs/implementation/<module>.md`, written by hand; do not commit generated HTML.
