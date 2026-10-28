# Django core

> Last verified: Django 5.x, DRF 3.15. Always loaded with `global.md`. Also read `../backend/contract.md` (error shape, 422, 429, tokens, logs). This is a rulebook, not one running app. `authentication/` and `payments/` in `structure.md` show file shape — they are not required apps. Replace `<project_name>/` with the real config package.

**Read a recipe before you write if the task matches.** Several matches: read all of them. Do not open a recipe the task does not match.

- New app, new module, signals, managers, admin files, or "where does this file go" → `structure.md`
- Tests → `testing.md`
- Payments, webhooks, ledgers, callbacks, dashboards, metering, debug routes → `domain-patterns.md` and `../backend/domain-patterns.md`
- Celery, beat, or email send → `background-jobs.md`
- Docker, env files, CI, Bruno, project docs, lockfile → `tooling.md`

## Where code lives

- One app per business domain. Logic in `services/` (split files, not one giant `services.py`). Views stay thin: request → service → `core/utils/response`.
- Root `admin.py` is Django admin site (`ModelAdmin`). `services/admin.py` is admin-triggered business logic. Never mix them.
- Writes: do not import another app's models; call that app's `services/`. Exception: read-only reporting may use string FKs (`"otherapp.Model"`) and query those tables; still no writes except through the owner.
- Check `core/utils/` before writing a shared helper: `response.py`, `exception_handler.py`, `pagination.py`, `permissions.py`, `filtering.py`, validators, generators, decorators, query, image.
- One Redis client: `core/cache/redis_client`. Health checks live in `core/health/`, aggregated in `core/services/`.
- `signals.py` only for a genuine outside reaction to `post_save` / `pre_delete`. Prefer a direct call from the service. Register signals in `AppConfig.ready()`, never at import time.
- Custom managers: `managers/<model>.py` (e.g. `managers/user.py`), not `managers.py`.

## API

- Each app owns `v1/` (`urls.py`, `views.py`, `serializers.py`). `models.py` and `services/` stay at app root and are shared across versions.
- `<project_name>/urls_v1.py` only includes app URLs. Root `urls.py` only: health, admin, `urls_v1.py`.
- Breaking change: add `v2/` for that app only. Do not delete `v1/` while a client still uses it.
- One serializer per use case — never one class for both input and output. Validate with `validate_<field>()` and object-level `validate()`. Never put password hashes, internal flags, or raw webhook payloads in a response serializer.
- DRF permission classes, not scattered `if` checks. `ViewSet` + `Router` for CRUD. `APIView` for webhooks, callbacks, and other non-CRUD actions.

## Models and queries

- Every model inherits `core.models.BaseModel` (abstract, soft delete). Default querysets exclude soft-deleted rows.
- Models hold data shape. Business logic stays in `services/`. Index FKs and hot filter/order columns. Every schema change is a migration; read the generated file before applying it.
- N+1: `select_related` (FK/O2O), `prefetch_related` (M2M/reverse FK).
- Every list endpoint uses `core/utils/pagination.py` — never an unbounded queryset.
- Multi-table writes: `transaction.atomic()`. Read-then-write on a concurrent row: `select_for_update()` inside that transaction. Ledgers and callbacks: `domain-patterns.md`.
- Production: connection pooling (`CONN_MAX_AGE` or pgbouncer).

## Throttles

- Baseline: `DEFAULT_THROTTLE_CLASSES` / `DEFAULT_THROTTLE_RATES`. An app `throttles.py` only for a dedicated-scope endpoint from `../backend/contract.md` — do not add the file by default.
- Classes: `AnonRateThrottle` (per-IP), `UserRateThrottle` / `ScopedRateThrottle` (per-user). Admin: `core/middleware/admin_throttle`.

## Auth (Django)

- Auth app owns `User`, JWT issue/verify, password flows, admin user management. Minutes and ownership: `../backend/contract.md`.
- OTP: per-purpose expiry and retry limits. Rate-limit generate and check per user/phone/email.
- Email send: `background-jobs.md`. Notifications: call the notification service; do not build the notification in the triggering app.

## Security (Django)

- Production: `DEBUG = False`, explicit `ALLOWED_HOSTS`. Secrets from env (`tooling.md`).
- Before every deploy: `python manage.py check --deploy --fail-level WARNING`. Register a custom system check that **errors** if debug routes would load when `DEBUG` is False or env is production (`domain-patterns.md` F).
- Endpoints that skip user auth still need verification (`domain-patterns.md`).

## Before you write

- Logic in `services/`? Duplicate of `core/utils/`? Cross-app write without a service?
- Ownership, input validation, env secrets — `../backend/contract.md`. Webhook/callback → `domain-patterns.md`.
- Dedicated throttle scope or Redis cache — `../backend/contract.md`.
- N+1 / pagination / `select_for_update` — this file. Retries → `domain-patterns.md` or `background-jobs.md`.
- If it does not map, say so.

## Load again

Same list as the top. If this change matches a line, read that file before writing. If several match, read all. If none match, stop here.
