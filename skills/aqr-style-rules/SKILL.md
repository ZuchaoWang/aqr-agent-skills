---
name: aqr-style-rules
description: Opinionated, stack-specific style defaults for code, tests, notebooks, presentations, and doc formatting — concrete taste layered on top of the universal code/doc criteria. Use when writing or reviewing code, tests, notebooks, or decks in a project that follows these defaults.
disable-model-invocation: false
---

# aqr-style-rules

Opinionated style defaults — taste, not a quality floor. They layer on top of universal code and doc quality principles; anything not covered here follows those. Applied when writing or reviewing; not copied into the project.

## Reference files to look at

| Area | What it covers | Reference |
| - | - | - |
| Code and tests (Python) | `TypedDict` over `dataclass`, `os.path` over `pathlib`, relative imports, naming, plain test functions, exact-equality assertions, ruff / pyright / pytest toolchain, `.python-version` / `pyproject.toml` config, editorconfig | `reference/code-python.md` |
| Code and tests (JavaScript) | `.nvmrc` Node version pin, editorconfig, naming — minimal seed | `reference/code-javascript.md` |
| Notebooks (Jupyter) | kernel pin, required header cells, root-finding boilerplate | `reference/notebooks.md` |
| Presentations (PowerPoint) | 16:10 layout, bullet symbols and margins, render-and-inspect | `reference/presentation.md` |
| Documentation (markdown) | summary paragraph, numbered headings, no separators, key: value lists, `.markdownlint.json` lint config | `reference/documentation.md` |
