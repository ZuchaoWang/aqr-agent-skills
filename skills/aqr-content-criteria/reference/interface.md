# Interface content criteria

Criteria for interface docs — a module's public surface or an external API contract, sitting between design and code. Keep it reviewable: a flat list of what is exposed, each item with a one-line description. Finer detail is hard to review and drifts into a spec; add it only when necessary, and expect a human may not review it. Record interface choices (what to expose, envelope shape, pagination style) with the format in `design.md` §2.4.

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
