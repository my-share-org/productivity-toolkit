# FastAPI core

> Last verified: FastAPI 0.1xx, Pydantic v2, SQLAlchemy 2. Always loaded with `global.md`. Also read `../backend/contract.md`. Folders in `structure.md` show shape, not a required domain list.

FastAPI does not ship throttling, permission classes, managers, or management commands. `Depends()` is manual. Rate limits need a library (e.g. `slowapi`) or a Redis `Depends()`. Do not assume Django parity.

**Read a recipe before you write if the task matches.** Several matches: read all of them. Do not open a recipe the task does not match.

- New domain, new module, or "where does this file go" → `structure.md`
- Tests → `testing.md`
- Payments, webhooks, ledgers, callbacks, dashboards, metering, debug routes → `domain-patterns.md` and `../backend/domain-patterns.md`
- Celery, beat, or email send → `background-jobs.md`
- Docker, env, CI, Swagger/OpenAPI, project docs, lockfile → `tooling.md`

## Django → FastAPI names

| Django | FastAPI |
|---|---|
| `app/v1/urls.py` + `views.py` | `app/v1/routers.py` |
| `app/v1/serializers.py` | `app/v1/schemas.py` |
| `<project_name>/urls_v1.py` | `project_config/api_v1.py` |
| `permission_classes` | `dependencies.py` (`Depends(...)`) |
| `throttles.py` | `slowapi` or Redis `Depends()` |
| `settings.py` + `env_config.py` | `settings.py` (`BaseSettings`) — reads and validates |
| `migrations/` per app | one `alembic/versions/` at project root |
| `signals.py` / `apps.py` / `managers/` | none — call side effects from the service; domain is a plain package |

## Where code lives

- One folder per domain. Logic in `services/` (split files). Routers stay thin: validated request → service → `core/utils/response`.
- `dependencies.py` next to the domain that owns auth — not one giant file. `services/admin.py` is admin-triggered logic, not a Django admin site (optional SQLAdmin in `main.py`).
- No signals. Call the side-effect from the service that triggers it.
- Writes: do not import another domain's models; call its `services/`. Exception: read-only reporting may use string FKs and query those tables; still no writes except through the owner.
- Check `core/utils/` first: `response.py`, `exception_handlers.py`, `pagination.py`, `permissions.py`, `filtering.py`, validators, generators, decorators, query, image.
- One Redis client: `core/cache/redis_client`. Health in `core/health/`, aggregated in `core/services/`.

## API

- Each domain owns `v1/` (`routers.py`, `schemas.py`). `models.py` and `services/` stay at domain root.
- `project_config/api_v1.py` only includes routers. `main.py` only: health + `api_v1.py`.
- Breaking change: add `v2/` for that domain only. Do not remove `v1/` while a client uses it.
- One Pydantic schema per use case — never one class for input and output. Field validators for one field, model validators for several. `ConfigDict(from_attributes=True)` on response schemas. No secrets or raw webhook payloads in responses.
- Auth via `Depends(get_current_user)` / `Depends(require_admin)`, never a manual check in the endpoint. Group with `APIRouter` + `tags`. **Every route sets `response_model`.** OpenAPI metadata: `tooling.md`.
- Resource existence, ownership, permissions: dependencies or service helpers — not Pydantic validators (those stay pure). Default `Depends` cache is per-request; `use_cache=False` only when you need a fresh run.

## Models and queries

- Every model inherits `core.models.BaseModel` (soft delete). Default queries exclude soft-deleted rows. Models = tables; logic in `services/`. Index FKs and hot columns.
- Schema change: Alembic in `alembic/versions/`. Read the generated file before applying.
- N+1: `joinedload` / `selectinload`. Lists: `core/utils/pagination.py`. Multi-table writes: a transaction. Concurrent read-then-write: `with_for_update()`.
- Create engines/pools in lifespan, expose via `app.state` + DI, release the request session in `yield`/`finally`. Tune `pool_size` / `max_overflow`.

## Throttles and auth

- App-level default (slowapi or shared Redis `Depends()`) on every router. Domain `throttles.py` only for a dedicated-scope endpoint from `../backend/contract.md`.
- Auth domain owns `User`, JWT, passwords, admin users. Minutes: `../backend/contract.md`. OTP: per-purpose limits. Email send: `background-jobs.md`. Notifications: call the notification service.
- Logs: `../backend/contract.md` (QueueHandler, request-context middleware). FastAPI middleware attaches request id, user, path.

## Async (breaking this freezes the server)

- `async def`: never call blocking sync DB, `requests`, or heavy CPU. Use `async def` only for real async (`AsyncSession`, `httpx.AsyncClient`, `aiofiles`).
- Sync driver or sync logic: plain `def` (threadpool) or `run_in_threadpool`.
- CPU-heavy work: process pool or Celery — not the request handler. Always `await` async calls (missing `await` is a silent no-op).

## FastAPI traps

- Dependency that opens a resource: `yield` + `finally` close. Returning a session leaks it.
- Do not pass a request-scoped DB session into `BackgroundTasks` (`DetachedInstanceError`). `BackgroundTasks` is in-process and dies on restart — Celery for durable work. Catch and log errors inside the task; the HTTP response is already gone.
- No global mutable request state. Do not read `request.body()` in middleware unless you restore the stream. Prefer pure ASGI middleware over `BaseHTTPMiddleware`.
- Create `httpx.AsyncClient` / engines at startup, not per request. Use lifespan, not deprecated `@app.on_event`. Do not `await file.read()` on large uploads — spool or chunk.
- `file.filename` is client-controlled — sanitize before save (path traversal). Separate Create vs Update schemas (mass assignment). Cookie-auth WebSockets: verify `Origin` (CSWSH). Avoid catastrophic regex in `Field(pattern=...)`. Compare secrets with `hmac.compare_digest`, never `==`.

## Security

- Production `/docs`: `../backend/contract.md`. Secrets via `project_config/settings.py` at startup. Every state-changing route: dependency-enforced auth.
- Webhooks, callbacks, debug routes: `domain-patterns.md`.

## Before you write

- Logic in `services/`? `response_model` set? Cross-domain write without a service?
- Ownership / 422 / 429 / Redis throttle — `../backend/contract.md`. Webhook → `domain-patterns.md`.
- Blocking call in `async def` → forbidden. Retries → `domain-patterns.md` or `background-jobs.md`.
- If it does not map, say so.

## Load again

Same list as the top. If this change matches a line, read that file before writing. If several match, read all. If none match, stop here.
