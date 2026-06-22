# Progress guidelines

`progress.md` is the rolling log of execution against `plan.md`. It is required for `doc-update`, `code-update`, and `fix`. Update it as steps land — not just at completion. A resumed session or a reviewer picks it up cold and sees what was done, what is in flight, and what deviated.

For the progress-vs-summary distinction (execution log vs diff summary), see `SKILL.md`. For section structure, see `templates/<type>/progress.md`. This doc covers the rules the templates cannot enforce on their own.

## 1. Status line — machine-checked

The first content after the `# Progress` title must be:

```markdown
## Status: in-progress
```

or

```markdown
## Status: done
```

The exact format matters. Automation may use it to decide whether to resume an issue or skip it. Other status values (`paused`, `blocked`, `abandoned`) are not part of this skill's contract; if a project needs them, define them in project-local docs and update this skill's reference accordingly.

## 2. Update cadence

Update `progress.md` as steps land. Do not batch updates to the end. A reader should be able to tell, at any point during execution, what is done and what is in flight.

## 3. investigate does not use progress.md

`investigate` work is single-session and produces no file changes. There is no plan to execute against and no diff to summarize. The deliverable is `report.md`. If an investigation genuinely spans multiple sessions, treat the first session as a sub-investigation whose `report.md` feeds into a follow-up issue.

## 4. Anti-patterns

- **Wrong status line format.** Automation depends on the exact `## Status: in-progress` / `## Status: done` string.
- **Batch-updating at completion.** The log should reflect execution as it happens.
- **Logging narrative without mapping to planned steps.** Each entry should reference the step or defect from `plan.md`.
- **Using progress.md as the diff summary.** That is `summary.md`'s role.

## 5. Quick checklist before flipping to done

- [ ] Status line is `## Status: done`.
- [ ] Every planned step (or per-defect fix) has an entry.
- [ ] Deviations from `plan.md` are called out.
- [ ] Multi-commit work has commits mapped to steps or defects.
