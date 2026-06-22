Status: active

# Issue types

# Issue types

Three issue types: `doc-update`, `code-update`, `fix`. Each type has its own template set under `templates/`. Pick the type based on what the issue primarily changes; the type determines which artifacts are required and which sections the templates emphasize.

## 1. Decision tree

```
Is the issue primarily fixing something broken?
├── yes → fix
└── no → Is the issue primarily changing source code or tests?
        ├── yes → code-update
        └── no → doc-update
```

If an issue touches both code and docs substantially, prefer `code-update` and list the doc updates under "Docs to update" in the task template. A doc-update issue should not ship code changes; if code changes become necessary, re-scope the issue as code-update or split it.

## 2. doc-update

Docs-only issue. Used for:

- Product specs and screen designs.
- Architecture docs and design decisions.
- Implementation docs (per-module reference).
- Research notes, background, related-work comparisons.
- Data source docs, schemas, processing notes.
- Convention updates (code style, doc style, testing policy).
- Migration analysis from prior codebases.

`research` is a `doc-update` subtype. Research changes knowledge records, not source code, so it lives under the doc-update format. A research issue typically adds new files under `docs/research/` and may also update `docs/architecture/` or `docs/implementation/` to reflect what the research concluded.

### 2.1 Artifacts

Required: `task.md`, `plan.md`, `result.md`.

Optional: `progress.md` — only if the doc set is large enough that intermediate checkpoints matter (more than a handful of files, or work spanning multiple sessions).

### 2.2 Template sections specific to doc-update

`task.md` has explicit `Docs to create or update` and `Docs to delete` subsections under `Scope`. `plan.md` has a `Planned Documentation Changes` section with `Cleanup (deletions)` and `Docs to create or update` subsections. `code-update` and `fix` templates omit these — they would just be empty.

## 3. code-update

Code and test change issue. May update docs if needed to keep behavior/API/module docs consistent with the new code. Should usually refer to approved docs or prior doc-update results rather than re-deriving design in the plan.

### 3.1 Artifacts

Required: `task.md`, `plan.md`, `result.md`.

Recommended: `progress.md` for any non-trivial implementation. Code work has more failure modes than doc work — silent regressions, partial migrations, cascading test failures — and a live checkpoint makes resume and review meaningfully easier.

### 3.2 Template sections specific to code-update

`plan.md` has `Planned Documentation Changes`, `Planned Code Changes`, and `Planned Test Changes` as separate sections. `task.md` has matching `Code to add or change`, `Tests to add or change`, and `Docs to update` subsections under `Scope`.

### 3.3 Relationship to docs

A code-update issue should usually reference an approved doc rather than redesign. If the implementation surfaces a design question that the docs do not answer, the right move is usually to open a separate doc-update issue, get it approved, then resume the code-update. Mixing design and implementation in one issue tends to produce plans that are both incomplete-as-design and incomplete-as-implementation.

## 4. fix

Small correction issue. Used for:

- Bugs (functional defects).
- Failed tests.
- Broken docs (typos, wrong paths, stale references).
- Inconsistencies between docs and code.
- Small cleanup that is too narrow for a full code-update.

Can touch code, docs, or both, but should stay narrow. A fix that grows to include new behavior is no longer a fix; convert it to a code-update (or split it) before scope expands.

### 4.1 Artifacts

Required: `task.md`, `plan.md`, `result.md`.

Recommended: `progress.md` — fixes often reveal sibling call sites with the same bug, and tracking per-defect verification in `progress.md` keeps the result.md cleaner.

### 4.2 Commit discipline

Fixes usually commit per defect. Each commit message should name the defect (e.g. `fix(<area>): <short description> — <root-cause>`). When live verification reveals that an initial fix missed a sibling site, write a follow-up commit rather than amending — the audit trail matters more than the clean log.

## 5. Anti-patterns

- **Mixing types.** An issue starts as doc-update and ends up shipping code. If scope changes type mid-flight, update `task.md` to reflect the new type and note the change in `result.md`. Do not silently cross type boundaries.
- **Fix-bloat.** A fix that grows new features or refactors surrounding code. Split before scope creeps.
- **Code-update without doc reference.** A code-update plan that re-derives design from scratch instead of pointing at an approved doc. Usually a sign that a doc-update issue should have happened first.
- **Doc-update shipping code.** A doc-update issue that touches source files. Either the docs led to discoveries that need a separate code-update, or the type was wrong from the start.
