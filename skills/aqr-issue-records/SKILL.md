---
name: aqr-issue-records
description: Repo-visible issue record format. Per-type templates (doc-update, code-update, fix, investigate) with task / plan / progress / summary / report artifacts. Manual-only — invoke via slash command.
disable-model-invocation: true
---

# aqr-issue-records

Defines the repo-visible issue record format. Provides artifact templates for four issue types (doc-update, code-update, fix, investigate) and reference docs that explain the meaning of each artifact and what a good summary.md / report.md looks like.

## Scope

This skill defines the **durable issue artifacts** that live inside the repo under `issues/`. It does not define the surrounding development workflow — clarification, planning, TDD, debugging, verification, and review are owned by whatever execution skill the user has paired with it (for example Superpowers).

The contract this skill enforces:

- Each issue lives in its own directory.
- Each issue has the same canonical artifacts, in the same roles.
- The artifacts are text files written in the user's project documentation style.
- Anyone — the user, a reviewer, a future agent — can read the artifacts cold and understand what was attempted, what was decided, what was verified, and what is still open.

## Invocation

This skill is manual-only. The user invokes it explicitly via slash command (e.g. `/aqr-issue-records`). It is not auto-applied based on project state, because the issue format is opinionated and not every project wants it.

## Where issues live

```
issues/<YYYYMM>/<issue-id>/
```

`<YYYYMM>` is the year-month the issue was opened, using the project's local date at creation time. `<issue-id>` is `<YYYYMMDD>-<short-kebab-case-slug>` using today's date for the prefix; if the user already supplied a slug, use it directly.

The `issues/<YYYYMM>/` grouping is for human navigation and bulk review. It carries no semantic meaning beyond that.

## Artifacts

Five canonical file names. Each name describes the file's semantic role; the issue type determines which ones appear.

| File          | Used by                                       | When it appears              | Role                                                                                                                                                   |
| --------------| --------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `task.md`     | all types                                     | at issue creation            | Semantic definition: goal, context, scope, out-of-scope, acceptance criteria. No file lists.                                                          |
| `plan.md`     | all types (optional for investigate)          | during planning              | Investigation findings, planned changes with concrete file paths, open questions, execution steps, self-review notes.                                 |
| `progress.md` | doc-update, code-update, fix                  | when execution begins        | Live operational log. Starts `## Status: in-progress`; ends `## Status: done`. Optional — skip for short, atomic work.                                 |
| `summary.md`  | doc-update, code-update, fix                  | at completion                | Final operational summary: files changed (with per-location detail for modifications), verification notes, known limitations.                          |
| `report.md`   | investigate                                   | at completion                | Findings, answer, confidence. The deliverable itself, not a summary of deliverables produced elsewhere.                                                |

### Why summary.md and report.md are different files

Both are completion-stage artifacts, but they have different semantic roles:

- `summary.md` describes what was **operationally produced** — which files changed, where, and how the change was verified. It is the final, curated form of the live operational log (`progress.md`).
- `report.md` describes what was **discovered or concluded** — findings, the answer to the investigation's question, confidence in that answer. It is the deliverable, not a description of deliverables.

A reader picking up `summary.md` expects file changes and verification. A reader picking up `report.md` expects findings and an answer. Mixing them under one name (as the previous `result.md` did) forced the reader to infer which kind of artifact they were looking at based on issue type.

### task.md is semantic, plan.md is enumerative

`task.md` describes the issue at the level of "what behavior or knowledge changes" — not "which files change". The exact file list lives in `plan.md`. Keeping these separate means:

- A reader can understand the issue from `task.md` without getting bogged down in implementation detail.
- The implementer can lift the file list directly from `plan.md` without re-deriving it.
- Scope changes (a new file added to the work) update `plan.md`, not `task.md` — so `task.md` stays stable as the plan evolves.

### progress.md vs summary.md

`progress.md` is the **live** operational log; `summary.md` is the **final** operational summary. Both describe the same kind of work at different lifecycle stages. Splitting them lets the live log stay honest about confusion and false starts while the summary stays curated and stable.

Short, atomic work can skip `progress.md` and write `summary.md` directly at completion.

## Issue types

Four types. Each type has its own template set under `templates/`.

- **doc-update** — Docs-only issue. Used for product specs, architecture docs, implementation docs, research notes that get a permanent home under `docs/`, design decisions, data notes, and convention updates.
- **code-update** — Code and test change issue. Code-only. If a doc needs to change to stay consistent with the new code, open a separate doc-update issue rather than expanding this issue's scope.
- **fix** — Small correction issue. Used for bugs, failed tests, broken docs, inconsistencies, or small cleanup. Can touch code, docs, or both, but should stay narrow.
- **investigate** — Pure investigation or feasibility check. Produces findings but no file changes. Findings live in `report.md`. If findings deserve a permanent home under `docs/`, follow up with a doc-update issue.

Per-type artifact sets:

- doc-update: `task.md` + `plan.md` + `summary.md` (optional: `progress.md`)
- code-update: `task.md` + `plan.md` + `summary.md` (optional: `progress.md`)
- fix: `task.md` + `plan.md` + `summary.md` (optional: `progress.md`)
- investigate: `task.md` + `report.md` (optional: `plan.md`; no `progress.md`, no `summary.md`)

See `reference/issue-types.md` for the full decision tree and `reference/artifact-meanings.md` for what each artifact is and is not.

## When the user invokes this skill

The user typically asks for one of:

- Create / plan / execute / revise an issue by ID or description.
- Apply a specific issue type's template to a new issue.
- Compare an existing issue's artifacts against the templates and report drift.

The skill does not auto-create issues from casual task descriptions. The user must explicitly invoke the skill or name a target issue directory.

## How this skill relates to execution workflows

This skill is workflow-agnostic. The user may pair it with Superpowers, with `handle-issue`, or with any other execution methodology they prefer. The split:

- **Execution methodology** (clarification, critique, planning, TDD, debugging, verification, review): owned by the workflow skill.
- **Durable artifacts** (what gets written into the repo so the work is reviewable, resumable, and auditable): owned by this skill.

When an execution workflow prescribes artifacts that overlap with this skill's, prefer this skill's format and names unless the user explicitly says otherwise. Names like `task.md`, `plan.md`, `progress.md`, `summary.md`, `report.md` are part of the contract — do not rename them.

## Reading order for a fresh issue

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (if present, code/doc/fix) — where did execution get to? What is in flight?
4. `summary.md` (if present, code/doc/fix) — what shipped, what was verified, what is left.
   `report.md` (if present, investigate) — what was found, what is the answer, how confident.

Conflict resolution: `task.md` wins for scope decisions. `plan.md` reflects current thinking. `progress.md` reflects live state. `summary.md` (or `report.md`) is the authoritative summary at completion.
