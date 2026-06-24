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

Issue groups use the same machine-checked line, with one additional value: a group `progress.md` may start at `## Status: pending` because a group can be created (with `index.md` and child directories) before any work is requested. The terminal value is still `## Status: done`. See `reference/issue-groups.md` for the group lifecycle.

## 2. Update cadence

Update `progress.md` as steps land. Do not batch updates to the end. A reader should be able to tell, at any point during execution, what is done and what is in flight.

## 3. investigate does not use progress.md

`investigate` work is single-session and produces no file changes. There is no plan to execute against and no diff to summarize. The deliverable is `report.md`. If an investigation genuinely spans multiple sessions, treat the first session as a sub-investigation whose `report.md` feeds into a follow-up issue.

## 4. Anti-patterns

- **Wrong status line format.** Automation depends on the exact `## Status: in-progress` / `## Status: done` string.
- **Batch-updating at completion.** The log should reflect execution as it happens.
- **Logging narrative without mapping to planned steps.** Each entry should reference the step or item from `plan.md`.
- **Using progress.md as the diff summary.** That is `summary.md`'s role.

## 5. Quick checklist before flipping to done

- [ ] Status line is `## Status: done`.
- [ ] Every planned step (or per-item change) has an entry.
- [ ] Deviations from `plan.md` are called out.
- [ ] Multi-commit work has commits mapped to steps or items.

## 6. Issue group progress.md

An issue group has its own `progress.md` with the same machine-checked status line (`pending` / `in-progress` / `done`) but a different body. Instead of a step log against a plan, the body is an **Execution Log**: an ordered queue of `(child, verb, status)` rows. The same update-cadence principle applies — move a row to `in-progress` when the work starts and `done` when it lands, so a resumed session or reviewer can pick up cold.

The rules the group template cannot enforce on its own — only queue explicitly requested work (the Important Rule), and order rows child-outer / verb-inner (the Ordering Rule) — are in `reference/issue-groups.md` §6–§8. For section structure, see `templates/issue-group/progress.md`.
