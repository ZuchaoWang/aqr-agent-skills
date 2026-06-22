Status: draft

# Task

## Goal

<!-- One paragraph: which defect set is being fixed and what "all fixed" looks like. Each defect lands in its own commit unless the plan explicitly says otherwise. -->

## Context

<!-- Where the defects came from: audit, regression, user report, failed test. Cite the source — audit log path, screen, failing test name, spec section. The implementation docs and specs are ground truth; this issue is a focused bug-fix pass, not a redesign. -->

## Scope

### Defects

<!-- Numbered list. Each defect is named and described semantically: symptom, root cause (with file:line), fix shape, spec or acceptance reference if applicable. The exact files and locations to touch belong in plan.md. -->

1. **<short name>** — symptom. Root cause at `path/to/file:LL`. Fix shape. Spec: `docs/...md §X`.
2. **<short name>** — symptom. Root cause at `path/to/file:LL`. Fix shape. Spec: `docs/...md §X`.

### Working directory constraint

<!-- Optional. State the tree or trees this issue is allowed to touch. Helps reviewers and the executing agent avoid scope creep. -->

All file changes confined to `<tree>/`.

## Out of Scope

<!-- Redesign, refactoring beyond the minimum needed for the fix, trees not touched, non-relevant defects, deferred work. -->

- ...

## Acceptance Criteria

<!-- Per-defect checkable condition plus global checks (build, test, lint, no regressions). -->

1. `build` exits 0 with no errors.
2. `test` exits 0; all tests pass.
3. ...
4. Per-defect: <condition that proves the fix>.

## Open Questions

<!-- "None." if empty. -->
