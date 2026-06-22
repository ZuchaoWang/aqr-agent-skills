---
name: aqr-issue-records
description: Repo-visible issue record format with task.md / plan.md / progress.md / result.md. Use when the user invokes aqr-issue-records, when the project already has issues/ folders using this format, or when project docs say this format is in use.
---

# aqr-issue-records

Defines the repo-visible issue record format. Provides artifact templates for three issue types (doc-update, code-update, fix) and reference docs that explain the meaning of each artifact and what a good result.md looks like.

## Scope

This skill defines the **durable issue artifacts** that live inside the repo under `issues/`. It does not define the surrounding development workflow — clarification, planning, TDD, debugging, verification, and review are owned by whatever execution skill the user has paired with it (for example Superpowers).

The contract this skill enforces:

- Each issue lives in its own directory.
- Each issue has the same canonical artifacts, in the same roles.
- The artifacts are text files written in the user's project documentation style.
- Anyone — the user, a reviewer, a future agent — can read the artifacts cold and understand what was attempted, what was decided, what was verified, and what is still open.

## Where issues live

```
issues/<YYYYMM>/<issue-id>/
```

`<YYYYMM>` is the year-month the issue was opened, using the project's local date at creation time. `<issue-id>` is `<YYYYMMDD>-<short-kebab-case-slug>` using today's date for the prefix; if the user already supplied a slug, use it directly.

The `issues/<YYYYMM>/` grouping is for human navigation and bulk review. It carries no semantic meaning beyond that.

## Artifacts

| File        | When it appears                                  | Role                                                                                                                          |
| ------------| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `task.md`   | At issue creation (Phase 0 of any workflow)        | Source of truth for scope, context, out-of-scope, and acceptance criteria.                                                    |
| `plan.md`   | During clarification/planning (Phases 1–4)         | Investigation findings, planned changes, open questions, execution steps, self-review notes.                                  |
| `progress.md` | Optional, created when execution begins (Phase 5) | Live checkpoint. Starts with `## Status: in-progress`; ends with `## Status: done` when the work is verified.                 |
| `result.md` | At issue completion                                | Final, stable summary: files changed, verification notes, known limitations, follow-up issues. Supersedes `progress.md`.      |

`progress.md` is for resumability — a long-running or risky execution writes intermediate state here so a resumed session or a reviewer can pick up where it left off. Short, atomic issues can skip `progress.md` and write `result.md` directly at completion. If both exist, `result.md` is authoritative.

## Issue types

Three types. Each type has its own template set under `templates/`.

- **doc-update** — Docs-only issue. Used for product specs, architecture docs, implementation docs, research notes, design decisions, data notes, and convention updates. Research is a doc-update subtype: research changes knowledge records, not source code, so it lives under the doc-update format.
- **code-update** — Code and test change issue. May update docs if needed to keep behavior/API/module docs consistent. Should usually refer to approved docs or prior doc-update results.
- **fix** — Small correction issue. Used for bugs, failed tests, broken docs, inconsistencies, or small cleanup. Can touch code, docs, or both, but should stay narrow.

See `reference/issue-types.md` for the full decision tree and `reference/artifact-meanings.md` for what each artifact is and is not.

## When to use this skill

Use this skill when **any** of the following is true:

- The user explicitly invokes `aqr-issue-records`.
- The project already has issue folders with `task.md` / `plan.md` / `result.md` matching this layout.
- Project-local docs say the project uses this issue record format.

Do **not** impose this issue format on unrelated projects unless the user asks. A project that already uses Linear, GitHub issues, or a different in-repo convention should keep doing that.

## How this skill relates to execution workflows

This skill is workflow-agnostic. The user may pair it with Superpowers, with `handle-issue`, or with any other execution methodology they prefer. The split:

- **Execution methodology** (clarification, critique, planning, TDD, debugging, verification, review): owned by the workflow skill.
- **Durable artifacts** (what gets written into the repo so the work is reviewable, resumable, and auditable): owned by this skill.

When an execution workflow prescribes artifacts that overlap with this skill's, prefer this skill's format and names unless the user explicitly says otherwise. Names like `task.md`, `plan.md`, `progress.md`, `result.md` are part of the contract — do not rename them.

## Reading order for a fresh issue

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (if present) — where did execution get to? What is in flight?
4. `result.md` (if present) — what shipped, what was verified, what is left.

If `task.md` and `plan.md` disagree, `task.md` wins for scope decisions; `plan.md` reflects current thinking. If `plan.md` and `progress.md` disagree, `progress.md` is the live state. If `progress.md` and `result.md` disagree, `result.md` is authoritative.
