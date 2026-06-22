Status: active

# Issue types

Four issue types: `doc-update`, `code-update`, `fix`, `investigate`. Each type has its own template set under `templates/`. Pick the type based on what the issue primarily produces; the type determines which artifacts are required.

## 1. Decision tree

```text
Does the issue produce any file change in the repo?
├── no → investigate
└── yes → Is the issue primarily fixing something broken?
        ├── yes → fix
        └── no → Is the issue primarily changing source code or tests?
                ├── yes → code-update
                └── no → doc-update
```

Each issue has exactly one type. If scope expands mid-flight such that the type would change, the right move is usually to split the issue rather than relabel it. See "Anti-patterns" below.

## 2. doc-update

Docs-only issue. Produces changes under `docs/` (or root dotfiles / config that are documentation-shaped, like `.markdownlint.json` or `CLAUDE.md`).

Used for:

- Product specs and screen designs.
- Architecture docs and design decisions.
- Implementation docs (per-module reference).
- Research notes that get a permanent home under `docs/research/`.
- Data source docs, schemas, processing notes.
- Convention updates (code style, doc style, testing policy).
- Migration analysis from prior codebases.

### 2.1 Boundary with investigate

`investigate` produces findings but no file changes; the findings live in `report.md`. `doc-update` produces permanent doc files under `docs/`. A common flow: an `investigate` issue answers a question, and one of its follow-ups is a `doc-update` that writes the answer into `docs/research/` or `docs/architecture/` so the answer outlives the issue record.

### 2.2 Boundary with code-update

`doc-update` does not ship code. If doc work surfaces a code change, open a separate `code-update` issue rather than expanding the doc-update.

### 2.3 Artifacts

Required: `task.md`, `plan.md`, `summary.md`.

Recommended for large doc sets: `progress.md`. Doc work that spans more than a handful of files, or that spans multiple sessions, benefits from a live checkpoint as much as code work does.

### 2.4 Template sections specific to doc-update

`task.md` describes the doc area semantically (Goal, Context, Scope as prose, Out of Scope, Acceptance Criteria, Open Questions) — it does not enumerate files. `plan.md` enumerates the exact docs to create, update, or delete under `Planned Documentation Changes`. `summary.md` lists every touched file under `Files Changed` with per-section locations for modifications.

## 3. code-update

Code and test change issue. **Code-only.** If a doc needs to change to stay consistent with the new code, open a separate `doc-update` issue rather than expanding this issue's scope.

The reasoning: code and docs change for different reasons, on different cadences, and are reviewed by different audiences. Mixing them produces plans that are incomplete-as-design and incomplete-as-implementation. A 1-line doc tweak forced into a code-update tends to either expand into a doc rewrite or get dropped on the floor; both are worse than a clean separate issue.

### 3.1 Artifacts

Required: `task.md`, `plan.md`, `summary.md`.

Recommended: `progress.md` for any non-trivial implementation. Code work has more failure modes than doc work — silent regressions, partial migrations, cascading test failures — and a live checkpoint makes resume and review meaningfully easier.

### 3.2 Template sections specific to code-update

`plan.md` has `Planned Code Changes` and `Planned Test Changes` as separate sections. It does NOT have a `Planned Documentation Changes` section — that belongs in a separate `doc-update` issue. `task.md` has matching semantic scope (`Goal`, `Context`, `Scope`, `Out of Scope`, `Acceptance Criteria`, `Open Questions`) without enumerating files.

### 3.3 Relationship to docs

A code-update issue should usually reference an approved doc rather than redesign. If the implementation surfaces a design question that the docs do not answer, the right move is usually to open a separate `investigate` issue to answer the question, then (if needed) a `doc-update` to record the answer, then resume the code-update.

## 4. fix

Small correction issue. Used for:

- Bugs (functional defects).
- Failed tests.
- Broken docs (typos, wrong paths, stale references).
- Inconsistencies between docs and code.
- Small cleanup that is too narrow for a full code-update.

Can touch code, docs, or both, but should stay narrow. A fix that grows to include new behavior is no longer a fix; convert it to a `code-update` (or split it) before scope expands.

### 4.1 Why fix can touch both code and docs

Unlike `code-update`, `fix` is allowed to touch both code and docs in the same issue. The reasoning: a defect often has a code fix and a doc fix that are inseparable as a unit (the doc said X, the code did Y, the fix changes both). Splitting these into two issues creates artificial coordination overhead.

The boundary with `code-update`: if the change introduces new behavior, it is a `code-update`. If it corrects existing behavior to match what was intended, it is a `fix`.

### 4.2 Artifacts

Required: `task.md`, `plan.md`, `summary.md`.

Recommended: `progress.md` — fixes often reveal sibling call sites with the same bug, and tracking per-defect verification in `progress.md` keeps the `summary.md` cleaner.

### 4.3 Commit discipline

Fixes usually commit per defect. Each commit message should name the defect (e.g. `fix(<area>): <short description> — <root-cause>`). When live verification reveals that an initial fix missed a sibling site, write a follow-up commit rather than amending — the audit trail matters more than the clean log.

## 5. investigate

Pure investigation or feasibility check. **Produces no file changes.** Findings live in `report.md`.

Used for:

- Feasibility checks ("can we do X with tool Y?").
- Debugging investigations ("why does this sometimes fail under load?").
- Comparative analysis ("option A vs option B for our use case").
- Lookups ("does this library already support X?").
- Pre-implementation research that does not yet warrant a permanent doc.

### 5.1 Boundary with doc-update (research subtype)

`doc-update` research writes findings to a permanent home under `docs/research/`. `investigate` keeps findings in `report.md` only. Use `investigate` when:

- The findings are likely time-sensitive (a decision will be made and the investigation becomes moot).
- The findings are specific to one issue's context and do not generalize.
- The user does not want to commit to maintaining a permanent doc.

Use `doc-update` (research) when:

- The findings should outlive the issue.
- The findings will be cited by future work.
- The project maintains a research library under `docs/research/`.

A common flow: an `investigate` issue concludes with a follow-up `doc-update` issue that promotes the findings to `docs/research/`.

### 5.2 Artifacts

Required: `task.md`, `report.md`.

Optional: `plan.md` — only for non-trivial investigations where the approach itself deserves review (multiple stages, expensive experiments, controversial sources). For simple lookups, skip `plan.md`.

Not used: `progress.md`, `summary.md`. Investigations are usually single-session; if an investigation genuinely spans multiple sessions, treat the first session as a sub-investigation and the second as a follow-up issue. And because there are no file changes, there is nothing for a `summary.md` to summarize — the `report.md` IS the artifact.

### 5.3 Template sections specific to investigate

`task.md` frames the question semantically. `report.md` IS the deliverable — it has `Summary`, `Findings`, `Answer`, `Confidence and Limitations`, `Open Questions`, `Follow-Up Issues`. It does NOT have a `Files Changed` section.

### 5.4 What investigate is not

- It is not a license to skip documentation. If findings warrant a permanent record, open a `doc-update` follow-up.
- It is not a synonym for "small". A two-week feasibility study is an `investigate`. A five-minute code change is a `code-update` or `fix`.
- It is not for orphan work that does not fit elsewhere. If you cannot articulate the question the investigation answers, the issue is not ready to open.

## 6. Anti-patterns

- **Mixing types.** An issue starts as one type and ends as another. If scope changes type mid-flight, split the issue — do not silently relabel.
- **Fix-bloat.** A `fix` that grows new features or refactors surrounding code. Split before scope creeps.
- **Code-update with doc changes.** A `code-update` that also touches docs. Move the doc work to a separate `doc-update` issue. (The reverse does not apply to `fix` — see §4.1.)
- **Doc-update shipping code.** A `doc-update` issue that touches source files. Either the docs led to discoveries that need a separate `code-update`, or the type was wrong from the start.
- **Investigate as a dumping ground.** An `investigate` issue that "explores X" without a clear question. If you cannot state the question, the issue is not ready.
- **Skipping report.md on investigate.** An `investigate` issue with `task.md` and `plan.md` but no `report.md` is incomplete. The `report.md` IS the artifact — without it, nothing was produced.
