# Lifecycle

How an issue moves from creation to completion. Covers type declaration at creation, the verbs used to drive an issue through phases, and how to handle replanning when the work is non-linear.

## 1. Type is declared at creation

An issue's type is one of `doc-update`, `code-update`, `fix`, `investigate`. It is declared at creation time through a short triage — see `issue-types.md` for the full decision tree. The type is recorded under `## Type` at the top of `task.md`.

Treat the type as revisable during clarification. If scope shifts enough to change the type before execution begins, swap the templates and rewrite the type-specific sections of `task.md` rather than keeping `task.md` type-neutral. Once execution starts, the type is fixed; type-changing scope growth from that point is a split, not a relabel.

The fix type carries an additional constraint: a fix that touches both code and docs must be small and atomic. Larger work that needs both code and docs is split into a `doc-update` first, then a `code-update` follow-up. See §4 of `issue-types.md`.

## 2. Verbs

The skill exposes four primitive verbs. Each verb is an entry point that drives an issue to a specific phase boundary.

### 2.1 create

Make the issue directory under `issues/<YYYYMM>/<issue-id>/`, declare the type, and draft `task.md` through clarification. Ends when every section of `task.md` is unambiguous and Open Questions reads "None.".

### 2.2 plan

Draft `plan.md` through investigation and self-review. Ends when Open Questions reads "None.", Self-Review Notes is filled, and (for non-`investigate` types) Execution Steps or the per-type equivalent is enumerated.

### 2.3 execute

Carry out `plan.md`. Writes `progress.md` as steps land; ends with `summary.md` (or `report.md` for `investigate`) and `progress.md`'s `## Status: done`.

### 2.4 revise

Re-open `task.md` or `plan.md` when something surfaces mid-execution. Log the trigger in `progress.md` Notes, update the relevant artifact, and resume `execute`. See §3.

### 2.5 Bundled operations

Verbs compose. To run `create` then `plan` in one invocation, say so; the agent chains them. To run `plan` then `execute`, same. For the full lifecycle in one go, name all three.

### 2.6 Bare invocation (resume)

Bare invocation with no verb means "pick up wherever this issue is":

- Only `task.md` exists → run `plan`.
- `task.md` + `plan.md` exist, no `progress.md` → run `execute`.
- `progress.md` shows `## Status: in-progress` → continue `execute`.
- `progress.md` shows `## Status: done` → report the issue state, do nothing.

## 3. Revising mid-flight

The lifecycle is not strictly linear. If a failing test, a critique from a methodology skill, or a newly discovered constraint invalidates the plan, the agent runs `revise`:

1. Log the trigger in `progress.md` Notes — what surfaced, when, and which artifact it affects.
2. Update `plan.md` (or, rarely, `task.md` — see §1.3 of `artifact-meanings.md` for the rules on scope-change handling).
3. Resume `execute` from the new plan.

`plan.md` is a living document; updates during execution are expected, not exceptional. `progress.md` records each replan so the audit trail survives. The artifacts are the source of truth; the verbs are pointers into them.

## 4. Where methodology skills plug in

This skill defines the artifacts and the verbs. It does not prescribe *how* a phase is conducted:

- Clarification dialogue technique → methodology skill (e.g. Superpowers).
- Critique and self-review technique → methodology skill.
- TDD discipline → methodology skill.
- Debugging technique → methodology skill.
- Verification commands (build, test, lint) → project toolchain.

The methodology skill drives the technique; this skill defines what gets written down at each phase boundary. When a methodology skill produces output (a critique, a debug session, a test result) that belongs in the issue record, capture it in the appropriate artifact — usually `plan.md` Self-Review Notes, `progress.md` Notes, or `summary.md` Verification Notes.
