# Documentation style

`docs/` is a spec-driven documentation set. Code is written to satisfy documented specifications, not the other way around.

## 1. Structure

- Place a summary paragraph after the title and before subsections in technical docs.
- Use numbered headings (`## 1. Overview`, `### 1.1 Motivation`) so cross-references are stable.
- Group related docs under a directory with its own `overview.md` or `index.md` that serves as the section map.

## 2. Markdown style

- Do not use `---` horizontal separators. Restructure instead.
- Use `-` for list items, not `*`.
- Use fenced code blocks with a language tag for any code or tree output.
- Cross-reference other docs with relative paths so links resolve from any rendering surface.

## 3. Lifecycle and freshness

Docs do not carry a `Status:` header. Lifecycle signals that earn their place:

- **Archived docs** live under `docs/archived/<original-path>/`. Moving the file is a stronger signal than a header — git tracks it, links break loudly, and stale files are grep-excludable.
- **Unfinished sections** are simply left with `<!-- TODO: ... -->` placeholders or empty. Their presence as TODOs is the signal.
- **Review and approval** are tracked in git history and PR review, not in the doc body. `git log <file>` shows when and by whom.
- **Last-updated** is `git log -1 --format=%cd <file>`. Do not write a date into the doc — it rots.

## 4. Markdownlint

The repo ships `.markdownlint.json` that disables rules incompatible with this style (no hard line-length limit, no tight list spacing enforcement, etc.). Do not re-enable those rules project-wide without updating this doc.
