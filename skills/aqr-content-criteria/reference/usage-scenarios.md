# Usage scenarios content criteria

Criteria for `project/usage_scenarios.md`. Layout lives in `aqr-project-blueprint`; markdown format lives in the project's `docs/rules/doc_markdown.md`.

## 1. Purpose

A reviewer reads it and understands the concrete user-facing situations the project must handle, without reading any code. Scenarios are the bridge between the mission (what the project is for) and the design (how it is built): they pin down observable behavior the system must produce.

## 2. Sections

1. **Overview** — one paragraph: the user population and the range of situations this doc covers.
2. **Scenarios** — one subsection per scenario. Each scenario has:
   - a short title,
   - a one-paragraph description of the situation,
   - what the user does,
   - what the system must do in response — described as observable behavior, not implementation.

Frame each scenario around a user and a goal. A scenario states the situation and the required response; it does not prescribe how the system achieves it.

## 3. Constraints

- Scenarios must be concrete, not abstract feature lists. "A researcher uploads a 500 MB CSV and expects row-level validation errors within 30 seconds" is a scenario; "support large files" is not.
- Describe observable behavior, not implementation. "The system returns a list of invalid rows within 30 seconds" — not "the system spawns a worker that streams the file."
- One scenario per situation. If two scenarios share a setup but diverge in response, keep them separate so each is testable on its own.
- Target ~2–3 pages for a typical project.

## 4. Anti-patterns

- Feature lists disguised as scenarios. "Support authentication, authorization, and audit logging" describes capabilities, not situations.
- Implementation leaking in. A scenario that names a queue, a table, or a function has crossed into design.
- Vague actors. "The user" where a specific role (researcher, admin, operator) changes the required response.
- Untestable responses. "The system works well" is not a response a test can verify; state the observable outcome.
