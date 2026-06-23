# Backend (Jupyter)

Include this doc only if the project ships Jupyter notebooks as a backend (data analysis, ML, research artifacts that other services or reports depend on). Otherwise delete it. For a Python service backend, use `backend_python.md` instead.

## 1. Stack

- Language: Python {{version}}.
- Kernel: {{kernel name and where it is defined}}.
- Notebook runtime: {{JupyterLab / VS Code / nbclassic / other}}.
- Backend state: {{where notebooks persist outputs — DB, object store, files}}.

## 2. Notebook structure

- One notebook per logical task. Do not accumulate unrelated work in a single notebook.
- Top cell: markdown header with purpose, owner, last-run date.
- Second cell: imports and configuration. No side effects.
- Cells run top-to-bottom on a fresh kernel. No "run cell 3 then cell 1 then cell 2" orderings.
- Markdown cells narrate why, not what. Code comments are for non-obvious code.

## 3. State and reproducibility

- No hidden state. Every variable used in a cell is produced by an earlier cell or an explicit load.
- Random operations set a seed; record it in the markdown header.
- External resources (data files, API endpoints, credentials) are referenced by path or env var, not inlined.
- Long-running cells cache their output (on disk or via `@functools.cache`); a re-run is cheap.

## 4. Outputs

- Strip outputs before commit (`nbstripout` or equivalent). The repo diffs source, not results.
- Generated artifacts (charts, tables) are written to a known path; the notebook shows them inline for review only.
- A notebook whose output is the deliverable ships a rendered export (HTML, PDF) next to the source.

## 5. Tests

- Pure functions used in notebooks live in importable modules under `src/` and have `pytest` tests.
- Notebooks themselves are smoke-tested: a test runs the notebook top-to-bottom on the CI Python version and fails on any cell error.

## 6. Tooling

- Format: `ruff format` on the notebook source (via `jupyter` ruff plugin or export → format → re-import).
- Lint: `ruff check` on extracted source.
- nbstripout installed as a pre-commit hook or run manually before commit.

## 7. Common pitfalls

- {{Add project-specific pitfalls here as they are discovered.}}
