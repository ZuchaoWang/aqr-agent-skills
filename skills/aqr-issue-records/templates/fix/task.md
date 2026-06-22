# Task

## Type

`fix`

<!-- Set at creation. A fix that touches both code and docs must be small and atomic — one item, no subtasks. Larger work that needs both code and docs is split: doc-update first, then a code-update follow-up. If the work outgrows this type, split into a new issue rather than relabeling. -->

## Goal

<!-- One paragraph: what is changing and what "done" looks like. Each item lands in its own commit unless the plan explicitly says otherwise. -->

## Context

<!-- Where the work came from: audit, regression, user report, failed test, small feature request, cleanup. Cite the source — audit log path, screen, failing test name, spec section. The implementation docs and specs are ground truth; this issue is a focused change pass, not a redesign. -->

## Scope

### Items

<!-- Numbered list. Each item is named and described semantically: what changes, why, location (with file:line if known), spec or acceptance reference if applicable. The exact files and locations to touch belong in plan.md. -->

1. **<short name>** — what changes and why. Location: `path/to/file:LL`. Spec: `docs/...md §X`.
2. **<short name>** — what changes and why. Location: `path/to/file:LL`. Spec: `docs/...md §X`.

### Working directory constraint

<!-- Optional. State the tree or trees this issue is allowed to touch. Helps reviewers and the executing agent avoid scope creep. -->

All file changes confined to `<tree>/`.

## Out of Scope

<!-- Redesign, refactoring beyond the minimum needed for the change, trees not touched, out-of-scope items, deferred work. -->

- ...

## Acceptance Criteria

<!-- Per-item checkable condition plus global checks (build, test, lint, no regressions). -->

1. `build` exits 0 with no errors.
2. `test` exits 0; all tests pass.
3. ...
4. Per-item: <condition that proves the change>.

## Open Questions

<!-- "None." if empty. -->
