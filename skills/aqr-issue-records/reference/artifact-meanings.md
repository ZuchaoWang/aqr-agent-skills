# Artifact meanings

Five canonical artifacts. Each has a single role; mixing roles across files is the most common source of confused issue records.

## 1. task.md — what and why (semantic)

Written first, at issue creation. The source of truth for **scope**.

The user's initial brief is usually rough — a goal and some context. The agent's job during clarification is to drive every section of `task.md` to a state where it can be stated unambiguously: restate the goal in the agent's own words for the user to confirm, ask targeted questions about context and scope, draft sections the user can revise, and surface open questions rather than resolving them by assumption. The bar is "a reader who never met the user can pick up this `task.md` and execute correctly." If any section still needs conversation with the user to reach that bar, the section is not done.

`task.md` is **semantic**, not enumerative. It describes the issue at the level of "what behavior or knowledge changes" — not "which files change". The exact file list lives in `plan.md`. Keeping these separate means:

- A reader can understand the issue from `task.md` without getting bogged down in implementation detail.
- The implementer can lift the file list directly from `plan.md` without re-deriving it.
- Scope changes (a new file added to the work) update `plan.md`, not `task.md` — so `task.md` stays stable as the plan evolves.

`task.md` answers:

- What type of issue is this? (Type)
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
- Commit discipline. That is execution-time and lives in `progress.md` / `summary.md`.

### 1.2 Acceptance criteria

Acceptance criteria are typically drafted by the agent based on the goal and context, then confirmed or revised by the user. Users rarely specify criteria unprompted — the agent sees the structure of the work and is better placed to enumerate checkable outcomes. Draft them, show them to the user, and do not proceed to execution until the user accepts them.

Acceptance criteria are the most important part of `task.md`. They are what `summary.md` checks against. Good criteria:

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
- **Out-of-scope discovery.** Note it under `Follow-Up Issues` in `summary.md` (or `report.md` for investigate); do not expand `task.md` scope.

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

## 3. progress.md — what was carried out (execution log)

Required for doc-update, code-update, and fix. The log of which planned steps (or per-defect fixes) were carried out, in order. Created when execution begins.

`progress.md` answers:

- What is the current status? (Status)
- Which planned steps were carried out, and in what order? (Execution Log)
- Which commits landed, mapped to steps or defects? (Commits — included when multi-commit)
- What has been verified during execution? (Verification Notes or Verification Log)
- What deviated from the plan? (Notes)

`progress.md` is the rolling log of execution against the plan. Update it as steps land — not just at completion. A resumed session or a reviewer picks it up cold and sees what was done, what is in flight, and what deviated.

### 3.1 Status line — machine-checked

The first content after the `# Progress` title must be:

```markdown
## Status: in-progress
```

or

```markdown
## Status: done
```

The exact format matters. Automation may use it to decide whether to resume an issue or skip it. Other status values (`paused`, `blocked`, `abandoned`) are not part of this skill's contract; if a project needs them, define them in project-local docs and update this skill's reference accordingly.

### 3.2 progress.md vs summary.md

`progress.md` and `summary.md` serve different purposes:

- `progress.md` is the **execution log**. It tracks which planned steps were carried out, in order. It answers "did you do what you planned?".
- `summary.md` is the **diff and verification summary**. It tracks what files changed in the working tree, which commits landed, how each acceptance criterion was verified, what limitations were accepted, and what follow-ups surfaced. It answers "what shipped and how was it verified?".

A reviewer reading `progress.md` wants to see execution unfold against the plan. A reviewer reading `summary.md` wants to see the resulting diff and its verification. Both are required for doc-update, code-update, and fix.

### 3.3 investigate does not use progress.md

`investigate` work is single-session and produces no file changes. There is no plan to execute against and no diff to summarize. The deliverable is `report.md`. If an investigation genuinely spans multiple sessions, treat the first session as a sub-investigation whose `report.md` feeds into a follow-up issue.

## 4. summary.md — what shipped (diff and verification)

Written at completion for doc-update, code-update, and fix issues. The diff and verification summary: what files changed, which commits landed, how each acceptance criterion was verified, what was accepted but not fully resolved, and what follow-ups the work surfaced.

`summary.md` answers:

- What shipped? (Summary)
- Which files changed, where exactly? (Files Changed)
- How was it verified? (Verification Notes)
- What was accepted but not fully resolved? (Known Limitations)
- What follow-up work did this surface? (Follow-Up Issues)

See `reference/summary-guidelines.md` for the full guidance on writing a good summary.md.

### 4.1 Files Changed format

`Files Changed` is **always present** for these three types, even if the list is short. The format is structured so a reviewer can see at a glance what kind of change each file experienced:

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

If a category is empty, omit the subsection. Do not write "None." under each — the absence of the subsection is the signal.

### 4.2 Verification notes must map to acceptance criteria

For each numbered acceptance criterion in `task.md`, `summary.md` should have a corresponding verification note. Number them to match. A criterion with no verification note is a criterion that was not actually checked.

### 4.3 Known Limitations

Known Limitations is where honesty lives. Anything accepted but not fully resolved goes here:

- Forward-looking references in docs.
- Code paths documented but not yet implemented.
- Edge cases deferred to follow-up.
- Tests skipped with a reason.

Each item should be checkable by a reviewer without re-running the work. "Performance could be better" is not checkable; "the N=1M case times out at 30s, deferred to issue `<X>`" is.

### 4.4 Follow-Up Issues

Follow-Up Issues is the inventory of next steps. Each item should be specific enough to become a new issue directory without further clarification. Vague items ("refactor later") are noise — either make them specific or drop them. Each item should name a suggested type (`doc-update`, `code-update`, `fix`, `investigate`).

If there are no follow-ups, write "None."

## 5. report.md — what was found (investigate)

Written at completion for investigate issues. **The deliverable itself**, not a summary of deliverables produced elsewhere. There are no file changes to summarize, so the report IS the artifact.

`report.md` answers:

- What was the question, what is the bottom-line answer, how confident? (Summary)
- What did the investigation uncover? (Findings)
- What is the explicit answer to the question from `task.md`? (Answer)
- How confident is the answer, and what would change it? (Confidence and Limitations)
- What new questions surfaced? (Open Questions)
- What action does this finding suggest? (Follow-Up Issues)

See `reference/report-guidelines.md` for the full guidance on writing a good report.md.

### 5.1 The bar is higher than for summary.md

Because `report.md` IS the artifact (not a description of artifacts produced elsewhere), the quality of the issue equals the quality of the report. A vague or hedged report is a failed issue, even if the investigation itself was thorough. Write the answer clearly even when the answer is "we cannot know yet, because `<reason>`".

### 5.2 report.md does not have

- `Files Changed` (the investigation produced no file changes — if it did, the type was wrong).
- `Verification Notes` mapped to acceptance criteria (the role is played by the evidence chain inside `Findings`).

## 6. Reading order

For a cold reader picking up an issue:

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (doc/code/fix) — which planned steps were carried out, and what is still in flight?
4. `summary.md` (doc/code/fix) — what shipped in the diff, what was verified, what is left.
   `report.md` (investigate) — what was found, what is the answer.

Conflict resolution: `task.md` wins for scope decisions. `plan.md` reflects current thinking. `progress.md` reflects execution state. `summary.md` (or `report.md`) is the authoritative summary at completion.
