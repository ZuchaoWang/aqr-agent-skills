# Interface content criteria

Criteria for interface docs — a module's code API (its public functions and classes) or a network API contract, sitting between design and code. Keep it reviewable: a flat list of what is exposed, each item with a one-line description. Finer detail is hard to review and drifts into a spec; add it only when necessary, and expect a human may not review it.

## 1. Code API (functions and classes)

- **File list** — one line per file: name and its role.
- **Public surface** — for each public class or function: the file it lives in, the signature on one line, and a one-line description of what it does.
- **Tests** — the names of the test functions that cover the public surface, and a one-line description of what it does.

Skip internal helpers, trivial accessors, and full type bodies — point at the file. If the public surface will not fit on roughly a page, the module is probably doing too much.

## 2. Network API (HTTP endpoints)

- **Endpoints** — one line per endpoint: method, path, one-line purpose.
- **Input format** — for each endpoint, the request fields (name, type, required/optional).
- **Output format** — for each endpoint, the response fields (name, type, meaning) and the error response shape.
