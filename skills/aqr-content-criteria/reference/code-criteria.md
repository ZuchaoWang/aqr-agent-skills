# Code content criteria

Universal code quality principles that apply regardless of language, framework, or project style. These are baseline expectations — a floor, not a ceiling. Project-specific style rules (`docs/rules/code_general.md` and per-stack files) layer on top and may be stricter.

## 1. Public surface integrity

- Type-annotate all public surfaces. Untyped internal helpers are acceptable; untyped public APIs are not.
- Prefix module-private helpers with `_` (Python) or use do-not-export patterns (TypeScript) to keep the public surface honest.
- Do not expose implementation details through the public surface. If a caller needs to know about an internal type or mechanism, that is a design problem, not a documentation problem.

## 2. Naming and constants

- Use named constants instead of magic numbers and magic strings.
- Name things for what they are, not how they are implemented. `user_store` over `user_dict`; `is_active` over `flag`.
- Function names are verbs or verb phrases. Data names are nouns.

## 3. Comments

- Comments explain why, not what. If the why is non-obvious, write one line. If the why is obvious, omit the comment entirely.
- Do not write multi-line comment blocks describing what a function does — that belongs in the doc, or the code is not clear enough and should be simplified.
- Do not reference issue numbers, PR titles, or task names in comments. Those belong in commit messages and PR descriptions; they rot in code.

## 4. Secrets and configuration

- Do not hardcode secrets, passwords, or tokens. Read them from environment variables or a config file outside version control.
- Do not hardcode environment-specific paths or hosts. Parameterize them.

## 5. Error handling

- Handle errors at the boundary where you can do something meaningful. Do not swallow exceptions silently. Do not re-raise exceptions that add no information.
- Validate at system boundaries (user input, external API responses, config values). Trust internal code and framework guarantees — do not add defensive checks for states that cannot occur.

## 6. File and module hygiene

- Do not create empty files. The only exception is `__init__.py`, which may be empty to mark a Python package.
- One clear responsibility per module. If a module file is growing past ~300 lines, reconsider its scope before adding more.
- Do not introduce an abstraction for fewer than three call sites. Duplication is cheaper than a premature abstraction that does not fit.

## 7. Before committing

- Run the project's linters, formatters, and type checker on touched files. See the project's per-stack rule docs for the exact tools.
- Ensure new public functions are covered by at least one test that exercises a realistic input.
