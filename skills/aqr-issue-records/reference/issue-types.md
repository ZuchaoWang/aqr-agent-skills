# Issue types

Four issue types: `doc-update`, `code-update`, `fix`, `investigate`. Pick the type based on what the issue primarily produces. Each issue has exactly one type.

For the per-type artifact sets, see the table in `SKILL.md`. For how to write each artifact, see `reference/<artifact>-guidelines.md`. This doc covers type choice only.

## 1. Decision tree

```text
Does the issue produce any file change in the repo?
├── no → investigate
└── yes → Is the change small and atomic (one cohesive change, no subtasks)?
        ├── yes → fix
        └── no → Does it change BOTH source code/tests AND docs?
                ├── yes → REJECT — split: doc-update first, then code-update follow-up
                └── no → Is it primarily source code or tests?
                        ├── yes → code-update
                        └── no → doc-update
```

A `fix` may touch any combination of code, tests, docs, config, infra, and data files in one issue — the small-and-atomic check is about size and cohesion, not about which trees are touched. Anything that is not small follows the right branch, where code and docs cannot share an issue.

## 2. doc-update

Docs-only issue. Produces changes under `docs/` (or root dotfiles / config that are documentation-shaped, like `.markdownlint.json` or `CLAUDE.md`).

Used for: product specs, architecture docs, implementation docs, research notes that get a permanent home under `docs/research/`, data source docs, convention updates, migration analysis.

Boundaries:

- If doc work surfaces a code change, open a separate `code-update` rather than expanding the doc-update.
- `investigate` produces findings in `report.md`; `doc-update` promotes findings to permanent files under `docs/`. A common flow is `investigate` → `doc-update` follow-up.

## 3. code-update

Code and test change issue. **Code-only.** If a doc needs to change to stay consistent with the new code, open a separate `doc-update` issue rather than expanding this issue's scope.

Why split: a large doc update needs human review before code follows it. Review may surface revisions; once the doc lands, the code-update follows an agreed direction. An issue does not pause mid-execution for re-clarification — so doc work and code work must live in separate issues, giving the doc its own review surface and the code a stable target.

A code-update should usually reference an approved doc rather than redesign. If the implementation surfaces a design question the docs do not answer, open a separate `investigate` to answer it, then (if needed) a `doc-update` to record the answer, then resume the code-update.

## 4. fix

A `fix` is any small change — one cohesive change, no subtasks, lands in a small number of commits. It can be corrective (bug fix), additive (small feature), subtractive (cleanup), or anything else, as long as it is small. It can touch any combination of code, tests, docs, config, infra, and data in one issue.

Small changes do not benefit from the doc-first-then-code structure — forcing them through separate issues creates artificial coordination overhead, so the skill allows a `fix` to span trees that larger work cannot.

Boundary: if the change is too large to fit "small and atomic", route via the right branch of the decision tree. If it touches both code and docs and is large, split (see §1 tree).

## 5. investigate

Pure investigation, feasibility check, or lookup. **Produces no file changes.** Findings live in `report.md`.

Used for: feasibility checks ("can we do X with tool Y?"), debugging investigations ("why does this sometimes fail under load?"), comparative analysis ("option A vs option B for our use case"), codebase audits ("audit the auth surface for outdated patterns"), environment checks ("is the remote server accessible from staging?"), quick lookups ("does this library already support X?"), pre-implementation research that does not yet warrant a permanent doc.

Boundaries:

- Use `investigate` when findings are time-sensitive, specific to one issue's context, or the user does not want to commit to maintaining a permanent doc.
- Use `doc-update` (research) when findings should outlive the issue, will be cited by future work, or the project maintains a research library under `docs/research/`.
- A common flow: `investigate` concludes with a follow-up `doc-update` that promotes the findings to `docs/research/`.

What investigate is not:

- Not a license to skip documentation. If findings warrant a permanent record, open a `doc-update` follow-up.
- Not a synonym for "small". A two-week feasibility study is an `investigate`; a five-minute code change is a `code-update` or `fix`.
- Not a dumping ground for orphan work. If you cannot state the question the investigation answers, the issue is not ready.
