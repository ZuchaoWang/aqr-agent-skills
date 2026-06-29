---
name: aqr-code-criteria
description: Universal code quality principles that apply regardless of language, framework, or project style. Use when writing or reviewing source code.
disable-model-invocation: false
---

# aqr-code-criteria

Universal code quality principles — a floor, not a ceiling, applying regardless of language or stack. These are reminders of what good code looks like; per-stack style (formatting, linting, framework mechanics) is not covered here. Decomposing code into modules and assigning responsibility is a design concern, also not covered here.

## 1. Library-first

Prefer an existing library over hand-rolled code for cross-cutting concerns (retry, validation, state management, auth). Custom code is justified only for domain-specific logic, performance-critical paths, or security-sensitive control. Record the chosen library and rationale in the tech-stack doc; the manifest holds the version pin, not the reason.

## 2. Public surface

- The public surface is a contract: cheap to propose, expensive to retract — default to exposing less.
- Type-annotate public APIs; untyped internal helpers are fine. Keep internals out of the public surface.
- If a caller needs to know an internal mechanism, that is a design problem, not a documentation problem.

## 3. Naming

- Named constants, not magic numbers or strings. Names describe what a thing is, not how it is built.
- Avoid dumping-ground names (`utils`, `helpers`, `common`, `shared`); name by domain responsibility so a file's purpose is clear from its name.
- Keep names consistent across the design-to-code boundary unless there is a recorded reason not to.

## 4. Comments

- Comments explain why, not what. Omit when the why is obvious; one line when it is not.
- Do not reference issue numbers or PR titles in comments — they rot; they belong in commit messages.

## 5. Secrets and configuration

- Secrets are never hardcoded; read them from environment variables or out-of-tree config.
- Separate per-environment config (read at runtime) from true constants (defined in code). Mixing them makes deployments fragile.

## 6. Error handling

- Handle errors at the boundary where something meaningful can be done. Do not swallow silently or re-raise without adding information.
- Validate at system boundaries; trust internal code and framework guarantees — do not defend against states that cannot occur.
- Errors are part of the contract: a function states how it fails, and surprising failure modes are bugs. Prefer loud, early failures over silent defaults.

## 7. File and module hygiene

- Do not create empty files (`__init__.py` excepted).
- Past ~300 lines, reconsider a module's scope before adding more.
- Co-locate what changes together; prefer early returns and avoid nesting beyond ~3 levels.

## 8. Testing

- Cover every pure function, and at least one realistic (non-happy-path) test for each public function.
- Test behavior, not implementation; mock slow, nondeterministic, or stateful dependencies and use real implementations for pure logic.
- Boundary tests for system boundaries: invalid input, missing fields, empty collections, off-by-one, large inputs.
- Avoid conceptually duplicated tests — combine minor input variations; split only when the logic path differs. Keep existing tests intact when modifying.

## 9. Before committing

- Run linters, formatters, and the type checker on touched files only — do not reformat the whole tree in an unrelated change.
- A new public function is covered by at least one realistic test.
- If the change affects a public surface or documented contract, update the matching interface or design doc in the same change.
