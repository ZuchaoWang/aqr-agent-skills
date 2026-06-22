Status: active

# Result guidelines

# Result guidelines

`result.md` is the artifact that future readers — the user, a reviewer, a future agent — will reach for first when they want to understand what an issue actually did. It deserves more care than `progress.md` because it is the durable summary.

## 1. What result.md is for

Three audiences, three uses:

- **The user reviewing the work.** Needs to decide whether to accept, request changes, or merge. Wants the bottom line first, then evidence.
- **A future agent resuming related work.** Needs to know what is settled and what is still open. Wants explicit Known Limitations and Follow-Up Issues.
- **Audit / search.** Six months later, someone greps `issues/` for "did we ever touch X?" Wants concrete file paths and grep-friendly summaries.

Write for all three. Lead with the bottom line; back it with evidence; be grep-friendly.

## 2. The Status header

`result.md` starts with `Status: active`. Not `draft` — by the time result.md exists, the work is done and the summary is stable. Not `archived` — the result is still the current truth.

The only time `result.md` carries a different status is when the work is later superseded by another issue; then this issue's `result.md` stays `active` and the superseding issue's docs reference it.

## 3. Section-by-section guidance

### 3.1 Summary

One paragraph. Three sentences max in most cases:

1. What shipped.
2. How it was verified at a high level (build / test / inspection).
3. Anything the reader needs to know before reading the rest.

Reference the issue directory (`issues/YYYYMM/<id>/`) so a reader can find the original task and plan. Reference any prior issue the work built on.

### 3.2 Files Created / Updated / Deleted (doc-update, fix) or Files Changed (code-update, fix)

Be exhaustive. Every file the diff touched belongs here, including dotfiles, configs, and test fixtures. Group by tree (`backend/`, `frontend/`, `docs/`, etc.) when the list is long.

Format each entry as:

```
- `path/to/file` — one-line role.
```

The "one-line role" should describe what the file does or what changed in it — not just the filename. "Adds the `/route/infer` endpoint." beats "Updated."

### 3.3 Commits

Include a commit table only if the work spanned multiple commits and the mapping matters for review or audit. For a single-commit issue, omit the table and reference the SHA in Summary.

Format:

```
| # | SHA | Defect / Step |
|---|-----|---------------|
| 1 | `3117408` | 1 — event-page anchors (drop `.close`/`.open`, set concrete tops) |
```

The "Defect / Step" column should match the `## Execution Steps` from `plan.md` so a reader can cross-reference. Never amend a published commit; if live verification reveals a follow-up is needed, add a new row.

### 3.4 Verification Notes

Map one-to-one to the acceptance criteria in `task.md`. Number them to match. For each criterion:

- State how it was checked.
- Paste the actual command if a command was run.
- State the outcome (pass / fail / accepted-with-caveat).

Bad verification note:

```
1. Tests pass.
```

Good verification note:

```
1. `npm test` exits 0; 31/31 tests pass (was 28/28 before this issue; the 3 new tests cover the throttle-after-unmount edge case).
```

### 3.5 Known Limitations

Each item should be:

- **Specific.** "Performance could be better" is not specific. "The N=1M case times out at 30s" is.
- **Checkable.** A reviewer should be able to confirm the limitation without re-running the work.
- **Accepted.** State that the limitation was accepted and why — not just that it exists.

If there are no known limitations, write "None." Do not omit the section. An empty section signals that the question was asked and answered.

### 3.6 Follow-Up Issues

Each item should be specific enough to become a new issue directory without further clarification. Format:

```
- **<short title>** — one-line rationale. Suggested type: doc-update | code-update | fix.
```

The suggested type helps the next agent pick the right template.

If there are no follow-ups, write "None."

## 4. Anti-patterns

- **Mirroring progress.md.** result.md is not a copy of progress.md. progress.md is the live log; result.md is the curated summary. Drop intermediate states that did not survive to the final result.
- **No verification notes.** "Done" without evidence is not a result. If verification was informal (manual check, visual inspection), say so explicitly.
- **Hidden limitations.** A result.md that claims everything works, with verification notes that only check half the acceptance criteria, is dishonest. State what was not checked.
- **Vague follow-ups.** "Refactor later" is noise. Either make it specific or drop it.
- **Missing files.** Every touched file belongs in the file list. Omitting the dotfile edits or the test fixture edits makes the result harder to audit.

## 5. Quick checklist before marking done

Before flipping `progress.md` to `## Status: done` (or writing `result.md` from scratch):

- [ ] Summary states what shipped, not what was attempted.
- [ ] Every file the diff touched is listed.
- [ ] Every acceptance criterion has a matching verification note.
- [ ] Every accepted-but-not-resolved item is in Known Limitations.
- [ ] Every discovered next-step is in Follow-Up Issues, or the section says "None."
- [ ] No reference to "TODO", "WIP", or "to be determined" remains.
- [ ] Cross-references to other issues, docs, or commits resolve (no broken paths).
