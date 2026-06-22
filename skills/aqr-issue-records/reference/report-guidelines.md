# Report guidelines

`report.md` is the deliverable for `investigate` issues. There are no file changes elsewhere — the investigation produces no artifacts under `docs/` or in source code — so the quality of the issue equals the quality of the report.

This guide covers `report.md` for `investigate` issues. For the operational summary written at completion of `doc-update`, `code-update`, or `fix` issues, see `summary-guidelines.md`.

## 1. What report.md is for

Three audiences, three uses:

- **The user making a decision.** Wants the bottom-line answer first, then the evidence chain that supports it, then the confidence level. Has limited time and needs the answer in the first paragraph.
- **A future agent acting on the findings.** Needs to know what was checked, what was not checked, and what would change the answer. Wants explicit Confidence and Limitations and Open Questions.
- **Audit / search.** Six months later, someone asks "did we ever investigate X?" Wants a self-contained document that reads as a finished piece, not a fragment of a larger issue.

Write for all three. The report should stand on its own — a reader should not need to consult `task.md` or `plan.md` to understand the answer.

## 2. The Status header

`report.md` starts with `Status: active`. Not `draft` — by the time report.md exists, the investigation is complete and the answer is settled. Not `archived` — the answer is still the current truth.

The only time `report.md` carries a different status is when the answer is later superseded by another investigation; then this issue's `report.md` stays `active` and the superseding issue's docs reference it. If the answer gets promoted to a permanent doc under `docs/research/`, the report stays `active` and the doc references it; the report is not flipped to `archived`.

## 3. Section-by-section guidance

### 3.1 Summary

One paragraph. Three sentences max in most cases:

1. The question (restated briefly).
2. The bottom-line answer.
3. The confidence level (high / medium / low) and the one-sentence reason.

A reader who only has time for one paragraph should leave with the answer and a sense of how much to trust it. Do not bury the answer behind setup or context — the question was already stated in `task.md`.

### 3.2 Findings

The body of the report. Structure varies by question — numbered findings, comparison table, timeline, decision matrix. Pick what fits.

Each finding has:

- A statement.
- Evidence: file:line, command output, doc citation, experiment result. Concrete, not "I checked and it's fine".

Bad finding:

```
The library supports the use case.
```

Good finding:

```
The library supports the use case.

- Evidence: `node_modules/foo/README.md:42` documents the `transform()` option; `experiments/transform-test.js` runs against our sample input and produces the expected output.
```

If the investigation involved running experiments or commands, paste the relevant output. If the investigation involved reading code, cite file:line. If the investigation involved external sources, link them.

### 3.3 Answer

Explicit answer to the question from `task.md`. State it directly; do not make the reader synthesize it from the findings.

If the answer is "it depends", state the conditions:

```
The answer depends on dataset size. For N ≤ 100k, option A is faster. For N > 100k, option B is faster. Our current datasets are all under 100k, so option A is the recommendation.
```

If the answer is "we cannot know yet", say what is blocking:

```
We cannot know yet whether the production deployment will hit the same bottleneck. The investigation reproduced the bottleneck in staging but production has different I/O characteristics. Recommend a follow-up investigate issue once production profiling data is available.
```

### 3.4 Confidence and Limitations

How confident is the answer, and why. What would change it. What did the investigation NOT check that a reader might assume was checked.

Be specific. "Medium confidence" alone is not useful. "Medium confidence — the bench test is conclusive but the production deployment may behave differently because <reason>" is.

Things to call out explicitly:

- Experiments that did not run (and why).
- Sources that were not authoritative.
- Versions that may have changed since the investigation.
- Assumptions that, if wrong, would change the answer.

### 3.5 Open Questions

New questions surfaced by the investigation. Each item should be specific enough to become a new `investigate` issue if the user wants to pursue it.

Vague open questions are noise. "More research needed" is not a question; "does the bottleneck persist under load X?" is.

### 3.6 Follow-Up Issues

If findings warrant action — code change, doc change, fix, or deeper investigation — list them here as new issue candidates. Format:

```markdown
- **<short title>** — one-line rationale. Suggested type: doc-update | code-update | fix | investigate.
```

The suggested type helps the next agent pick the right template.

A common follow-up for investigate: a `doc-update` that promotes the findings to `docs/research/<topic>.md` so the answer outlives the issue record.

If there are no follow-ups, write "None."

## 4. Anti-patterns

- **Hedging without saying why.** "Maybe X, maybe Y" is not an answer. State which is more likely and on what evidence; or state what is blocking the answer.
- **Findings without evidence.** Each finding needs a citation, command output, or experimental result. "I checked and it's fine" is not evidence.
- **Burying the answer.** The answer goes in Summary (one-line form) and Answer (full form). Do not make the reader hunt for it in Findings.
- **Ignoring confidence.** An answer without a confidence level invites the reader to treat it as certain. State the confidence explicitly and back it.
- **Skipping what was NOT checked.** A reader assumes you checked the obvious things. If you did not, say so.
- **No follow-up when findings clearly warrant action.** If the investigation concluded "this is broken and needs fixing", the fix is a follow-up issue — list it.
- **Mirroring plan.md.** The report is the deliverable, not a log of the investigation process. Process notes belong in `plan.md` (if anywhere); findings belong in the report.

## 5. Quick checklist before publishing

Before considering the investigate issue done:

- [ ] Summary states the question, the answer, and the confidence level in one paragraph.
- [ ] Findings are structured (numbered, table, timeline, or matrix — not a wall of prose).
- [ ] Each finding has evidence (file:line, command output, doc citation, or experiment result).
- [ ] Answer section explicitly answers the question from `task.md`. No synthesis required from the reader.
- [ ] Confidence and Limitations names what was NOT checked and what would change the answer.
- [ ] Open Questions are specific enough to become new `investigate` issues.
- [ ] Follow-Up Issues name a suggested type, or the section says "None."
- [ ] No reference to "TODO", "WIP", or "to be determined" remains.
- [ ] The report stands on its own — a reader does not need `task.md` or `plan.md` to understand the answer.
