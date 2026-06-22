Status: active

# Artifact meanings

Four canonical artifacts. Each has a single role; mixing roles across files is the most common source of confused issue records.

## 1. task.md — what and why (semantic)

Written first, at issue creation. The source of truth for **scope**.

`task.md` is **semantic**, not enumerative. It describes the issue at the level of "what behavior or knowledge changes" — not "which files change". The exact file list lives in `plan.md`. Keeping these separate means:

- A reader can understand the issue from `task.md` without getting bogged down in implementation detail.
- The implementer can lift the file list directly from `plan.md` without re-deriving it.
- Scope changes (a new file added to the work) update `plan.md`, not `task.md` — so `task.md` stays stable as the plan evolves.

`task.md` answers:

- What is the issue? (Goal)
- Why now? (Context)
- What kind of change is this, at a high level? (Scope)
- What is explicitly not in scope? (Out of Scope)
- How do we know it is done? (Acceptance Criteria)
- What needs deciding before execution? (Open Questions)

### 1.1 What does NOT belong in task.md

- File-by-file bullet lists. Those belong in `plan.md`.
- Implementation steps. Those belong in `plan.md`'s `Execution Steps`.
- Test names. Those belong in `plan.md`'s `Planned Test Changes`.
- Commit discipline. That is execution-time and lives in `progress.md` / `result.md`.

### 1.2 Acceptance criteria

Acceptance criteria are the most important part of `task.md`. They are what `result.md` checks against. Good criteria:

- Checkable from the diff or from a single command.
- Numbered so verification notes can refer to them by number.
- Specific about file paths, command output, or observable behavior.

Bad criteria:

- "The code is cleaner."
- "Tests pass." (too broad — be specific about which tests)
- "Docs are good."

### 1.3 When task.md changes

`task.md` is not updated during execution. If scope changes mid-flight:

- **Small, in-spirit change.** Add an addendum at the bottom of `task.md` noting the change. Do not silently edit the original scope.
- **Type-changing scope change.** Open a new issue of the correct type. Do not relabel this one.
- **Out-of-scope discovery.** Note it under `Follow-Up Issues` in `result.md`; do not expand `task.md` scope.

## 2. plan.md — how (enumerative)

Written and updated during clarification and planning. The record of **current thinking** about how to satisfy the task. Where `task.md` is semantic, `plan.md` is enumerative: it lists the exact files, locations, and steps.

`plan.md` answers:

- What did we find while investigating? (Investigation Findings)
- What are we planning to change, concretely? (Planned Documentation / Code / Test Changes — which sections appear depends on issue type)
- What needs deciding before execution? (Open Questions)
- What is the order of work? (Execution Steps)
- What did self-review surface? (Self-Review Notes)

`plan.md` evolves. Unlike `task.md`, the plan is expected to be updated as the work progresses — investigation findings get refined, execution steps get reordered or split, open questions get answered.

### 2.1 Open Questions

`Open Questions` is the gate. If it has unresolved items, execution (Phase 5 and later in most workflows) must not start. Resolve each question, then write "None." in this section before proceeding.

### 2.2 Self-Review Notes

Self-Review Notes capture the critique done before execution. They are not removed when execution starts — they are part of the record. A reviewer should be able to read them and see what concerns were considered, even if those concerns were ultimately accepted.

### 2.3 Per-type section differences

- `doc-update/plan.md` has `Planned Documentation Changes` (with `Cleanup (deletions)` and `Docs to create or update` subsections).
- `code-update/plan.md` has `Planned Code Changes` and `Planned Test Changes`. It does NOT have a `Planned Documentation Changes` section — doc work goes in a separate `doc-update` issue.
- `fix/plan.md` has `Per-Defect Fix Plan` — one subsection per defect from `task.md`.
- `investigate/plan.md` has `Investigation Approach` — ordered steps that describe what will be checked.

## 3. progress.md — where execution is

Optional. Created when execution begins. The **live checkpoint** for resumability.

`progress.md` answers:

- What is the current status? (Status)
- What is done so far? (Summary)
- What has been verified? (Verification Notes)
- What deviated from the plan? (Notes)

### 3.1 Status line — machine-checked

The first content after the `# Progress` title must be:

```
## Status: in-progress
```

or

```
## Status: done
```

The exact format matters. Automation may use it to decide whether to resume an issue or skip it. Other status values (`paused`, `blocked`, `abandoned`) are not part of this skill's contract; if a project needs them, define them in project-local docs and update this skill's reference accordingly.

### 3.2 When progress.md is not needed

For short, atomic work — a doc update touching three files, a single-commit fix, a one-shot investigation — `progress.md` is overhead. Skip it and write `result.md` directly when execution completes.

### 3.3 progress.md vs result.md

`progress.md` is the live log; `result.md` is the final summary. If both exist, `result.md` is authoritative — `progress.md` is kept for audit but does not override the result.

A common pattern: write `progress.md` during execution with `## Status: in-progress`, then at the end write `result.md` and set `progress.md` to `## Status: done` with a one-line pointer ("See result.md"). This keeps the status line machine-checkable while making `result.md` the human-facing summary.

### 3.4 investigate rarely uses progress.md

`investigate` work is usually single-session. If an investigation genuinely spans multiple sessions, the cleaner move is to treat the first session as a sub-investigation whose result feeds into a follow-up issue, rather than maintaining a long-running `progress.md`.

## 4. result.md — what shipped

Written at completion. The **stable summary** that replaces `progress.md` as the human-facing record.

`result.md` answers:

- What shipped? (Summary)
- Which files changed, where exactly? (Files Changed) — not used for `investigate`.
- How was it verified? (Verification Notes) — not used for `investigate`.
- What was accepted but not fully resolved? (Known Limitations) — `Confidence and Limitations` for `investigate`.
- What follow-up work did this surface? (Follow-Up Issues)

### 4.1 Files Changed format

For `doc-update`, `code-update`, and `fix` issues, `Files Changed` is **always present**, even if the list is short. The format is structured so a reviewer can see at a glance what kind of change each file experienced:

```markdown
## Files Changed

### Created

- `path/to/new/file` — one-line role.

### Modified

- `path/to/existing/file`
  - `<function or section>` — what changed.
  - `<function or section>` — what changed.

### Deleted

- `path/to/removed/file` — reason.
```

For modifications, every entry must name **where in the file** the change happened — function name, class name, section heading, or line range. "Updated `foo.py`" is not acceptable; "`foo.py` — `_calculate_total`: handle empty input" is.

If a category is empty (no files created, no files deleted), omit the subsection. Do not write "None." under each — the absence of the subsection is the signal.

### 4.2 Verification notes must map to acceptance criteria

For each numbered acceptance criterion in `task.md`, `result.md` should have a corresponding verification note. Number them to match. A criterion with no verification note is a criterion that was not actually checked.

For `investigate` issues, verification is replaced by `Findings` and `Answer` — the investigation's evidence chain plays the role of verification.

### 4.3 Known Limitations

Known Limitations is where honesty lives. Anything accepted but not fully resolved goes here:

- Forward-looking references in docs.
- Code paths documented but not yet implemented.
- Edge cases deferred to follow-up.
- Tests skipped with a reason.

Each item should be checkable by a reviewer without re-running the work. "Performance could be better" is not checkable; "the N=1M case times out at 30s, deferred to issue <X>" is.

For `investigate`, this section is `Confidence and Limitations` — how confident is the answer, what would change it, what was NOT checked.

### 4.4 Follow-Up Issues

Follow-Up Issues is the inventory of next steps. Each item should be specific enough to become a new issue directory without further clarification. Vague items ("refactor later") are noise — either make them specific or drop them. Each item should name a suggested type (`doc-update`, `code-update`, `fix`, `investigate`).

If there are no follow-ups, write "None."

## 5. Reading order

For a cold reader picking up an issue:

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (if present) — where did execution get to?
4. `result.md` (if present) — what shipped, what was verified, what is left.

Conflict resolution: `task.md` wins for scope decisions. `plan.md` reflects current thinking. `progress.md` reflects live state. `result.md` is the authoritative summary at completion.
