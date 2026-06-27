# Project doc content criteria

Criteria for the project-level docs: mission, usage scenarios, roadmap, tech stack, and the project's active concept set. These are lighter than design docs — they state what and why at the project level, not how. Where each doc lives is a layout choice — see the `aqr-doc-blueprint` skill.

## 1. Mission

Purpose: a new team member reads it and immediately understands what problem is being solved and for whom.

Sections:

1. **Goal** — one paragraph: what problem this project solves and for whom.
2. **Problem statement** — what is broken or missing today, concretely. Reference prior work, incidents, or external context.
3. **Scope** — in scope and out of scope for the project as a whole. Revise when scope shifts; ambiguous scope creates drift.
4. **Stakeholders** — who cares about this project, who depends on it, who decides priorities.

Constraints: target ~1 page. Scope must be explicit.

## 2. Usage scenarios

Purpose: a reviewer reads it and understands the concrete user-facing situations the project must handle, without reading code. Scenarios bridge the mission (what the project is for) and the design (how it is built) by pinning down observable behavior the system must produce.

Sections:

1. **Overview** — one paragraph: the user population and the range of situations this doc covers.
2. **Scenarios** — one subsection per scenario. Each: a short title, a one-paragraph description of the situation, what the user does, and what the system must do in response — described as observable behavior, not implementation.

Frame each scenario around a user and a goal; state the situation and the required response, not how the system achieves it.

Constraints:

- Scenarios must be concrete, not abstract feature lists. "A researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds" is a scenario; "support large files" is not.
- Describe observable behavior, not implementation — "the system returns invalid rows within 30 seconds," not "the system spawns a worker that streams the file."
- One scenario per situation; keep divergent responses as separate scenarios so each is testable.

## 3. Roadmap

Purpose: a reviewer reads it and understands where the project is heading, what is in flight now, and what comes next.

Sections:

1. **Vision** — the long-term direction; where the project is heading qualitatively.
2. **Now** — the current milestone; what is in flight.
3. **Next** — the milestone queued after Now.
4. **Later** — milestones beyond Next, stated at a coarser grain.
5. **Decisions log** — key decisions that shaped the roadmap, each with a one-line rationale and date. Example: "2026-03: deferred multi-tenant isolation to v2 — single-tenant ships first to meet the pilot deadline; revisit when a second tenant is signed."

Constraints: reference work by name rather than re-describing it. For a date-driven project, swap Now/Next/Later for dated milestones (name, target date, what is included). Keep it current — a stale roadmap misleads more than no roadmap.

## 4. Tech stack

Purpose: a reviewer reads it and understands what tools are in use and why those choices were made.

Sections:

1. **Languages and runtimes** — one line per language: version, where the pin lives.
2. **Frameworks and libraries** — one line per key dependency: what it does here, why it was chosen over alternatives. When a library was chosen over custom code (library-first), say so and name the custom code it replaces — e.g. "cockatiel — retry logic, chosen over hand-rolled retry."
3. **Toolchain** — linters, formatters, type checkers, test runners, build tools.
4. **Rationale note** — any non-obvious choice that would confuse a reader without explanation.

Constraints: target ~1 page. If a tool is standard and the reason is obvious ("pytest because it's the Python default"), omit the rationale.

## 5. Concepts

The domain concepts the project actively uses. Concepts considered but not go here. For each concept:

- **Definition** — one or two sentences precise enough that two team members would agree on it.
- **Used here** — where it shows up (a component, data model, algorithm, or user-facing term). If it has no use site, it belongs in background, not here.

This is the active vocabulary, not a dictionary — list only what a reader needs to understand the rest of the docs.
