# Task guidelines

`task.md` defines what an issue is for. It is written first, at issue creation, and is the source of truth for **scope**. Every other artifact references it: `plan.md` plans how to satisfy it, `progress.md` logs execution against it, `summary.md` checks acceptance criteria back to it.

For the task-vs-plan distinction (semantic vs enumerative), see `SKILL.md`. For section structure, see `templates/<type>/task.md`. This doc covers how to drive `task.md` to a state where a reader can execute it cold.

## 1. The clarification dynamic

The user's initial brief is usually rough — a goal and some context. The agent's job during clarification is to drive every section to a state where it can be stated unambiguously: restate the goal in the agent's own words for the user to confirm, ask targeted questions about context and scope, draft sections the user can revise, and surface open questions rather than resolving them by assumption.

The bar is "a reader who never met the user can pick up this `task.md` and execute correctly." If any section still needs conversation to reach that bar, the section is not done.

## 2. Faithfully record user hints

`task.md` should faithfully record all useful details and hints the user mentions. If the user points to specific files, functions, or line numbers, capture them verbatim as scope hints or context (e.g. "the bug is in `foo.py:_calculate_total`", "update the schema table in `docs/data.md`"). What does NOT belong is the agent deriving a complete file list — that enumeration is `plan.md`'s job.

## 3. What does NOT belong in task.md

- A complete file-by-file bullet list of every file that will change. The user's mentioned specifics are fine as context; what does not belong is the agent's derived full-file-list.
- Implementation steps. Those belong in `plan.md`'s `Execution Steps`.
- Test names. Those belong in `plan.md`'s `Planned Test Changes`.
- Commit discipline. That is execution-time and lives in `progress.md` / `summary.md`.

## 4. Acceptance criteria

Acceptance criteria are typically drafted by the agent based on the goal and context, then confirmed or revised by the user. Users rarely specify criteria unprompted — the agent sees the structure of the work and is better placed to enumerate checkable outcomes. Draft them, show them to the user, and do not proceed to execution until the user accepts them.

Acceptance criteria are the most important part of `task.md`. They are what `summary.md` checks against. Good criteria:

- Checkable from the diff or from a single command.
- Numbered so verification notes can refer to them by number.
- Specific about file paths, command output, or observable behavior.

Bad criteria:

- "The code is cleaner."
- "Tests pass." (too broad — be specific about which tests)
- "Docs are good."

## 5. When task.md changes

`task.md` is not updated during execution. If scope changes mid-flight:

- **Small, in-spirit change.** Add an addendum at the bottom of `task.md` noting the change. Do not silently edit the original scope.
- **Type-changing scope change.** Open a new issue of the correct type. Do not relabel this one.
- **Out-of-scope discovery.** Note it under `Follow-Up Issues` in `summary.md` (or `report.md` for investigate); do not expand `task.md` scope.

## 6. Anti-patterns

- **Enumerating every file that will change.** Belongs in `plan.md`. `task.md` is semantic.
- **Vague acceptance criteria.** "Tests pass" is not checkable. Name the test suite, the command, the expected outcome.
- **Resolving open questions by assumption.** Open Questions surfaces ambiguity; it does not bury it.
- **Rewriting scope mid-flight.** `task.md` is not edited during execution. Use an addendum or open a new issue.
- **Abstracting away user-mentioned specifics.** If the user pointed to a file or function, record it verbatim.

## 7. Quick checklist before planning

- [ ] Goal restated in the agent's own words and confirmed by the user.
- [ ] Context section explains why now.
- [ ] Scope is prose, not a file list.
- [ ] All user-mentioned specifics captured verbatim.
- [ ] Acceptance criteria are numbered, specific, and checkable.
- [ ] Open Questions is "None." or every item is genuinely unresolved.
