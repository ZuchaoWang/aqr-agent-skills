# Plan guidelines

`plan.md` is the record of **current thinking** about how to satisfy the task. Where `task.md` is semantic, `plan.md` is enumerative: it lists the exact files, locations, and steps. The implementer can lift the file list directly from `plan.md` without re-deriving it.

For the task-vs-plan distinction, see `SKILL.md`. For section structure, see `templates/<type>/plan.md`. This doc covers how `plan.md` evolves and what each section must do.

## 1. plan.md evolves

Unlike `task.md`, the plan is expected to be updated as the work progresses — investigation findings get refined, execution steps get reordered or split, open questions get answered. `task.md` stays stable; `plan.md` reflects current thinking.

## 2. Open Questions is the gate

If `Open Questions` has unresolved items, execution must not start. Resolve each question, then write "None." in this section before proceeding.

Open Questions is where ambiguity is surfaced, not where it is buried. If the answer is not known, leave the question open and resolve it through clarification, an `investigate` sub-issue, or an explicit assumption documented as a follow-up.

## 3. Self-Review Notes are part of the record

Self-Review Notes capture the critique done before execution. They are not removed when execution starts — a reviewer should be able to see what concerns were considered, even if those concerns were ultimately accepted.

## 4. Per-type section differences

The `Planned Changes` shape varies by issue type:

- `doc-update/plan.md` has `Planned Documentation Changes` (with `Cleanup (deletions)` and `Docs to create or update` subsections).
- `code-update/plan.md` has `Planned Code Changes` and `Planned Test Changes`. It does NOT have a `Planned Documentation Changes` section — doc work goes in a separate `doc-update` issue.
- `fix/plan.md` has `Per-Item Change Plan` — one subsection per item from `task.md`.
- `investigate/plan.md` has `Investigation Approach` — ordered steps that describe what will be checked.

## 5. Anti-patterns

- **Starting execution with unresolved Open Questions.** The gate exists for a reason. Resolve first.
- **Removing Self-Review Notes once execution starts.** They are part of the record.
- **Re-deriving the file list during execution.** If the list changes, update `plan.md` — do not keep it in your head.
- **Mixing doc changes into a code-update plan.** Open a separate `doc-update` issue.
- **Vague execution steps.** Each step should be checkable from the diff alone.

## 6. Quick checklist before execution

- [ ] Planned Changes sections match the type's template.
- [ ] Every modification names where (file + function/section).
- [ ] Open Questions is "None." or execution has not started.
- [ ] Self-Review Notes captured, not stripped.
- [ ] Execution Steps are numbered and diff-checkable.
