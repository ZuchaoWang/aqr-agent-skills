# Testing

What the project tests, how it tests, and what it does not test.

## 1. What to test

- Public-surface behavior of modules: input → expected output.
- Edge cases that the implementation explicitly handles.
- Regression cases for fixed bugs (a bug fix lands with a test that would have caught it).
- Data transformations: each transformation function gets at least one concrete input → output case.

## 2. What not to test

- Internal helpers that have no public surface.
- Framework behavior (do not re-test FastAPI's routing; test that the route is registered and returns the expected shape).
- Mock-heavy tests that assert interaction order rather than observable behavior.

## 3. Test organization

- Tests live alongside the code they test, or under a parallel `tests/` tree mirroring the source layout. Pick one rule per project and stay consistent.
- One test file per source module. Name the file after the module.
- Test names describe the behavior, not the implementation: `test_<behavior>`, not `test_<function>`.

## 4. Test-first

For code-update and fix issues, prefer writing or updating the test first when practical. The test captures the expected behavior; the implementation makes it pass. When tests cannot be written first (e.g. infrastructure changes), document the verification method explicitly in the issue's `plan.md`.

## 5. Running tests

Document the exact commands in this section:

- Run all tests: {{command}}.
- Run one test file: {{command}}.
- Run one test: {{command}}.
- Run with coverage: {{command}}.
