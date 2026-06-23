# Backend (Python) reference

Defaults for a Python backend service. The blueprint recommends FastAPI for new HTTP services; the rules below cover the common case. Adjust per project; record deviations in `docs/rules/backend-python.md`.

## 1. Stack defaults

- Language: Python 3.12 (or pinned project version).
- Framework: FastAPI for HTTP services. Flask is acceptable for smaller services. Django is acceptable for full-stack apps with admin / ORM needs.
- Pydantic: v2 for new projects. v1 is acceptable for projects with existing v1 dependencies; pin explicitly and document the choice.
- Storage: pick per project. Common choices: PostgreSQL (via SQLAlchemy or asyncpg), MongoDB (via Motor or pymongo), in-memory (for prototypes and small datasets), SQLite (for single-node services).
- Task queue: Celery or RQ for background jobs; FastAPI `BackgroundTasks` for fire-and-forget within the request lifecycle.

## 2. Module layout

Router-per-module. Each top-level domain ships:

```
backend/src/
  <domain>/
    __init__.py
    views.py         # route handlers, thin
    models.py        # Pydantic request/response models
    services.py      # business logic and data transformations
  utils/
    __init__.py
    misc.py          # pure helpers
    request.py       # shared Pydantic query mixins (PageQuery, TimeRangeQuery, ...)
  app.py             # FastAPI factory and startup hooks
  config.py          # Settings, get_config(), Config singleton
  logs.py            # log configuration
  decorators.py      # cross-cutting decorators (time_cost, etc.)
  exceptions.py      # domain exception types (may be empty placeholder)
```

`views.py` handlers stay thin: parse request via Pydantic, call `services.py`, return the response. Business logic and data transformations live in `services.py` where they can be unit-tested without going through HTTP.

## 3. Configuration

- Pydantic BaseSettings for typed config loaded from environment variables.
- A module-level `Config` singleton via `get_config()`. Do not instantiate `Settings()` ad hoc; always go through the accessor.
- `DevSettings` subclass for local development defaults.

## 4. Startup

- `config_app()` builds the FastAPI app, configures routers, registers middleware.
- A startup hook loads datasets or warms caches. For the in-memory `DataStore` pattern, this is where the loaders run.
- A `/health/ping` endpoint for liveness checks. Drop `/health/mem` and other cache-introspection endpoints when there is no cache backend.

## 5. Request / response envelope

Standardize on one envelope and apply it via middleware or a response model:

```json
{ "status": "ok", "data": ..., "total": <int, optional> }
```

Error responses:

```json
{ "status": "error", "message": "..." }
```

Pick the literal value (`"ok"` / `"error"` vs `"success"` / `"bad"`) once and use it everywhere. Document the choice in `docs/architecture/overview.md` or a dedicated API doc.

## 6. Tests

- Framework: `pytest`.
- Layout: `backend/tests/` mirroring `backend/src/`.
- One test file per source module. Test names describe behavior.
- For handler tests, use FastAPI's `TestClient`. For service tests, instantiate the service directly with an in-memory store.
- For pure helpers in `utils/`, write transformation tests with concrete input → expected output cases.

## 7. Common pitfalls

- Putting business logic in `views.py`. Move it to `services.py`; handlers stay thin.
- Reading the database / store from inside Pydantic validators. Validators validate shape; data access happens in services.
- Hardcoding configuration values. Read from `Settings` via `get_config()`.
- Catching and silently swallowing exceptions. Use domain exception types in `exceptions.py`; let FastAPI's exception handlers translate them to error responses.
- Mixing Pydantic v1 and v2 patterns in one codebase. Pick one per project.
