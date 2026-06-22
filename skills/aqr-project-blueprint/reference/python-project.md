# Python project reference

Defaults for a Python service or library following the blueprint. Adjust per project; record deviations in `docs/conventions/backend-python.md`.

## 1. Versions

- Python 3.12 by default. Pin in `.python-version`. Adjust if a project needs an older or newer version.
- pyenv env name: `<project-name>-<pyversion-suffix>` (e.g. `bgp-vis-312`). Document in `CLAUDE.md` so a fresh agent knows which env to activate.

## 2. Package manager

Default to `uv` for new projects. `poetry` and `pip` are both acceptable; `conda` is acceptable for projects with non-Python binary dependencies.

- `uv` — fast, modern, single tool for venv + install + lock. Lock file: `uv.lock`.
- `poetry` — established, well-tooled. Lock file: `poetry.lock`.
- `pip` + `requirements.txt` — simplest; lock by pinning in the requirements file.
- `conda` — only when binary dependencies make `uv` / `poetry` impractical.

State the choice in `CLAUDE.md` and `docs/conventions/backend-python.md`.

## 3. Project metadata: pyproject.toml

Single source of truth for build config, tool config, and project metadata. Do not scatter tool config across `pytest.ini`, `.ruff.toml`, `pyrightconfig.json`, `setup.cfg` — those are deprecated patterns.

A minimal `pyproject.toml` carries:

- `[project]` — name, version, requires-python, dependencies.
- `[project.optional-dependencies]` — dev dependencies under `dev`.
- `[tool.ruff]` — line length, target Python version, rule selections.
- `[tool.pytest.ini_options]` — test paths, markers, options.
- `[tool.pyright]` — type-checking mode, include / exclude paths.

## 4. Lint / format / type-check

- Format: `ruff format`. Replaces Black for new projects.
- Lint: `ruff check`. Replaces Flake8 + isort + many plugins.
- Type check: `pyright` (default) or `mypy`. Pick one per project; do not run both.

Run on touched files only:

```bash
ruff check --fix path/to/file.py
ruff format path/to/file.py
pyright path/to/file.py
```

Do not reformat the whole tree as part of an unrelated change — that creates noise in review and conflicts with in-flight branches.

## 5. Module layout

For services (FastAPI, Flask), recommend router-per-module:

```
backend/src/
  module_name/
    __init__.py
    views.py        # route handlers
    models.py       # Pydantic models
    services.py     # business logic and transformations
  utils/
    __init__.py
    misc.py
    request.py      # shared Pydantic query mixins
  app.py            # FastAPI factory
  config.py         # Settings, get_config()
```

For libraries, the layout follows the library's nature — but the same principle: one module per concern, `utils/` for shared helpers, no orphan files at the root.

## 6. Tests

- Framework: `pytest`.
- Layout: mirror the source tree under `tests/`. One test file per source module.
- Naming: `test_<behavior>`, not `test_<function>`.
- Run one test: `pytest path/to/test_file.py::test_name`.

## 7. Common pitfalls

- `__init__.py` files are empty package markers — do not put code in them.
- Pydantic v1 and v2 have incompatible APIs. Pin the major version explicitly and write the choice in `docs/conventions/backend-python.md`.
- FastAPI `Depends()` is the right way to share query models; do not hand-roll a request-parsing layer.
- For async I/O, prefer `httpx` over `aiohttp` for new code. Pin the choice and document why.
