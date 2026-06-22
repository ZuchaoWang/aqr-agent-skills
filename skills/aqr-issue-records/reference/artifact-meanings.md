Status: active

# Artifact meanings

# Artifact meanings

Four canonical artifacts. Each has a single role; mixing roles across files is the most common source of confused issue records.

## 1. task.md — what and why

Written first, at issue creation. The source of truth for **scope**.

`task.md` answers:

- What is the issue? (Goal)
- Why now? (Context)
- What is in scope? (Scope)
- What is out of scope? (Out of Scope)
- How do we know it is done? (Acceptance Criteria)
- What needs deciding before execution? (Open Questions)

`task.md` is not updated during execution. If scope changes mid-flight, open a new issue or convert the type — do not silently edit `task.md` to match what was actually built. If the scope change is small and clearly within the spirit of the original, an addendum at the bottom of `task.md` noting the change is acceptable; large scope changes deserve a new issue.

### 1.1 Acceptance criteria

Acceptance criteria are the most important part of `task.md`. They are what `result.md` checks against. Good criteria:

- Checkable from the diff or from a single command.
- Numbered so verification notes can refer to them by number.
- Specific about file paths, command output, or observable behavior.

Bad criteria:

- "The code is cleaner."
- "Tests pass." (too broad — be specific about which tests)
- "Docs are good."

## 2. plan.md — how

Written and updated during clarification and planning. The record of **current thinking** about how to satisfy the task.

`plan.md` answers:

- What did we find while investigating? (Investigation Findings)
- What are we planning to change? (Planned Documentation / Code / Test Changes)
- What needs deciding before execution? (Open Questions)
- What is the order of work? (Execution Steps)
- What did self-review surface? (Self-Review Notes)

`plan.md` evolves. Unlike `task.md`, the plan is expected to be updated as the work progresses — investigation findings get refined, execution steps get reordered or split, open questions get answered.

### 2.1 Open Questions

`Open Questions` is the gate. If it has unresolved items, execution (Phase 5 and later in most workflows) must not start. Resolve each question, then write "None." in this section before proceeding.

### 2.2 Self-Review Notes

Self-Review Notes capture the critique done before execution. They are not removed when execution starts — they are part of the record. A reviewer should be able to read them and see what concerns were considered, even if those concerns were ultimately accepted.

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

For short, atomic work — a doc update touching three files, a single-commit fix — `progress.md` is overhead. Skip it and write `result.md` directly when execution completes.

### 3.3 progress.md vs result.md

`progress.md` is the live log; `result.md` is the final summary. If both exist, `result.md` is authoritative — `progress.md` is kept for audit but does not override the result.

A common pattern: write `progress.md` during execution with `## Status: in-progress`, then at the end write `result.md` and set `progress.md` to `## Status: done` with a one-line pointer ("See result.md"). This keeps the status line machine-checkable while making `result.md` the human-facing summary.

## 4. result.md — what shipped

Written at completion. The **stable summary** that replaces `progress.md` as the human-facing record.

`result.md` answers:

- What shipped? (Summary)
- Which files changed? (Files Created / Updated / Deleted or Files Changed)
- How was it verified? (Verification Notes)
- What was accepted but not fully resolved? (Known Limitations)
- What follow-up work did this surface? (Follow-Up Issues)

### 4.1 Verification notes must map to acceptance criteria

For each numbered acceptance criterion in `task.md`, `result.md` should have a corresponding verification note. Number them to match. A criterion with no verification note is a criterion that was not actually checked.

### 4.2 Known Limitations

Known Limitations is where honesty lives. Anything accepted but not fully resolved goes here:

- Forward-looking references in docs.
- Code paths documented but not yet implemented.
- Edge cases deferred to follow-up.
- Tests skipped with a reason.

Each item should be checkable by a reviewer without re-running the work. "Performance could be better" is not checkable; "the N=1M case times out at 30s, deferred to issue <X>" is.

### 4.3 Follow-Up Issues

Follow-Up Issues is the inventory of next steps. Each item should be specific enough to become a new issue directory without further clarification. Vague items ("refactor later") are noise — either make them specific or drop them.

## 5. Reading order

For a cold reader picking up an issue:

1. `task.md` — what is the issue?
2. `plan.md` — what is the agreed approach? Any open questions?
3. `progress.md` (if present) — where did execution get to?
4. `result.md` (if present) — what shipped?

Conflict resolution: `task.md` wins for scope decisions. `plan.md` reflects current thinking. `progress.md` reflects live state. `result.md` is the authoritative summary at completion.
