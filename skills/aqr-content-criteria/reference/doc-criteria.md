# Doc content criteria

Content criteria for every standard doc type in the recommended `docs/` layout. Format and markdown style live in the project's `docs/rules/doc_markdown.md`; layout (which file goes where) lives in `aqr-project-blueprint`. These criteria apply regardless of project.

## 1. Project docs (`docs/project/`)

### 1.1 `mission.md`

Purpose: a new team member reads it and immediately understands what problem is being solved and for whom.

Sections:

1. **Goal** — one paragraph: what problem this project solves and for whom.
2. **Problem statement** — what is broken or missing today, concretely. Reference prior work, incidents, or external context.
3. **Scope** — in scope and out of scope for the project as a whole. Revise when scope shifts.
4. **Stakeholders** — who cares about this project, who depends on it, who decides priorities.

Constraints: target ~1 page. Scope must be explicit — ambiguous scope creates drift.

### 1.2 `usage_scenarios.md`

Purpose: a reviewer reads it and understands the concrete user-facing situations the project must handle, without reading any code.

Sections:

1. **Overview** — one paragraph: the user population and the range of situations this doc covers.
2. **Scenarios** — one subsection per scenario. Each scenario: a short title, a one-paragraph description of the situation, what the user does, and what the system must do in response. Do not prescribe implementation; describe observable behavior.

Constraints: scenarios must be concrete, not abstract feature lists. "A researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds" is a scenario; "support large files" is not. Target ~2–3 pages for a typical project.

### 1.3 `roadmap.md`

Purpose: a reviewer reads it and understands what has been decided, what is coming, and roughly when.

Sections:

1. **Milestones** — one subsection per milestone: name, target date, what is included (referenced by issue group or feature name, not re-described).
2. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date.

Constraints: milestone entries reference work rather than re-describe it. Keep it current — a stale roadmap misleads more than no roadmap.

### 1.4 `migration/{{old_project_name}}.md`

Purpose: a reviewer who knew the prior project reads it and understands what carried over, what was dropped, and why.

Sections:

1. **Summary** — one paragraph: which project this migrates from and the high-level reason.
2. **Inherited** — concepts, code, data, or docs that carried over, with one-line roles.
3. **Dropped** — what was left behind, with a one-line reason for each.
4. **Restructured** — a mapping table from old locations / names to new ones where the shape changed.
5. **Rationale** — why the migration made these changes. Constraints, deadlines, new requirements.

Constraints: target ~1–2 pages. A reader who knew the old project should navigate the new one without re-discovering each mapping.

## 2. Architecture docs (`docs/architecture/`)

### 2.1 `design.md`

Purpose: a reviewer reads it in ~15 minutes and understands the system shape, component boundaries, data flow, and major decisions, without reading code.

Sections:

1. **Summary** — one paragraph: system shape, major components, how data flows through them.
2. **Components** — one subsection per major component: role, public surface, dependencies.
3. **Data flow** — how data enters the system, how it is transformed, where it is stored, how it leaves. A diagram is welcome; describe it in prose first so the doc is readable without rendering.
4. **Key design decisions** — numbered list. Each: the decision, the reason, the alternative considered, and the date made. Cross-link to any research note that informed it.

Constraints: target ~2 pages. Component boundaries must be explicit — a reader should be able to draw the architecture diagram from the text alone.

### 2.2 `interface.md`

Purpose: a reviewer reads it and understands the full external API contract without reading code.

Sections:

1. **Summary** — one paragraph: what this interface exposes and who consumes it.
2. **Endpoints / entry points** — one subsection per endpoint or entry point: method, path, one-line purpose.
3. **Request and response shapes** — for each endpoint: request fields (name, type, required/optional, constraints) and response fields (name, type, meaning).
4. **Error envelope** — the standard error response shape and the error codes / messages the API produces.
5. **Authentication and authorization** — how callers authenticate and what access controls apply.

Constraints: target ~2–3 pages. Any field whose semantics are non-obvious must have an inline note. Do not point readers at the code for things that should be documented here.

### 2.3 `tech_stack.md`

Purpose: a reviewer reads it and understands what tools are in use and why those choices were made.

Sections:

1. **Languages and runtimes** — one line per language: version, where the pin lives.
2. **Frameworks and libraries** — one line per key dependency: what it does here, why it was chosen over alternatives.
3. **Toolchain** — linters, formatters, type checkers, test runners, build tools.
4. **Rationale notes** — any non-obvious choices that would confuse a reader without explanation.

Constraints: target ~1 page. If a tool is standard and the reason is obvious ("pytest because it's the Python default"), omit the rationale.

## 3. Research docs (`docs/research/`)

Research docs inform decisions; they are not specs. Once conclusions are folded into `architecture/` or `implementation/`, move the note to `docs/archived/research/` and have the new doc reference it.

### 3.1 `background.md`

Purpose: a reviewer reads it and understands the domain concepts and motivations behind the project's technical choices.

Sections:

1. **Summary** — one paragraph: what this background doc covers and why it matters to the project.
2. **Concepts** — one subsection per key concept: definition, relevance to the project, pointers to external sources.
3. **Implications for design** — how these concepts constrain or inform the system design.

Constraints: target ~2 pages. Write for a technically literate reader who is unfamiliar with the domain. Avoid re-stating what the code already makes obvious.

### 3.2 `related_works.md`

Purpose: a reviewer reads it and understands what existing work is related, how it differs, and what the project borrows or deliberately avoids.

Sections:

1. **Summary** — one paragraph: the landscape of related work and what gap this project fills.
2. **Entries** — one subsection per related work: name, brief description, how it relates to this project (similar, complementary, or superseded), and what if anything was borrowed.

Constraints: target ~1–2 pages. Each entry must state the relationship explicitly; "this also does X" is not enough.

### 3.3 `brainstorm.md`

Purpose: a reviewer reads it and understands what design options were considered, what trade-offs were discussed, and what was decided and why.

Sections:

1. **Summary** — one paragraph: the design question being explored.
2. **Options** — one subsection per option considered: description, advantages, disadvantages.
3. **Decision** — which option was chosen, or that the question is still open. If decided, cross-link to the `architecture/design.md` key-decisions entry.

Constraints: target ~1–2 pages. The goal is to capture reasoning so it does not have to be reconstructed later. If the decision is still open, mark the open question explicitly.

## 4. Implementation docs (`docs/implementation/`)

### 4.1 `implementation/[{{layer}}/]{{module_name}}/design.md`

Purpose: a reviewer reads it in ~10 minutes and understands the module's role, internal structure, data, algorithms, and testing approach.

Sections (omit any that do not apply):

1. **Summary** — one paragraph: what this module does and what it explicitly does not do; its conceptual role in the larger system.
2. **Submodules and interactions** — for each submodule, a one-line role and how it interacts with the others. Omit if the module is flat.
3. **Data model** — persistent structures at a conceptual level (entity names, relationships, key fields). Not a full SQL DDL. Omit if the module has no persistent state.
4. **Key algorithms** — brief pseudocode for any non-obvious algorithm. Only the ones a reviewer needs to understand the design. Omit if the module is straightforward.
5. **Testing approach** — how this module should be tested: behaviors to cover, edge cases that matter, what to mock vs. use real. Conceptual, not a list of test cases.

Constraints: target ~1 page. Conceptual only — implementation detail lives in code. Must be reviewable without reading the code.

### 4.2 `implementation/[{{layer}}/]{{module_name}}/interface.md`

Purpose: a reviewer reads it in ~5 minutes and spots missing pieces, wrong shapes, unclear naming, over-coupling.

Sections:

1. **Files** — one-line role per file in the module.
2. **Public surface** — for each public function / class / type / endpoint: signature on one line, one-sentence role on the next. Skip internal helpers, trivial accessors, and full type bodies (point at the file instead).

Constraints: target ~1 page for the whole module. The one-sentence role under each signature is the load-bearing piece.

## 5. Data docs (`docs/data/`)

### 5.1 `data/{{dataset}}.md`

Purpose: a reviewer reads it and understands what the dataset contains, where it came from, and how to use it correctly, without opening it.

Sections (omit any that do not apply):

1. **Summary** — one paragraph: what the dataset represents, its size and scope.
2. **Source and provenance** — where the data came from, how it was obtained, license / terms of use.
3. **Schema** — fields, types, units, allowed values. Point at the data dictionary file if one exists; do not reproduce it inline.
4. **Format** — file format (CSV, Parquet, JSON, ...), encoding, on-disk size, partitioning.
5. **Quality notes** — known gaps, anomalies, duplicates, version drift.
6. **Usage** — how to load it, common pitfalls, sample queries.

Constraints: target ~1 page. Anyone reading should be able to decide whether and how to use the dataset without opening it.
