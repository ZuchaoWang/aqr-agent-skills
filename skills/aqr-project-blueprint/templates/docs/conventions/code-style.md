Status: active

# Code style

Language-agnostic rules apply project-wide. Per-language rules live in `frontend-react.md` and `backend-python.md` (include only the language-specific docs that apply to this project).

## 1. Shared rules

- Use the language version pinned in `.python-version` / `.nvmrc` / equivalent.
- Type-annotate public surfaces. Untyped internal helpers are acceptable; untyped public APIs are not.
- Do not create empty files. The only exception is `__init__.py`, which may be empty to mark a package.
- Do not hardcode secrets. Read them from environment variables or a config file outside version control.
- Prefix module-private helper functions with `_` (Python) or do-not-export patterns (TypeScript) to keep the public surface honest.
- Use named constants instead of magic numbers.
- Comments explain why, not what. If the why is non-obvious, write a one-line comment; otherwise omit.

## 2. After editing

Run the project's linters and formatters on touched files before commit. The exact tools depend on the stack — see the per-language convention docs.

## 3. What goes in per-language docs

Each per-language doc covers:

- Language version and runtime.
- Type system conventions (e.g. `TypedDict` vs `dataclass`).
- Styling / formatting tool and config.
- Linting tool and config.
- Type-checking tool and config.
- Framework-specific conventions (React, FastAPI, etc.).
- Common pitfalls and the project's chosen workaround.
