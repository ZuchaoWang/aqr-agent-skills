# Code content criteria

Universal code quality principles that apply regardless of language, framework, or project style. These are baseline expectations — a floor, not a ceiling. Project-specific style rules (per-stack formatting, linting, framework mechanics) layer on top and may be stricter; those live in the project's own rule files, not here.

How to decompose code into components and modules, assign responsibility, and own state is a design concern — see `reference/design.md` §2.1. This doc covers the code-level floor that any decomposition must still meet.

## 1. Library-first approach

Search for an existing solution before writing custom code. Check the language's package registry (npm, PyPI, …), existing services or SaaS, and third-party APIs for the functionality needed. Prefer a library over a hand-rolled utility for cross-cutting concerns — use a retry library instead of writing retry logic, a validation library instead of hand-rolled checks.

Custom code is justified only when:

- It is specific business logic unique to the domain.
- It is a performance-critical path with special requirements no library meets.
- It is security-sensitive code requiring full control.
- Existing solutions fail a thorough evaluation against the requirements.

Avoid NIH ("not invented here"): do not build custom auth when an identity provider exists, custom state management when a store library exists, or custom form validation when an established library exists. Every line of custom code is a liability that needs maintenance, testing, and documentation.

Record the chosen library and the reason in the project's tech-stack doc; the manifest (`package.json`, `requirements.txt`, …) holds the version pin, not the rationale.

## 2. Public surface integrity

- Type-annotate all public surfaces. Untyped internal helpers are acceptable; untyped public APIs are not.
- Prefix module-private helpers with `_` (Python) or use do-not-export patterns (TypeScript) to keep the public surface honest.
- Do not expose implementation details through the public surface. If a caller needs to know about an internal type or mechanism, that is a design problem, not a documentation problem.
- The public surface is a contract. It is cheap to propose and expensive to retract — default to exposing less.

## 3. Naming and constants

- Use named constants instead of magic numbers and magic strings.
- Name things for what they are, not how they are implemented. `user_store` over `user_dict`; `is_active` over `flag`.
- Function names are verbs or verb phrases. Data names are nouns. Booleans answer a yes/no question (`is_`, `has_`, `can_`).
- Names are consistent across the boundary: a concept called `invoice` in the design keeps that name in code unless there is a recorded reason not to.
- Avoid generic dumping-ground names — `utils`, `helpers`, `common`, `shared` — that collect unrelated functions. Name by domain responsibility (`OrderCalculator`, `UserAuthenticator`) so a file's purpose is clear from its name.

## 4. Comments

- Comments explain why, not what. If the why is non-obvious, write one line. If the why is obvious, omit the comment entirely.
- Do not write multi-line comment blocks describing what a function does — that belongs in the doc, or the code is not clear enough and should be simplified.
- Do not reference issue numbers, PR titles, or task names in comments. Those belong in commit messages and PR descriptions; they rot in code.

## 5. Secrets and configuration

- Do not hardcode secrets, passwords, or tokens. Read them from environment variables or a config file outside version control.
- Do not hardcode environment-specific paths or hosts. Parameterize them.
- Distinguish config that changes per environment (read at runtime) from constants that do not (defined in code). Mixing the two makes deployments fragile.

## 6. Error handling

- Handle errors at the boundary where you can do something meaningful. Do not swallow exceptions silently. Do not re-raise exceptions that add no information.
- Validate at system boundaries (user input, external API responses, config values). Trust internal code and framework guarantees — do not add defensive checks for states that cannot occur.
- Errors are part of the contract. A function that can fail states how it fails (exception type, error code, null) and a caller can branch on it. Surprising failure modes are bugs.
- Prefer errors that fail loudly and early over silent defaults that mask a mistake.

## 7. File and module hygiene

- Do not create empty files. The only exception is `__init__.py`, which may be empty to mark a Python package.
- If a module file is growing past ~300 lines, reconsider its scope before adding more — the responsibility split is a design call (see `reference/design.md` §2.1).
- Co-locate what changes together. A component, its styles, and its tests that change together live together.
- Prefer early returns over nested conditionals, and avoid deep nesting (roughly more than three levels). Exact thresholds are a project-style choice.

## 8. Testing

- New public functions are covered by at least one test that exercises a realistic input, not just the happy path.
- Test behavior, not implementation. A test that breaks on a harmless refactor is testing the wrong thing.
- Boundary tests for system boundaries: invalid input, missing fields, empty collections, off-by-one, large inputs.
- What to mock vs. use real: mock external dependencies that are slow, nondeterministic, or stateful; use real implementations for pure logic. Over-mocking tests the mocks, not the code.

## 9. Before committing

- Run the project's linters, formatters, and type checker on touched files. See the project's per-stack style rules for the exact tools. Run on touched files only — do not reformat the whole tree as part of an unrelated change.
- Ensure new public functions are covered by at least one test that exercises a realistic input.
- If the change affects a public surface or a documented contract, update the corresponding interface doc or design doc in the same change.
