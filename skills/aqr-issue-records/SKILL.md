---
name: aqr-issue-records
description: Disciplined doc and code changes, fixes, and investigations in a repo. Each issue lives in its own directory with task / plan / progress / summary / report artifacts, typed as doc-update, code-update, fix, or investigate. Manual-only — invoke via slash command.
disable-model-invocation: true
---

# aqr-issue-records

A disciplined way to make doc or code changes, ship fixes, or run investigations in a repo. Each piece of work — an **issue** — lives in its own directory under `issues/`, follows one of four types (`doc-update`, `code-update`, `fix`, `investigate`), and is recorded through five canonical artifacts: `task.md`, `plan.md`, `progress.md`, `summary.md`, `report.md`. Templates and reference docs are provided for each.

## Scope

The skill specifies the artifacts, their roles, and the type taxonomy. The surrounding workflow (clarification, planning, execution, review) is owned by the user's methodology skill (such as Superpowers), not by this one.

The artifacts are text files written in the user's project documentation style. Anyone — the user, a reviewer, a future agent — can read them cold and understand what was attempted, what was decided, what was verified, and what is still open.

## Invocation

This skill is manual-only. The user invokes it explicitly via slash command (e.g. `/aqr-issue-records`). It is not auto-applied based on project state, because the issue format is opinionated and not every project wants it.

## Where issues live

```
issues/<YYYYMM>/<issue-id>/
```

`<YYYYMM>` is the year-month the issue was opened, using the project's local date at creation time. `<issue-id>` is `<YYYYMMDD>-<short-kebab-case-slug>` using today's date for the prefix; if the user already supplied a slug, use it directly.

The `issues/<YYYYMM>/` grouping is for human navigation and bulk review. It carries no semantic meaning beyond that.

## Inside an issue directory

```text
issues/<YYYYMM>/<issue-id>/
├── task.md            # all types
├── plan.md            # all types
├── progress.md        # doc-update, code-update, fix
├── summary.md         # doc-update, code-update, fix
├── report.md          # investigate
└── assets/            # optional, user-provided inputs
```

The `.md` files at the directory root are the canonical artifacts — see Artifacts below for their roles. `assets/` is an optional subdirectory for reference material the user provided as input: a screenshot of a bug, a PDF spec, sample data, an excerpt of discussion with another AI used to guide the issue, anything the user pasted in to shape the issue's scope.

## Artifacts

Five canonical file names. Each name describes its semantic role; the issue type determines which ones appear. These names and roles are part of the contract — do not rename them and do not invent additional required artifacts.

| File          | Used by                                       | When it appears              | Role                                                                                                                                                   |
| --------------| --------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `task.md`     | all types                                     | at issue creation            | Semantic definition: type, goal, context, scope, out-of-scope, acceptance criteria. No file lists.                                                    |
| `plan.md`     | all types                                     | during planning              | Investigation findings, planned changes with concrete file paths, open questions, execution steps, self-review notes.                                 |
| `progress.md` | doc-update, code-update, fix                  | when execution begins        | Log of which planned steps were carried out. Starts `## Status: in-progress`; ends `## Status: done`. Distinct from `summary.md` (see below).         |
| `summary.md`  | doc-update, code-update, fix                  | at completion                | What shipped in the diff: files changed (with per-location detail for modifications), commits made, verification notes, known limitations.            |
| `report.md`   | investigate                                   | at completion                | Findings, answer, confidence. The deliverable itself, not a summary of deliverables produced elsewhere.                                                |

### Why summary.md and report.md are different files

Both are completion-stage artifacts, but they have different semantic roles:

- `summary.md` describes what was **operationally produced** — which files changed, where, and how the change was verified. Used by types that produce a diff (doc-update, code-update, fix).
- `report.md` describes what was **discovered or concluded** — findings, the answer to the investigation's question, confidence in that answer. Used by the type that produces no diff (investigate). The report IS the deliverable, not a description of deliverables.

A reader picking up `summary.md` expects file changes and verification. A reader picking up `report.md` expects findings and an answer. Mixing them under one name (as the previous `result.md` did) forced the reader to infer which kind of artifact they were looking at based on issue type.

### task.md is semantic, plan.md is enumerative

`task.md` describes the issue at the level of "what behavior or knowledge changes" — not "which files change". The exact file list lives in `plan.md`. Keeping these separate means:

- A reader can understand the issue from `task.md` without getting bogged down in implementation detail.
- The implementer can lift the file list directly from `plan.md` without re-deriving it.
- Scope changes (a new file added to the work) update `plan.md`, not `task.md` — so `task.md` stays stable as the plan evolves.

### progress.md vs summary.md

These two files serve different purposes, not the same purpose at different lifecycle stages:

- `progress.md` is the **execution log**. It tracks which planned steps (from `plan.md`'s Execution Steps) or per-defect fixes (from `plan.md`'s Per-Defect Fix Plan) were carried out, in order. It answers "did you do what you planned?".
- `summary.md` is the **diff and verification summary**. It tracks what files changed, which commits landed, how each acceptance criterion was verified, what limitations were accepted, and what follow-ups surfaced. It answers "what shipped and how was it verified?".

A reviewer reading `progress.md` wants to see the work unfold against the plan. A reviewer reading `summary.md` wants to see the resulting diff and its verification. Both are required for doc-update, code-update, and fix.

## Issue types

Four types. Each type has its own template set under `templates/`.

- **doc-update** — Docs-only issue. Used for product specs, architecture docs, implementation docs, research notes that get a permanent home under `docs/`, design decisions, data notes, and convention updates.
- **code-update** — Code and test change issue. Code-only. If a doc needs to change to stay consistent with the new code, open a separate doc-update issue rather than expanding this issue's scope.
- **fix** — Small correction issue. Used for bugs, failed tests, broken docs, inconsistencies, or small cleanup. Can touch code, docs, or both, but **a fix that touches both must be small and atomic** — one defect, no subtasks. Large work that needs both code and docs is split: `doc-update` first to define the intended behavior, then a `code-update` follow-up to implement it.
- **investigate** — Pure investigation or feasibility check. Produces findings but no file changes. Findings live in `report.md`. If findings deserve a permanent home under `docs/`, follow up with a doc-update issue.

See `reference/issue-types.md` for the full decision tree and the per-type artifact sets. See `reference/<artifact>-guidelines.md` (one per artifact: task, plan, progress, summary, report) for how to write each artifact.

## When the user invokes this skill

Each verb names the artifact to create or update; the methodology skill (e.g. Superpowers) drives the actual work.

- **create** — draft `task.md`; set up the issue directory if it does not already exist, otherwise update in place.
- **plan** — draft `plan.md`.
- **execute** — carry out `plan.md`; writes `progress.md` as work lands and `summary.md` (or `report.md`) at completion.
- **revise** — update an existing `task.md` or `plan.md` when something surfaces mid-execution.

The skill activates only on explicit slash invocation. Once activated, the verb is read from the user's prose, inferred, or asked for if unclear. The user may name a single verb or chain multiple (e.g. "create, plan, and execute this issue").

**Clarification stance.** Front-load clarification at the start of each invocation — one round covers the whole chain of verbs the user named, not per verb. Ask a few targeted questions to make the work unambiguous. Do not run for an hour and then ask. If something ambiguous turns up later and is not blocking, pick the most likely interpretation and proceed; stop to ask only when the ambiguity is material and no reasonable default exists.

## Reading order for a fresh issue

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (doc/code/fix) — which planned steps were carried out? What is still in flight?
4. `summary.md` (doc/code/fix) — what shipped, what was verified, what is left.
   `report.md` (investigate) — what was found, what is the answer, how confident.

Conflict resolution: `task.md` wins for scope decisions. `plan.md` reflects current thinking. `progress.md` reflects execution state. `summary.md` (or `report.md`) is the authoritative summary at completion.
