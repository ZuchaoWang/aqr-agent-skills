# Interface content criteria

Criteria for interface docs — a module's public surface or an external API contract. An interface doc sits **between design and code**: both must honor the contract it states. Where the doc lives is a layout choice; this skill covers content only.

Keep it reviewable. The core is a flat list of what is exposed, each item with a one-line description; more detail than that is hard to review and tends to drift into a spec. Add finer detail only when it is genuinely necessary, and expect that a human may not review it.

Interface choices (what to expose vs. hide, envelope shape, pagination style) are design decisions — record them with the format in `design.md` §2.4.

## 1. Module public surface

- **File list** — one line per file: name and its role.
- **Public surface** — for each public class or function: the file it lives in, the signature on one line, and a one-line description of what it does.

Skip internal helpers, trivial accessors, and full type bodies — point at the file. If the public surface will not fit on roughly a page, the module is probably doing too much.

## 2. External API

- **Endpoints** — one line per endpoint: method, path, one-line purpose.
- **Input format** — for each endpoint, the request fields (name, type, required/optional).
- **Output format** — for each endpoint, the response fields (name, type, meaning) and the error response shape.

## 3. Optional detail (add only when necessary)

The following are not part of the reviewable core. Include a section only when it carries information a caller cannot get from the input/output formats, and mark it as detail a human may not review:

- Error codes and their retryability.
- Versioning and what counts as a breaking change.
- Pagination style (cursor vs. offset) and the contract for paging.
- Idempotency rules and keys.
- Authentication and authorization.
