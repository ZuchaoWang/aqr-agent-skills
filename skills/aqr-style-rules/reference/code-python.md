# Code and test style (Python)

Opinionated Python-stack style defaults for code and tests, layered on top of the universal floor in the `aqr-code-criteria` skill — code quality in the main body, testing in §8. Apply these unless the project records a different choice. Anything not mentioned here follows `aqr-code-criteria` — type annotations on public surfaces, `_`-prefix private helpers, named constants, secrets read from the environment, no empty files, test behavior rather than implementation.

## 1. Code

### 1.1 Type and data choices

- Use `TypedDict` for structured data, not `dataclass`, unless the project has chosen otherwise — record the choice if so.
- Use `os.path` for path manipulation, not `pathlib`.
- Use relative imports within the package.

### 1.2 Toolchain

- Format: `ruff format <file>`.
- Lint: `ruff check --fix <file>`.
- Type check: `pyright <file>` (or `mypy <file>`).
- Test: `pytest <path>`.

Run on touched files only — do not reformat the whole tree as part of an unrelated change. Skip format, lint, and type checks on generated files (e.g. parser / lexer / listener files produced by antlr4 or similar tools).

### 1.3 Project files and editor config

- `.python-version` — pins the Python version (e.g. `3.12`); read by pyenv and other version managers.
- `pyproject.toml` — project metadata, build system, and tool config. The toolchain above (`ruff`, `pyright` / `mypy`, `pytest`) reads its settings from here.
- `.editorconfig` for Python (`[*.py]`): `indent_style = space`, `indent_size = 4`, `end_of_line = lf`, `charset = utf-8`, `insert_final_newline = true`, `trim_trailing_whitespace = true`. Line length is set via `ruff` (`line-length` in `pyproject.toml`), not in `.editorconfig`.

## 2. Tests

### 2.1 Test shape

- Write tests as plain functions, not classes. Avoid fixtures; pass inputs directly.

### 2.2 Assertions

- Assert exact equality (`==`), not substring containment (`in`). If testing a computed string, assert the full expected value.
- Try to test every output field. If a function returns a dict with five keys, assert all five — not just one or two.

### 2.3 Test organization

- Mirror the source path under `tests/`: the test file for `pkg/path/to/module.py` lives at `tests/path/to/test_module.py`.
