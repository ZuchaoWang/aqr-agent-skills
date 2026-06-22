Status: active

# Documentation style

`docs/` is a spec-driven documentation set. Code is written to satisfy documented specifications, not the other way around.

## 1. Document status

Every document starts with a `Status:` header on the first line. The header is the first non-empty line of the file; no title or YAML frontmatter precedes it.

- `Status: placeholder` — exists as a skeleton but has no substantive content yet.
- `Status: draft` — has substantive content but has not been reviewed or approved.
- `Status: active` — current and should be followed.
- `Status: archived` — historical; do not implement from it unless explicitly instructed.

New documents start as `placeholder`. Move to `draft` once they have substantive content. Move to `active` once reviewed and approved. Move to `archived` when superseded.

## 2. Structure

- Place a summary paragraph after the title and before subsections in technical docs.
- Use numbered headings (`## 1. Overview`, `### 1.1 Motivation`) so cross-references are stable.
- Group related docs under a directory with its own `overview.md` or `index.md` that serves as the section map.

## 3. Markdown style

- Do not use `---` horizontal separators. Restructure instead.
- Use `-` for list items, not `*`.
- Use fenced code blocks with a language tag for any code or tree output.
- Cross-reference other docs with relative paths so links resolve from any rendering surface.

## 4. Lifecycle

When a doc describes a decision that has been made and approved, the status is `active`. When the decision is later reversed or the doc is superseded, do not delete the doc — flip the status to `archived` and add a one-line pointer to the new doc at the top.

## 5. Markdownlint

The repo ships `.markdownlint.json` that disables rules incompatible with this style (no hard line-length limit, no tight list spacing enforcement, etc.). Do not re-enable those rules project-wide without updating this doc.
