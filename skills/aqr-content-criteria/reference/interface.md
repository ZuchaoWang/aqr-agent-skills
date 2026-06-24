# Interface content criteria

Criteria for every interface doc — the external API contract or a module's public surface. An interface doc sits **between design and code**: the design above it and the code below it must both honor the contract it states. Where the doc lives in the project is a layout choice; this skill covers content only.

An interface doc carries two things: **contract criteria** (this doc) and **recorded interface decisions** — using the same decision-record format as `design.md` §2.4 (Context / Current decision / Alternatives), because interface choices (cursor vs. offset pagination, error-envelope shape, what to hide) are design decisions.

## 1. What every interface.md must contain

Regardless of scope, an interface doc states what is exposed and what is deliberately hidden, and gives every public element a one-sentence role.

1. **Summary** — one paragraph: what this interface exposes and who consumes it.
2. **Public surface** — for each public function, class, type, or endpoint: the signature on one line, and a one-sentence role on the next. The one-sentence role is the load-bearing piece — a signature without a role forces the reader to read code.
3. **What is hidden** — the internals deliberately kept out of the public surface. Stating the boundary explicitly keeps callers from depending on implementation detail.
4. **Non-obvious field notes** — any field, parameter, or return value whose semantics are non-obvious gets an inline note (units, allowed values, nullability, lifecycle). Do not point readers at the code for things that should be documented here.
5. **Key interface decisions** — decision records (same format as `design.md` §2.4) for the choices that shaped the contract: what was exposed vs. hidden, envelope shape, versioning approach, pagination style.

Constraints: reviewable without reading code. Skip internal helpers, trivial accessors, and full type bodies — point at the file instead.

## 2. External API addendum

Purpose: a reviewer reads it and understands the full external API contract without reading code. Adapt the Google API Improvement Proposals conventions for the standard concerns.

Sections:

1. **Endpoints and entry points** — one subsection per endpoint or entry point: method, path, one-line purpose.
2. **Request and response shapes** — for each endpoint: request fields (name, type, required/optional, constraints) and response fields (name, type, meaning).
3. **Error envelope** — the standard error response shape and the error codes or messages the API produces. A consistent shape (`code`, `message`, `details`) across all errors; codes are stable and documented. State retryability — which errors a caller may retry and which it may not.
4. **Versioning** — how the API is versioned (URL path, header, media type) and the compatibility contract a version implies. What changes are breaking.
5. **Pagination** — the pagination style (cursor/page-token vs. offset) and why it was chosen. Cursor is correct for large or changing datasets; offset is simpler but unstable under inserts. State the contract a caller follows to page through results.
6. **Idempotency** — which operations are idempotent and how a caller makes a non-idempotent operation safe (idempotency key, request deduplication). State the idempotency window.
7. **Naming conventions** — resource and field naming (plural resources, snake_case vs. camelCase), and any project-wide convention.
8. **Authentication and authorization** — how callers authenticate and what access controls apply.
9. **Key interface decisions** — §1 decision records for the envelope, versioning, pagination, and idempotency choices.

Constraints: target ~2–3 pages. The contract must be complete enough that two independent implementations of it interoperate.

## 3. Module public-surface addendum

Purpose: a reviewer reads it in ~5 minutes and spots missing pieces, wrong shapes, unclear naming, over-coupling.

Sections:

1. **Files** — one-line role per file in the module.
2. **Public surface** — for each public function, class, type: signature on one line, one-sentence role on the next. Skip internal helpers, trivial accessors, and full type bodies (point at the file).
3. **What is hidden** — the module-private helpers and types deliberately kept out of the surface (prefixed `_` in Python, do-not-export patterns in TypeScript).
4. **Key interface decisions** — §1 decision records for what was exposed vs. hidden.

Constraints: target ~1 page for the whole module. If the public surface will not fit on a page, the module is probably doing too much — record that as a decision or split the module.
