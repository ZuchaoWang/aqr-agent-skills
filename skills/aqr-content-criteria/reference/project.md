# Project doc content criteria

Criteria for the project-level docs: `project/mission.md`, `project/roadmap.md`, `architecture/tech_stack.md`, `project/migration/{{old_project_name}}.md`, and the project's active concept set. These are lighter than design docs — they state what and why at the project level, not how. Layout lives in `aqr-project-blueprint`; markdown format lives in the project's `docs/rules/doc_markdown.md`.

## 1. `mission.md`

Purpose: a new team member reads it and immediately understands what problem is being solved and for whom.

Sections:

1. **Goal** — one paragraph: what problem this project solves and for whom.
2. **Problem statement** — what is broken or missing today, concretely. Reference prior work, incidents, or external context.
3. **Scope** — in scope and out of scope for the project as a whole. Revise when scope shifts; ambiguous scope creates drift.
4. **Stakeholders** — who cares about this project, who depends on it, who decides priorities.

Constraints: target ~1 page. Scope must be explicit.

## 2. `roadmap.md`

Purpose: a reviewer reads it and understands what has been decided, what is coming, and roughly when.

Sections:

1. **Milestones** — one subsection per milestone: name, target date, what is included (referenced by issue group or feature name, not re-described).
2. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date.

Constraints: milestone entries reference work rather than re-describe it. Keep it current — a stale roadmap misleads more than no roadmap.

## 3. `architecture/tech_stack.md`

Purpose: a reviewer reads it and understands what tools are in use and why those choices were made.

Sections:

1. **Languages and runtimes** — one line per language: version, where the pin lives.
2. **Frameworks and libraries** — one line per key dependency: what it does here, why it was chosen over alternatives.
3. **Toolchain** — linters, formatters, type checkers, test runners, build tools.
4. **Rationale note** — any non-obvious choice that would confuse a reader without explanation.

Constraints: target ~1 page. If a tool is standard and the reason is obvious ("pytest because it's the Python default"), omit the rationale.

## 4. `project/migration/{{old_project_name}}.md`

Purpose: a reviewer who knew the prior project reads it and understands what carried over, what was dropped, and why.

Sections:

1. **Summary** — one paragraph: which project this migrates from and the high-level reason.
2. **Inherited** — concepts, code, data, or docs that carried over, with one-line roles.
3. **Dropped** — what was left behind, with a one-line reason for each.
4. **Restructured** — a mapping table from old locations or names to new ones where the shape changed.
5. **Rationale** — why the migration made these changes. Constraints, deadlines, new requirements.

Constraints: target ~1–2 pages. A reader who knew the old project should navigate the new one without re-discovering each mapping.

## 5. Concepts (active)

The domain concepts the project **actively uses**. Distinguished from `research/background.md` (see `research.md`): this section holds only concepts in active use; background knowledge, motivations, and concepts considered but **not** used live there.

For each concept:

- **Definition** — a one- or two-sentence definition precise enough that two team members would agree on it.
- **How it is used here** — where the concept shows up in this project (a component, a data model, an algorithm, a user-facing term). If a concept has no use site, it does not belong here — it belongs in `background.md`.

Constraints: a concept appears here only if the project acts on it. Writing a glossary of every related term defeats the purpose; this is the active vocabulary, not a dictionary. Target length scales with the domain — keep it to the concepts a reader needs to understand the rest of `docs/`.
