# Documentation style (markdown)

Opinionated markdown formatting defaults for project docs.

## 1. Structure

- Place a summary paragraph after the title and before subsections in technical docs.
- Use numbered headings (`## 1. Overview`, `### 1.1 Motivation`) so cross-references stay stable.

## 2. Markdown style

- Do not use `---` horizontal separators. Restructure instead.
- Use `-` for list items, not `*`.
- For two-column tables, convert to key: value lists instead.

Enforce these in `.markdownlint.json` at the repo root, disabling any rule that conflicts with this style (for example, the horizontal-rule rule, since separators are disallowed, and any line-length rule).
