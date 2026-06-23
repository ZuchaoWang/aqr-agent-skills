# Backend (Python)

Include this doc only if the project has a Python backend. Otherwise delete it and remove the reference from `docs/index.md`.

## 1. Stack

- Language: Python {{version}}.
- Framework: {{FastAPI / Flask / Django / none}}.
- Pydantic version: {{v1 / v2}}.
- Database / storage: {{PostgreSQL / MongoDB / in-memory / other}}.

## 2. Rules

- Use type annotations on all public surfaces.
- Use `TypedDict` for structured data, not `dataclass` (unless the project has chosen otherwise — record the choice here).
- Docstrings on major functions; inline comments only for non-obvious why.
- Prefix module-private helpers with `_`.
- Do not create empty files. The only exception is `__init__.py`, which may be empty to mark a package.
- Secrets: read from environment variables, never hardcode.

## 3. Module layout

- Router-per-module: each top-level module ships `views.py` (handlers), `models.py` (Pydantic), `services.py` (transformations).
- Pure helpers live in `utils/`. Pydantic query mixins live in `utils/request.py` or equivalent.

## 4. Tooling

- Format: `ruff format <file>`.
- Lint: `ruff check --fix <file>`.
- Type check: `pyright <file>` (or `mypy <file>`).
- Test: `pytest <path>`.

Run on touched files only — do not reformat the whole tree as part of an unrelated change.

## 5. Common pitfalls

- {{Add project-specific pitfalls here as they are discovered.}}
