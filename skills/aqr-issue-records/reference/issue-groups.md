# Issue groups

The issue system has two object types: the **Issue** and the **Issue Group**. An Issue is a single executable unit of work (see `SKILL.md` and `reference/issue-types.md`). An Issue Group is a container that batches multiple related issues so they can be managed — created, planned, executed — together. This doc covers the group.

## 1. Two object types

- **Issue** — one executable unit of work. Has a type (`doc-update`, `code-update`, `fix`, `investigate`) and the artifact set for that type. Lives in its own directory.
- **Issue Group** — a batch-execution container for related issues. Has no type of its own. Holds child issues and tracks their progress as an ordered queue. It is an execution mechanism, not a planning artifact.

Both live under `issues/`. They are told apart structurally: a group directory contains `index.md` and a `children/` subdirectory; an issue directory contains `task.md`.

## 2. Structure and naming

An Issue Group lives at:

```text
issues/<YYYYMM>/<YYYYMMDD>-<group-name>/
  index.md
  progress.md
  children/
    <index>-<issue-name>/        # a child issue
      task.md
      plan.md
      progress.md          # doc-update, code-update, fix
      summary.md           # doc-update, code-update, fix
      report.md            # investigate
```

Group naming is the same as a standalone issue: the directory is `<YYYYMMDD>-<group-name>`, where `<group-name>` is a short kebab-case name, under the `issues/<YYYYMM>/` year-month group.

Child issues are regular issues that live under `children/`, each in its own directory `<index>-<issue-name>`. `<index>` is a one-based, zero-padded three-digit sequence (`001`, `002`, `003`, …) that fixes the order among siblings. A child issue drops the date prefix because its parent group already carries the date and ordering context. Example:

```text
issues/202606/20260624-auth-redesign/
  index.md
  progress.md
  children/
    001-update-architecture/
    002-update-api-contract/
    003-update-backend/
```

## 3. Maximum nesting depth

Only two levels are allowed:

```text
Issue
```

or

```text
Issue Group
  └── Child Issues
```

Child issues must not create grandchildren. A child issue is a leaf — it has the same artifact set and the same rules as a standalone issue, and no `children/` subdirectory of its own. If a child issue grows too large to stay one issue, do not nest: create a new standalone Issue or Issue Group beside the current group and reference it from `index.md` or the child's `summary.md`.

## 4. index.md

`index.md` is the static description of the group. It describes:

- The **purpose** of the group — the joint outcome, in one paragraph.
- The **execution order** — the child issues, by directory name, in the order they should be worked.
- The **dependencies** between child issues, as edges (`002 -> 001`, `005 -> 003,004`). Omit this section if the children are independent.

`index.md` should remain relatively stable after creation. It is the plan; `progress.md` is what actually happened. If the execution order or dependencies change materially, update `index.md` — but do not let it drift into a second execution log.

For section structure, see `templates/issue-group/index.md`.

## 5. progress.md

`progress.md` is the dynamic execution state of the group. It reuses the same machine-checked status line as an issue `progress.md`, plus an Execution Log table.

### 5.1 Status line — machine-checked

The first content after the `# Issue Group Progress` title must be:

```markdown
## Status: pending
```

or

```markdown
## Status: in-progress
```

or

```markdown
## Status: done
```

`pending` is group-only: a group can be created (with `index.md` and child directories) before any work is requested. Standalone issues never use `pending` — an issue `progress.md` is created only when execution begins, so it starts at `in-progress`. The terminal value is `done`, the same as for issues, so existing automation that greps for `## Status: done` matches both.

Group status lifecycle:

- `pending` — `index.md` and child directories exist, but no child verb has been requested.
- `in-progress` — at least one row in the Execution Log has moved to `in-progress` or `done`.
- `done` — every row in the Execution Log is `done`.

### 5.2 Execution Log

The Execution Log is an ordered queue of requested work. Each row is a tuple `(Subissue, Verb, Status)`:

| Subissue | Verb | Status |
| --- | --- | --- |

See `templates/issue-group/progress.md` for the section structure. The rules the template cannot enforce are below.

## 6. Execution Log fields

### 6.1 Verb

```text
create
plan
execute
```

These are a subset of the issue verbs from `SKILL.md`. `revise` is intentionally excluded from the queue — a child issue may be revised on demand during its own execution, but that revision is not a separately queued group step. Queue only `create`, `plan`, and `execute`.

### 6.2 Status

```text
pending
in-progress
done
```

A row starts at `pending`, moves to `in-progress` when the work begins, and `done` when it lands.

## 7. Important Rule — only requested work

The Execution Log contains only work that has been explicitly requested. It must not predict future work.

Example. The user says:

```text
Create issue group.
```

Then these rows are added:

```text
001 create
002 create
003 create
```

But these must NOT be added yet:

```text
001 plan
001 execute
```

Planning and executing the children are separate requests. Adding them preemptively would make the queue a guess at intent rather than a record of what was asked for.

## 8. Ordering Rule — child outer, verb inner

When multiple verbs are requested at once, rows are generated with the child as the outer loop and the verbs as the inner loop. Each child is treated as a complete work unit before the next begins.

Example. The user says:

```text
Plan and execute the issue group.
```

The Execution Log becomes:

```text
001 plan
001 execute

002 plan
002 execute

003 plan
003 execute
```

Not:

```text
001 plan
002 plan
003 plan

001 execute
002 execute
003 execute
```

The first form keeps each child's plan and execute together, so a child is planned and executed as one coherent unit before the next child is touched. The second form front-loads all planning and defers all execution, which breaks the per-child cohesion and the dependency order in `index.md`.

This rule governs the order in which rows are written and worked, not when clarification happens. Clarification stays front-loaded: when more than one child is being planned and any child's plan is ambiguous, resolve all the ambiguous plans up front (001's and 002's together) before writing any rows or executing, then generate and work the queue in the order above. Planning does not always need clarification — only clarify what is genuinely ambiguous. This is the group special case of the general Clarification stance in `SKILL.md`.

## 9. Recovery Rule — filesystem is the source of truth

Agents must not rely on memory. After long execution, context compression, or a session restart, reconstruct state from:

```text
index.md
progress.md
child issue artifacts
```

Read `index.md` for the intended order and dependencies. Read `progress.md` for which `(child, verb)` rows are pending, in-progress, or done. Read each in-flight child's own `progress.md` for step-level state. The filesystem is the source of truth, not conversation memory.

This rule applies to standalone issues too: resume any issue by reading its `task.md`, `plan.md`, and `progress.md` cold, never by recalling what was "about to" happen.

## 10. Decomposition Rule — decompose before grouping

Issue Groups are execution containers, not decomposition artifacts. The breakdown into children must already be available before the group is created, so the user can review the decomposition before execution begins. Decomposition is valid if it comes from any one of:

- The user explicitly specifies the child issues.
- The decomposition is trivial and obvious.
- A project document defines it.
- Conversation history defines it.
- A previously completed `investigate` / research issue defines it.

Typical flow:

```text
Discussion / Investigation
        ↓
Review decomposition
        ↓
Create Issue Group
        ↓
Create child issues
        ↓
Plan / Execute child issues
```

If the decomposition is not yet available, do not create the group to force one. Run the discussion or an `investigate` issue first, then create the group against an agreed breakdown.

## 11. Child issues are full issues

Every child issue is a full issue. It carries one of the four types (`doc-update`, `code-update`, `fix`, `investigate`) and that type's artifact set, using the same templates under `templates/<type>/`. There are no separate child-issue templates — copy the relevant type's template into the child directory.

A child issue follows all the same rules as a standalone issue: `task.md` is semantic, `plan.md` is enumerative, `progress.md` logs execution against the plan, `summary.md` (or `report.md`) is the completion artifact. The only differences are its location (under `children/`), its name (`<index>-<issue-name>`), and that its execution is also reflected as a row in the parent group's Execution Log.
