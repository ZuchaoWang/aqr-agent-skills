Status: active

# Result guidelines

`result.md` is the artifact that future readers — the user, a reviewer, a future agent — will reach for first when they want to understand what an issue actually did. It deserves more care than `progress.md` because it is the durable summary.

For `investigate` issues, `result.md` IS the deliverable — there are no file changes elsewhere, so the quality of the issue is the quality of `result.md`.

## 1. What result.md is for

Three audiences, three uses:

- **The user reviewing the work.** Needs to decide whether to accept, request changes, or merge. Wants the bottom line first, then evidence.
- **A future agent resuming related work.** Needs to know what is settled and what is still open. Wants explicit Known Limitations and Follow-Up Issues.
- **Audit / search.** Six months later, someone greps `issues/` for "did we ever touch X?" Wants concrete file paths and grep-friendly summaries.

Write for all three. Lead with the bottom line; back it with evidence; be grep-friendly.

## 2. The Status header

`result.md` starts with `Status: active`. Not `draft` — by the time result.md exists, the work is done and the summary is stable. Not `archived` — the result is still the current truth.

The only time `result.md` carries a different status is when the work is later superseded by another issue; then this issue's `result.md` stays `active` and the superseding issue's docs reference it.

## 3. Files Changed — for doc-update, code-update, fix

`Files Changed` is **always present** for these three types, even if the list is short. (If the issue produced no file changes, it should have been an `investigate`.)

### 3.1 Structure

```markdown
## Files Changed

### Created

- `path/to/new/file` — one-line role.

### Modified

- `path/to/existing/file`
  - `<function or section>` — what changed.
  - `<function or section>` — what changed.

### Deleted

- `path/to/removed/file` — reason.
```

Omit empty subsections. Do not write "None." under each — the absence of the subsection is the signal.

### 3.2 Modifications must name locations

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

### 3.3 Be exhaustive

Every file the diff touched belongs in `Files Changed`, including dotfiles, configs, and test fixtures. Group by tree (`backend/`, `frontend/`, `docs/`, etc.) inside each subsection when the list is long.

Omitting the dotfile edits or the test fixture edits makes the result harder to audit and harder to grep months later.

## 4. Summary

One paragraph. Three sentences max in most cases:

1. What shipped.
2. How it was verified at a high level (build / test / inspection).
3. Anything the reader needs to know before reading the rest.

Reference the issue directory (`issues/YYYYMM/<id>/`) so a reader can find the original task and plan. Reference any prior issue the work built on.

## 5. Commits

Include a commit table only if the work spanned multiple commits and the mapping matters for review or audit. For a single-commit issue, omit the table and reference the SHA in Summary.

Format:

```markdown
| # | SHA | Defect / Step |
|---|-----|---------------|
| 1 | `3117408` | 1 — event-page anchors (drop `.close`/`.open`, set concrete tops) |
```

The "Defect / Step" column should match the `## Execution Steps` from `plan.md` so a reader can cross-reference. Never amend a published commit; if live verification reveals a follow-up is needed, add a new row.

## 6. Verification Notes

Map one-to-one to the acceptance criteria in `task.md`. Number them to match. For each criterion:

- State how it was checked.
- Paste the actual command if a command was run.
- State the outcome (pass / fail / accepted-with-caveat).

Bad verification note:

```markdown
1. Tests pass.
```

Good verification note:

```markdown
1. `npm test` exits 0; 31/31 tests pass (was 28/28 before this issue; the 3 new tests cover the throttle-after-unmount edge case).
```

## 7. Known Limitations

Each item should be:

- **Specific.** "Performance could be better" is not specific. "The N=1M case times out at 30s" is.
- **Checkable.** A reviewer should be able to confirm the limitation without re-running the work.
- **Accepted.** State that the limitation was accepted and why — not just that it exists.

If there are no known limitations, write "None." Do not omit the section. An empty section signals that the question was asked and answered.

## 8. Follow-Up Issues

Each item should be specific enough to become a new issue directory without further clarification. Format:

```markdown
- **<short title>** — one-line rationale. Suggested type: doc-update | code-update | fix | investigate.
```

The suggested type helps the next agent pick the right template.

If there are no follow-ups, write "None."

## 9. investigate results — different shape

`investigate` issues do not have `Files Changed` or `Verification Notes`. Their `result.md` is structured around the answer:

- `Summary` — the question, the bottom-line answer, the confidence level.
- `Findings` — the actual investigation output, structured as fits the question. This IS the deliverable.
- `Answer` — explicit answer to the question from `task.md`. If "it depends", state the conditions.
- `Confidence and Limitations` — how confident, what would change the answer, what was NOT checked.
- `Open Questions` — new questions surfaced.
- `Follow-Up Issues` — same format as other types.

The bar for an investigate result is higher than for other types, because the result IS the artifact. A vague or hedged investigate result is a failed issue, even if the investigation itself was thorough — write the answer clearly even when the answer is "we cannot know yet, because `<reason>`".

## 10. Anti-patterns

- **Mirroring progress.md.** result.md is not a copy of progress.md. progress.md is the live log; result.md is the curated summary. Drop intermediate states that did not survive to the final result.
- **No verification notes.** "Done" without evidence is not a result. If verification was informal (manual check, visual inspection), say so explicitly.
- **Hidden limitations.** A result.md that claims everything works, with verification notes that only check half the acceptance criteria, is dishonest. State what was not checked.
- **Vague follow-ups.** "Refactor later" is noise. Either make it specific or drop it.
- **Missing files.** Every touched file belongs in the file list. Omitting the dotfile edits or the test fixture edits makes the result harder to audit.
- **Modifications without locations.** "Updated `foo.py`" forces the reviewer to diff. Always name the function, class, or section.
- **Investigate results that hedge without saying why.** "Maybe X, maybe Y" is not an answer. State which is more likely and on what evidence; or state what is blocking the answer.

## 11. Quick checklist before marking done

Before flipping `progress.md` to `## Status: done` (or writing `result.md` from scratch):

For doc-update / code-update / fix:

- [ ] Summary states what shipped, not what was attempted.
- [ ] Every file the diff touched is listed under `Files Changed`.
- [ ] Every Modified entry names a location (function, class, section, or line range).
- [ ] Every acceptance criterion has a matching verification note.
- [ ] Every accepted-but-not-resolved item is in Known Limitations.
- [ ] Every discovered next-step is in Follow-Up Issues, or the section says "None."
- [ ] No reference to "TODO", "WIP", or "to be determined" remains.
- [ ] Cross-references to other issues, docs, or commits resolve (no broken paths).

For investigate:

- [ ] Summary states the bottom-line answer and confidence level.
- [ ] Findings are structured (numbered, table, timeline, or matrix — not a wall of prose).
- [ ] Each finding has evidence (file:line, command output, doc citation).
- [ ] Answer section explicitly answers the question from task.md.
- [ ] Confidence and Limitations names what was NOT checked.
- [ ] Open Questions are specific enough to become new issues.
- [ ] Follow-Up Issues name a suggested type.
