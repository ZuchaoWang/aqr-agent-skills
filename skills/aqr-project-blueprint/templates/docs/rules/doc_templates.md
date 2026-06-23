# Dynamic doc templates

Templates for dynamic doc types — docs created on demand when a new module, dataset, or prior-project migration is introduced. Static one-off docs (`mission.md`, `tech_stack.md`, ...) are scaffolded once and have no entry here. Format conventions that apply to all docs live in `doc_markdown.md`; layout (which file goes where) lives in `project_layout.md`.

**Module folder location.** Each module folder lives at `implementation/[{{layer}}/]{{module}}/`. The optional `{{layer}}/` (e.g., `frontend/`, `backend/`) is used when the project has multiple source trees; otherwise the path is flat. The criteria below apply at each leaf module folder regardless of nesting.

## 1. Module design doc — `implementation/[{{layer}}/]{{module}}/design.md`

Purpose: a reviewer reads it in ~10 minutes and understands the module's role, internal structure, data, algorithms, and testing approach.

Sections (omit any that do not apply):

1. **Summary** — one paragraph: what this module does and what it explicitly does not do; its conceptual role in the larger system.
2. **Submodules and interactions** — for each submodule, a one-line role and how it interacts with the others. Omit if the module is flat.
3. **Data model** — persistent structures at a conceptual level (entity names, relationships, key fields). Not a full SQL DDL. Omit if the module has no persistent state.
4. **Key algorithms** — brief pseudocode for any non-obvious algorithm. Only the ones a reviewer needs to understand the design. Omit if the module is straightforward.
5. **Testing approach** — how this module should be tested: behaviors to cover, edge cases that matter, what to mock vs. use real. Conceptual, not a list of test cases.

Constraints: target ~1 page. Conceptual only — implementation detail lives in code. Must be reviewable without reading the code.

## 2. Module interface doc — `implementation/[{{layer}}/]{{module}}/interface.md`

Purpose: a reviewer reads it in ~5 minutes and spots missing pieces, wrong shapes, unclear naming, over-coupling.

Sections:

1. **Files** — one-line role per file in the module.
2. **Public surface** — for each public function / class / type / endpoint: signature on one line, one-sentence role on the next. Skip internal helpers, trivial accessors, and full type bodies (point at the file instead).

Constraints: target ~1 page for the whole module. The one-sentence role under each signature is the load-bearing piece.

## 3. Dataset doc — `data/{{dataset}}.md`

Purpose: a reviewer reads it and understands what the dataset contains, where it came from, and how to use it correctly without opening it.

Sections (omit any that do not apply):

1. **Summary** — one paragraph: what the dataset represents, its size and scope.
2. **Source and provenance** — where the data came from, how it was obtained, license / terms of use.
3. **Schema** — fields, types, units, allowed values. Point at the data dictionary file if one exists; do not reproduce it inline.
4. **Format** — file format (CSV, Parquet, JSON, ...), encoding, on-disk size, partitioning.
5. **Quality notes** — known gaps, anomalies, duplicates, version drift.
6. **Usage** — how to load it, common pitfalls, sample queries.

Constraints: target ~1 page. Anyone reading should be able to decide whether and how to use the dataset without opening it.

## 4. Migration doc — `project/migration/{{old_project_name}}.md`

Purpose: a reviewer who knew the prior project reads it and understands what carried over, what was dropped, and why the migration made the changes it did.

Sections:

1. **Summary** — one paragraph: which project this migrates from and the high-level reason for the migration.
2. **Inherited** — concepts, code, data, or docs that carried over, with one-line roles.
3. **Dropped** — what was left behind, with a one-line reason for each.
4. **Restructured** — a mapping table from old locations / names to new ones, where the shape changed.
5. **Rationale** — why the migration made these changes. Constraints, deadlines, new requirements.

Constraints: target ~1–2 pages. A reader who knew the old project should be able to navigate the new one without re-discovering each mapping.
