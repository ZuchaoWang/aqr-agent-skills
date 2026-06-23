# Summary guidelines

`summary.md` is the artifact that future readers — the user, a reviewer, a future agent — will reach for first when they want to understand what an issue actually did. It deserves more care than `progress.md` because it is the durable summary.

This guide covers `summary.md` for `doc-update`, `code-update`, and `fix` issues. For investigate findings, see `report-guidelines.md`.

## 1. What summary.md is for

Three audiences, three uses:

- **The user reviewing the work.** Needs to decide whether to accept, request changes, or merge. Wants the bottom line first, then evidence.
- **A future agent resuming related work.** Needs to know what is settled and what is still open. Wants explicit Known Limitations and Follow-Up Issues.
- **Audit / search.** Six months later, someone greps `issues/` for "did we ever touch X?" Wants concrete file paths and grep-friendly summaries.

Write for all three. Lead with the bottom line; back it with evidence; be grep-friendly.

## 2. Files Changed

`Files Changed` is **always present** for these three types, even if the list is short. (If the issue produced no file changes, it should have been an `investigate` — see `report-guidelines.md`.)

### 2.1 Structure

See `templates/<type>/summary.md` for the Created / Modified / Deleted structure. Omit empty subsections — do not write "None." under each; the absence of the subsection is the signal.

### 2.2 Modifications must name locations

For every Modified entry, name **where in the file** the change happened. Acceptable location anchors:

- Function name: `_calculate_total`
- Class and method: `DataStore.load_asns`
- Section heading (for docs): `## 3. Per-module doc template`
- Line range: `routes.py:75-80`

The one-line role next to the location describes **what changed about it**, not just the name. Examples:

Good:

```markdown
- `backend/src/route/services.py`
  - `list_route_table` — added `page_size` clamp at 200.
  - `infer_path` — return `[]` instead of raising on empty input.
```

Bad:

```markdown
- `backend/src/route/services.py` — updated.
```

The bad version forces a reviewer to diff the file to know what to look at. The good version lets them navigate directly.

### 2.3 Be exhaustive

Every file the diff touched belongs in `Files Changed`, including dotfiles, configs, and test fixtures. Group by tree (`backend/`, `frontend/`, `docs/`, etc.) inside each subsection when the list is long.

Omitting the dotfile edits or the test fixture edits makes the summary harder to audit and harder to grep months later.

## 3. Summary section

One paragraph. Three sentences max in most cases:

1. What shipped.
2. How it was verified at a high level (build / test / inspection).
3. Anything the reader needs to know before reading the rest.

Reference the issue directory (`issues/YYYYMM/<id>/`) so a reader can find the original task and plan. Reference any prior issue the work built on.

## 4. Commits

Include a commit table only if the work spanned multiple commits and the mapping matters for review or audit. For a single-commit issue, omit the table and reference the SHA in Summary.

See `templates/<type>/summary.md` for the table format. The "Item / Step" column should match the `## Execution Steps` (or Per-Item Change Plan) from `plan.md` so a reader can cross-reference. Never amend a published commit; if live verification reveals a follow-up is needed, add a new row.

## 5. Verification Notes

Map one-to-one to the `Acceptance Checks` in `plan.md` (which in turn map to the `Acceptance Criteria` in `task.md`). Number them to match. For each entry:

- Reference the criterion from `task.md` and the check from `plan.md`.
- Paste the actual command if a command was run.
- State the outcome (pass / fail / accepted-with-caveat).

If `plan.md` includes checks beyond the `task.md` criteria (extra global checks, regression checks), verification notes must cover those too — every check in `plan.md` needs a result in `summary.md`.

Bad verification note:

```markdown
1. Tests pass.
```

Good verification note:

```markdown
1. `npm test` exits 0; 31/31 tests pass (was 28/28 before this issue; the 3 new tests cover the throttle-after-unmount edge case).
```

## 6. Known Limitations

Each item should be:

- **Specific.** "Performance could be better" is not specific. "The N=1M case times out at 30s" is.
- **Checkable.** A reviewer should be able to confirm the limitation without re-running the work.
- **Accepted.** State that the limitation was accepted and why — not just that it exists.

If there are no known limitations, write "None." Do not omit the section. An empty section signals that the question was asked and answered.

## 7. Follow-Up Issues

Each item should be specific enough to become a new issue directory without further clarification. Format:

```markdown
- **<short title>** — one-line rationale. Suggested type: doc-update | code-update | fix | investigate.
```

The suggested type helps the next agent pick the right template.

If there are no follow-ups, write "None."

## 8. Anti-patterns

- **Mirroring progress.md.** `summary.md` is not a copy of `progress.md`. `progress.md` logs which planned steps were carried out; `summary.md` describes the resulting diff and verification. Step-by-step execution narrative does not belong in `summary.md` — only what shipped and how it was verified.
- **No verification notes.** "Done" without evidence is not a summary. If verification was informal (manual check, visual inspection), say so explicitly.
- **Hidden limitations.** A `summary.md` that claims everything works, with verification notes that only check half the acceptance criteria or half the plan.md checks, is dishonest. State what was not checked.
- **Vague follow-ups.** "Refactor later" is noise. Either make it specific or drop it.
- **Missing files.** Every touched file belongs in the file list. Omitting the dotfile edits or the test fixture edits makes the summary harder to audit.
- **Modifications without locations.** "Updated `foo.py`" forces the reviewer to diff. Always name the function, class, or section.

## 9. Quick checklist before marking done

Before flipping `progress.md` to `## Status: done`:

- [ ] Summary section states what shipped, not what was attempted.
- [ ] Every file the diff touched is listed under `Files Changed`.
- [ ] Every Modified entry names a location (function, class, section, or line range).
- [ ] Every acceptance criterion and every plan.md check has a matching verification note.
- [ ] Every accepted-but-not-resolved item is in Known Limitations.
- [ ] Every discovered next-step is in Follow-Up Issues, or the section says "None."
- [ ] No reference to "TODO", "WIP", or "to be determined" remains.
- [ ] Cross-references to other issues, docs, or commits resolve (no broken paths).
