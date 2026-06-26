---
name: aqr-issue-records
description: A lightweight issue-record format for doc or code changes, fixes, and investigations in a repo. Each issue lives in its own directory under issues/<YYYY-MM-DD>-<name>/ with task.md, progress.md, and one completion artifact — summary.md for a change, report.md for an investigation. No plan.md, no issue types, no issue groups. Manual-only — invoke via slash command.
disable-model-invocation: true
---

# aqr-issue-records

A lightweight way to record doc or code changes, ship fixes, or run investigations in a repo. Each issue lives in its own directory under `issues/` and is recorded through `task.md`, `progress.md`, and one completion artifact: `summary.md` for a change, `report.md` for an investigation. No `plan.md`, no issue types, no issue groups.

The distinction that shapes it:

- **Project docs are durable.** They live under `docs/` and are the real review surface.
- **Issue docs are disposable coordination.** They exist to make work reviewable and resumable: confirm the task was understood, see what is done or skipped, see what was decided, and give the next edit context on what already changed — not to capture every agent thought.
- **Agent plans are temporary cognition.** Planning is an agent behavior, not a repo artifact.

Hence no `plan.md`: the agent plans privately, and the repo holds only what a human would actually read — what was asked, what was decided, what is done.

This skill specifies the artifacts, their roles, and when an issue is warranted; the surrounding workflow (clarification, planning, execution, review) is owned by the user's methodology skill (such as Superpowers), not this one. The artifacts are plain text in the project's documentation style — anyone (the user, a reviewer, a future agent) can read them cold and understand what was asked, what was decided, what shipped, and what is still open.

## When to create an issue

Not every change needs an issue. Use judgment; the goal is the lightest record that still helps.

No issue needed:

- typo fixes
- small doc edits
- local refactors
- one-file changes
- obvious bug fixes

Issue needed:

- multi-session work
- risky refactor
- changes touching architecture or contracts
- tasks you may need to resume
- anything you want future-you to understand

If you are unsure, ask: will this issue help recovery, review, or accountability? If not, do not create it.

## Artifacts and where they live

Each issue lives in its own directory:

```text
issues/<YYYY-MM-DD>-<issue-name>/
├── task.md          # required, at creation
├── progress.md      # required, when work begins
├── summary.md       # at completion — for a change (code, docs, fix)
├── report.md        # at completion — for an investigation
└── assets/          # optional, user-provided inputs
```

`<YYYY-MM-DD>` is the project's local date when the issue was opened; `<issue-name>` is a short kebab-case name (use one the user supplies directly, if any).

These names and roles are part of the contract — do not rename them and do not invent additional required artifacts.

| File           | When it appears               | Role                                                                                                                                    |
| -------------- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `task.md`      | at issue creation             | The goal, context, scope, acceptance criteria, decisions, and open questions for the work.                                              |
| `progress.md`  | when work begins              | A coarse checkpoint log with a machine-checked status line. Not a diary.                                                                |
| `summary.md`   | at completion (change)        | What shipped and how it was verified. The completion artifact for any issue that produces file changes.                                 |
| `report.md`    | at completion (investigation) | The findings and answer. The completion artifact — and the deliverable — for a pure investigation that produces no file changes.        |

`task.md` and `progress.md` are always present. The issue closes with exactly one of `summary.md` or `report.md` — `summary.md` when it produced file changes, `report.md` when it was a pure investigation. If an issue was worth opening, it is worth a one-line completion artifact saying what happened.

`assets/` is an optional subdirectory for reference material the user provided as input: a screenshot of a bug, a PDF spec, sample data, an excerpt of discussion with another AI used to shape the issue's scope.

## Writing each artifact

### task.md

The definition of the work. Seven sections:

- **Goal** — what we want to change. One paragraph at the outcome level; do not enumerate files.
- **Context** — why the change is needed. Reference prior issues, approved design docs, client requirements, or external constraints; point at `assets/` for user-provided material.
- **Scope** — prose, what changes at a high level. No file list.
- **Out of Scope** — what will deliberately not be touched.
- **Acceptance Criteria** — numbered, checkable conditions (build/test/lint/type-check and behavior).
- **Decisions** — resolved decisions reached during initial clarification: direction chosen, trade-off accepted, constraint confirmed. Each entry is the decision plus a one-line why. This section is fixed once initial clarification finishes; decisions made during execution go in `progress.md`.
- **Open Questions** — unresolved sub-questions. Resolve all of them before planning or executing — an issue with open questions is not ready to proceed (see Decisions and open questions during execution).

Clarifications from the user are not a separate section — a clarification refines whichever section it pertains to, in place (a scope clarification updates Scope, a constraint updates Out of Scope, and so on). Only resolved decisions land in Decisions.

### progress.md

A checkpoint log, not a detailed diary. Four sections: an issue-level status line, the subtask list, and two structured event logs.

- **Status line** — `## Status: in-progress` when work begins, `## Status: done` at completion. It is the first content after the `# Progress` title, so automation can grep for `## Status: done` to find finished issues. This is the issue-level status, distinct from per-subtask status.
- **Subtasks** — the planned subtasks as a `Subtask | Status` list. The subtask name is set at plan/replan and does not change during execution; only its status changes. Status is blank for pending, or `in-progress` / `done` / `blocked` / `skipped` — `blocked` means cannot proceed (needs an answer or dependency), `skipped` means consciously dropped. A subtask marked `blocked` or `skipped` is also recorded under Blocked and skipped. Names must be unique — other sections reference subtasks by name.
- **Decisions** — execution-time decisions, logged as they happen. Each entry names the subtask it pertains to, the decision, and the context/why. This is the canonical record of decisions made during execution; `task.md`'s Decisions holds only what was resolved during initial clarification.
- **Blocked and skipped** — subtasks that did not reach done. Each entry names the subtask, the reason, and the impact: *blocking* (stops the whole issue, e.g. an unanswered question), *deferred* (only this subtask — pick it up later), or *dropped* (consciously not doing it). The subtask's status in the Subtasks list is `blocked` for blocking or deferred, `skipped` for dropped.

The Subtasks list reflects the plan of work; Decisions and Skipped are the event trail. Together they are the working memory `summary.md` is written from at completion — record events as they happen so nothing is lost to context compression or a session restart.

For large multi-part work (say, "refactor 100 modules"), do not create child issues unless you truly plan to review them individually. Use one issue: put the overall requirement and constraints in `task.md`, track coarse batches in `progress.md`, and let the agent plan and execute privately. The durable module docs are the review surface; `progress.md` just tracks which batches are done.

### summary.md — for a change

Use `summary.md` when the issue produced file changes (code, docs, or fix).

- **Changed** — what shipped, written so a later reviewer or follow-up issue can find each change. For each file touched: what changed and where (the doc section, function, or region). Include the decisions that shaped it (drawn from `progress.md`'s Decisions).
- **Verification** — how it was checked: tests run, manual checks, commands used and their outcome.
- **Remaining Issues** — only what is still open at the end: blocked subtasks (whether they block the whole issue or are deferred for later), drawn from `progress.md`'s Blocked and skipped. Consciously skipped subtasks are closed decisions, not remaining. Resolved decisions stay in `progress.md` (the material ones land in Changed).
- **Follow-Up Issues** (optional) — natural follow-ups discovered during the work.

### report.md — for an investigation

Use `report.md` when the issue was a pure investigation that produced no file changes. The report IS the deliverable.

- **Findings** — the investigation output, structured to fit the question (numbered findings, comparison table, timeline, …).
- **Answer** — the explicit answer to the question from `task.md`. If it depends on conditions, state them.
- **Confidence & Limitations** — how confident the answer is, and what would change it.
- **Follow-Up Issues** (optional) — actions the findings suggest.

If investigation findings should outlive the issue — they will be cited by future work, or the project keeps a research library — promote them to a permanent doc under `docs/` and link to it from `report.md`. Do not let `report.md` become the permanent home for findings that deserve one.

## Running an issue

An issue moves through phases. The boundary that matters is the start of plan and execute: before it `task.md` is open, after it `task.md` is frozen.

- **Create and clarify.** When the user says to create an issue, make the directory and `task.md`, then clarify the work — goal, context, scope, acceptance criteria, and the decisions reached. Resolve open questions as you go; `task.md` is still open, so keep refining it until initial clarification is done (Open Questions empty).
- **Checkpoint.** Stop at the end of clarification and ask the user whether to proceed to plan and execute. Do not start planning or execution until the user says go.
- **Plan and execute (the freeze point).** Once the user says to proceed, `task.md` is frozen — do not edit it again. Create `progress.md`, plan privately, and execute. Decisions that surface now go in `progress.md`. Carry on to completion unless the user stops you; do not pause to ask when a reasonable default exists.
- **Complete.** Write `summary.md` (or `report.md`) at the end.

Common situations:

- **User says "plan and execute" but `task.md` still has open questions** — finish clarification first; resolve Open Questions before any execution. The user's instruction already satisfies the checkpoint.
- **User says just "execute" (never planned)** — plan first, then execute. Planning is always done privately; "execute" does not skip it.
- **Partially executed (resuming)** — continue from where it stopped; do not redo finished subtasks.
- **User stops mid-execution to give a decision** — record it in `progress.md`'s Decisions; `task.md` stays frozen.
- **User does not intervene** — execute through to completion and close the issue.

### Decisions and open questions during execution

Once execution begins, `task.md` is frozen (see the freeze point above), so decisions and events that surface during execution go in `progress.md`, not back into `task.md`. `task.md`'s Open Questions gate execution: do not plan or execute while it is non-empty. Resolve every open question first.

New questions and decisions surface once work is underway. Handle each by case, and **log it in `progress.md` as it happens** — the Subtasks, Decisions, and Skipped sections together are the working memory `summary.md` is written from at completion, so nothing is lost to context compression or a session restart:

- **Simple, safe decision** — make it. Record it in `progress.md`'s Decisions (subtask, decision, context); do not edit `task.md` for it, since `task.md` is frozen once execution begins. The decisions that shaped what shipped are carried into `summary.md`'s Changed at completion.
- **Blocking question** — stop. Mark the affected subtask `blocked` in `progress.md`'s Subtasks and add an entry under Blocked and skipped (reason, impact: blocking); if still unresolved at the end, it becomes a Remaining Issue in `summary.md`.
- **Independent subtask blocked** — stop that subtask, continue the others. Mark it `blocked` in Subtasks and add an entry under Blocked and skipped (reason, impact: deferred — where it resumes); if still open at the end, it goes in `summary.md`'s Remaining Issues.

At completion, write `summary.md` by reviewing `progress.md`'s Subtasks, Decisions, and Blocked and skipped, plus the diff. The execution trail stays in `progress.md`; carry the decisions that shaped what shipped into Changed, and only what is still open — blocked subtasks, blocking or deferred — lands in `summary.md`'s Remaining Issues.

## When the user invokes this skill

The skill activates only on explicit slash invocation (e.g. `/aqr-issue-records`); it is not auto-applied based on project state, because the issue format is opinionated and not every project wants it. Once activated, read the intent from the user's prose; ask a few targeted questions only where the work is genuinely ambiguous, and skip what is already clear. Do not run for an hour and then ask. If something ambiguous turns up later and is not blocking, pick the most likely interpretation and proceed; stop to ask only when the ambiguity is material and no reasonable default exists.
