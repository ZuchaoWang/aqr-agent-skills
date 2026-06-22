Status: active

# Docs layout

The recommended `docs/` structure. Six sections, each with its own role. Projects add per-section docs as the project grows; they do not create new top-level sections without good reason.

## 1. Sections

### 1.1 docs/index.md

Documentation map. One section per top-level `docs/` subdirectory, listing each doc with a one-line role. New readers start here. Update this file whenever a doc is added, removed, or moves.

Status: `active` from the moment the project has more than one doc. Never `placeholder` — an empty index is worse than no index because it implies there is nothing to read.

### 1.2 docs/project-layout.md

Top-level repo layout. One-line annotation per top-level directory and root file. Update whenever the top-level shape changes.

### 1.3 docs/project/

Project-level docs: mission, problem statement, scope, roadmap, milestones, client requirements. The "what and why" of the project as a whole, not any individual feature or issue.

### 1.4 docs/architecture/

System-level architecture: component boundaries, data flow, major design decisions. The shape of the system, not the shape of any single module. Per-module implementation detail lives under `implementation/`.

### 1.5 docs/implementation/

Per-module implementation reference. One `.md` per top-level source module, plus an `overview.md` that lists them. Each module doc covers: description, files and folders, public surface (with full type signatures), and tests.

This is the layer that "guides implementation by removing ambiguity" — the layer the executing agent reaches for when writing code in a specific module.

### 1.6 docs/research/

Background research, related-work comparisons, exploratory notes. Research docs inform decisions; they are not specs. Once a research note's conclusions are folded into `architecture/` or `implementation/`, the note is flipped to `archived` and the new doc references it.

### 1.7 docs/data/

Per-dataset documentation. One `.md` per dataset, plus an `overview.md` listing them. Each dataset doc covers: source, schema, refresh cadence, known issues.

### 1.8 docs/conventions/

Code, doc, and test conventions. The project's standing rules. Includes:

- `code-style.md` — language-agnostic rules; per-language rules in `frontend-react.md` / `backend-python.md`.
- `documentation.md` — doc style and lifecycle (status header, headings, list style, etc.).
- `testing.md` — what to test, what not to test, test organization, test commands.
- `frontend-react.md` — include only if the project has a React frontend.
- `backend-python.md` — include only if the project has a Python backend.

Projects add other per-language convention docs as needed.

## 2. Status header convention

Every doc starts with `Status: placeholder | draft | active | archived`. The header is the first non-empty line — no title or YAML frontmatter precedes it. The header is what readers and tooling use to decide whether a doc is safe to act on.

See `templates/docs/conventions/documentation.md` for the full lifecycle.

## 3. When to add a new top-level section

Default: do not. The six sections cover the common cases. If a project feels it needs a seventh (e.g. `docs/security/`, `docs/operations/`), the right move is usually:

1. Check whether the content actually belongs under an existing section (security notes often belong under `architecture/`; runbooks often belong under `implementation/`).
2. If it genuinely does not fit, add the new section, update `index.md`, and add an entry to this doc explaining the rationale.

## 4. What does not belong in docs/

- Issue records. Those live under `issues/`. `docs/` references issues by directory path when needed.
- Scratch notes, drafts, half-formed ideas. Those go in `tmp/` or in a personal notes repo.
- Generated API references. Those go in `docs/implementation/<module>.md`, written by hand; do not commit generated HTML.
