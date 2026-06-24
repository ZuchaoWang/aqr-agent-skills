---
name: aqr-issue-records
description: Disciplined doc and code changes, fixes, and investigations in a repo. Each issue lives in its own directory under issues/<YYYYMM>/ with task / plan / progress / summary / report artifacts, typed as doc-update, code-update, fix, or investigate. Related issues can be batched in an issue group (index.md + progress.md + children/). Manual-only — invoke via slash command.
disable-model-invocation: true
---

# aqr-issue-records

A disciplined way to make doc or code changes, ship fixes, or run investigations in a repo. The system has two object types: an **issue** — a single executable unit of work — and an **issue group** — a batch-execution container that holds related issues under a `children/` subdirectory. An issue lives in its own directory under `issues/`, follows one of four types (`doc-update`, `code-update`, `fix`, `investigate`), and is recorded through five canonical artifacts: `task.md`, `plan.md`, `progress.md`, `summary.md`, `report.md`. An issue group is recorded through two artifacts of its own (`index.md`, `progress.md`) plus its child issues. Templates and reference docs are provided for each.

## Scope

The skill specifies the artifacts, their roles, and the type taxonomy. The surrounding workflow (clarification, planning, execution, review) is owned by the user's methodology skill (such as Superpowers), not by this one.

The artifacts are text files written in the user's project documentation style. Anyone — the user, a reviewer, a future agent — can read them cold and understand what was attempted, what was decided, what was verified, and what is still open.

## Invocation

This skill is manual-only. The user invokes it explicitly via slash command (e.g. `/aqr-issue-records`). It is not auto-applied based on project state, because the issue format is opinionated and not every project wants it.

## Where issues and issue groups live

There are two cases — a standalone issue and an issue group. Both live under `issues/<YYYYMM>/`:

```
issues/<YYYYMM>/<YYYYMMDD>-<issue-name>/   # a standalone issue
issues/<YYYYMM>/<YYYYMMDD>-<group-name>/   # an issue group
```

`<YYYYMM>` is the year-month the issue was opened, using the project's local date at creation time. The `issues/<YYYYMM>/` grouping is for human navigation and bulk review; it carries no semantic meaning beyond that.

**Standalone issue.** The directory is `<YYYYMMDD>-<issue-name>`, where `<issue-name>` is a short kebab-case name, using today's date for the prefix; if the user already supplied a name, use it directly.

**Issue group.** The directory has the same shape, `<YYYYMMDD>-<group-name>`, and holds a `children/` subdirectory where its child issues live:

```
issues/<YYYYMM>/<YYYYMMDD>-<group-name>/
  children/
    <index>-<issue-name>/                  # a child issue
```

Child issues are regular issues that live under `children/`, each in its own directory `<index>-<issue-name>` (for example `001-update-architecture`). `<index>` is a one-based, zero-padded three-digit sequence (`001`, `002`, `003`, …) that fixes the order among siblings. Child issues drop the date prefix because the parent group already carries the date and ordering context. See `reference/issue-groups.md` for the full group structure and rules.

## Issue types

Types are an issue concept: every issue — standalone or a child inside a group — has exactly one of four types. An issue group is not a type; it is a container that batches issues and has no type of its own. Each type has its own template set under `templates/`.

- **doc-update** — Docs-only issue. Used for product specs, architecture docs, implementation docs, research notes that get a permanent home under `docs/`, design decisions, data notes, and convention updates.
- **code-update** — Code and test change issue. Code-only — if a doc needs to change to stay consistent with the new code, open a separate `doc-update` issue rather than expanding this issue's scope.
- **fix** — Any small, atomic change. Can span code, tests, docs, config, infra, and data in one issue, as long as it is one cohesive change with no subtasks. Large work follows the non-fix branch of the decision tree (`code-update` or `doc-update`, split if it touches both).
- **investigate** — Pure investigation, feasibility check, or lookup. Produces findings but no file changes. Findings live in `report.md`. If findings deserve a permanent home under `docs/`, follow up with a `doc-update` issue.

See `reference/issue-types.md` for the full decision tree and the per-type artifact sets. See `reference/issue-groups.md` for issue groups.

## Inside the directory

There are two cases. A standalone issue directory holds the issue's own artifacts. An issue group directory holds `index.md`, `progress.md`, and a `children/` subdirectory of child issues.

### Standalone issue

```text
issues/<YYYYMM>/<YYYYMMDD>-<issue-name>/
├── task.md            # all types
├── plan.md            # all types
├── progress.md        # doc-update, code-update, fix
├── summary.md         # doc-update, code-update, fix
├── report.md          # investigate
└── assets/            # optional, user-provided inputs
```

The `.md` files at the directory root are the canonical artifacts — see Artifacts below for their roles. `assets/` is an optional subdirectory for reference material the user provided as input: a screenshot of a bug, a PDF spec, sample data, an excerpt of discussion with another AI used to guide the issue, anything the user pasted in to shape the issue's scope.

### Issue group

```text
issues/<YYYYMM>/<YYYYMMDD>-<group-name>/
├── index.md            # static description: purpose, execution order, dependencies
├── progress.md         # Execution Log queue + machine-checked status line
└── children/
    └── <index>-<issue-name>/  # a child issue — full issue with its own type and artifacts
        ├── task.md
        ├── plan.md
        ├── progress.md
        ├── summary.md   # or report.md for investigate
        └── assets/      # optional
```

A group has only `index.md` and `progress.md` of its own; it has no type, no `task.md`, and no `summary.md`/`report.md`. The group is an execution container — each child issue carries its own type and summarizes itself. See `reference/issue-groups.md`.

## Artifacts

There are two cases. A standalone issue uses five canonical issue files; an issue group adds one group-only file, `index.md`, and reuses `progress.md` with a different body. These names and roles are part of the contract — do not rename them and do not invent additional required artifacts.

| File          | Used by                                       | When it appears              | Role                                                                                                                                                   |
| --------------| --------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `task.md`     | all types                                     | at issue creation            | Semantic definition: type, goal, context, scope, out-of-scope, acceptance criteria. No file lists.                                                    |
| `plan.md`     | all types                                     | during planning              | Investigation findings, planned changes with concrete file paths, open questions, execution steps, self-review notes.                                 |
| `progress.md` | doc-update, code-update, fix                  | when execution begins        | Log of which planned steps were carried out. Starts `## Status: in-progress`; ends `## Status: done`. Distinct from `summary.md` (see below).         |
| `summary.md`  | doc-update, code-update, fix                  | at completion                | What shipped in the diff: files changed (with per-location detail for modifications), commits (when multi-commit), verification notes mapped to plan.md's checks, known limitations, follow-up issues.            |
| `report.md`   | investigate                                   | at completion                | Findings, answer, confidence. The deliverable itself, not a summary of deliverables produced elsewhere.                                                |
| `index.md`    | issue groups                                  | at group creation            | Static description of the group: purpose, execution order of children, dependencies between children. Group-only; no issue has this file.             |

Issue groups reuse `progress.md` with a different body: instead of a step log, the body is an Execution Log table of `(child, verb, status)` rows. The machine-checked status line is the same, except a group may also start at `## Status: pending` (a group can be created before any work is requested). See `reference/issue-groups.md`.

See `templates/<type>/` for section structure and inline format. See `reference/<artifact>-guidelines.md` (one per artifact: task, plan, progress, summary, report) for how to write each artifact. See `templates/issue-group/` and `reference/issue-groups.md` for issue groups.

### Why summary.md and report.md are different files

Both are completion-stage artifacts, but they have different semantic roles:

- `summary.md` describes what was **operationally produced** — which files changed, where, and how the change was verified. Used by types that produce a diff (doc-update, code-update, fix).
- `report.md` describes what was **discovered or concluded** — findings, the answer to the investigation's question, confidence in that answer. Used by the type that produces no diff (investigate). The report IS the deliverable, not a description of deliverables.

A reader picking up `summary.md` expects file changes and verification. A reader picking up `report.md` expects findings and an answer. Mixing them under one name would force the reader to infer which kind of artifact they were looking at based on issue type.

### task.md is semantic, plan.md is enumerative

`task.md` describes the issue at the level of "what behavior or knowledge changes" — not "which files change". The exact file list lives in `plan.md`. Keeping these separate means:

- A reader can understand the issue from `task.md` without getting bogged down in implementation detail.
- The implementer can lift the file list directly from `plan.md` without re-deriving it.
- Scope changes (a new file added to the work) update `plan.md`, not `task.md` — so `task.md` stays stable as the plan evolves.

### progress.md vs summary.md

These two files serve different purposes, not the same purpose at different lifecycle stages:

- `progress.md` is the **execution log**. It tracks which planned steps (from `plan.md`'s Execution Steps) or per-item changes (from `plan.md`'s Per-Item Change Plan) were carried out, in order. It answers "did you do what you planned?".
- `summary.md` is the **diff and verification summary**. It tracks what files changed, which commits landed, how each acceptance criterion was verified, what limitations were accepted, and what follow-ups surfaced. It answers "what shipped and how was it verified?".

A reviewer reading `progress.md` wants to see the work unfold against the plan. A reviewer reading `summary.md` wants to see the resulting diff and its verification. Both are required for doc-update, code-update, and fix.

## When the user invokes this skill

There are two cases. The same verbs apply to both, but at the group level they drive the children rather than drafting a single issue's artifacts. Each verb names the artifact to create or update; the methodology skill (e.g. Superpowers) drives the actual work.

**Standalone issue.**

- **create** — draft `task.md`; set up the issue directory if it does not already exist, otherwise update in place.
- **plan** — draft `plan.md`.
- **execute** — carry out `plan.md`; writes `progress.md` as work lands and `summary.md` (or `report.md`) at completion.
- **revise** — update an existing `task.md` or `plan.md` when something surfaces mid-execution.

**Issue group.** The decomposition must already be available before the group is created (see `reference/issue-groups.md` §10):

- **create** group — write `index.md` (purpose, execution order, dependencies) and create the child issue directories; queue one `create` row per child in the group `progress.md`. Do not queue `plan` or `execute` rows yet.
- **plan** group — drive each child's `plan` in execution order, following the Ordering Rule (outer loop = child, inner loop = verbs); queue and work the `plan` rows.
- **execute** group — drive each child's `execute` the same way; queue and work the `execute` rows.
- **revise** is not a queued group verb. A child issue may be revised on demand during its own execution; that revision stays inside the child and is not a separate row in the group's Execution Log.

The skill activates only on explicit slash invocation. Once activated, the verb is read from the user's prose, inferred, or asked for if unclear. The user may name a single verb or chain multiple (e.g. "create, plan, and execute this issue").

**Clarification stance.** Front-load clarification at the start of each invocation — one round covers the whole chain of verbs the user named, not per verb. Ask a few targeted questions to make the work unambiguous. Do not run for an hour and then ask. If something ambiguous turns up later and is not blocking, pick the most likely interpretation and proceed; stop to ask only when the ambiguity is material and no reasonable default exists.

## Reading order

There are two cases.

**Standalone issue.**

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (doc/code/fix) — which planned steps were carried out? What is still in flight?
4. `summary.md` (doc/code/fix) — what shipped, what was verified, what is left.
   `report.md` (investigate) — what was found, what is the answer, how confident.

**Issue group.**

1. `index.md` — what is the group for, in what order do the children run, and what depends on what?
2. `progress.md` — which `(child, verb)` rows are pending, in-progress, or done?
3. Each in-flight child's own artifacts, in the issue reading order above.

Conflict resolution: `task.md` wins for scope decisions. `plan.md` reflects current thinking. `progress.md` reflects execution state. `summary.md` (or `report.md`) is the authoritative summary at completion. For a group, `index.md` is the intended plan and `progress.md` is what actually happened; on conflict between them, `progress.md` is the execution state of record.
