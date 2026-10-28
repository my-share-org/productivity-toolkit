# Backend HTTP contract

> Last verified: Django 5.x, FastAPI 0.1xx. Load with the stack `core.md` when the task is an HTTP API. One home for error shape, status codes, 429, tokens, logs, and the per-dispatch callback token.

Frontend maps `error.code` and `429`. n8n callbacks use the dispatch token below. Stack files do not restated these bullets.

## Errors

Shape, every failure:

```json
{ "error": { "code": "...", "message": "..." } }
```

- `code` is stable. `message` is generic. Never a stack trace or DB error in the body. Log the detail server-side.
- Status: validation **422**, missing auth **401**, permission **403**, not found **404**, conflict **409**, unhandled **500**.
- Django: enforce 422 in `core/utils/exception_handler.py` (do not leave DRF's 400 for validation). FastAPI: keep the default 422; do not remap validation to 400.
- Log every **401**, **403**, and **429**, not only 500s.

## 429

- Body still uses the error shape. Header: `Retry-After`. Never replace with HTTP 200.
- Counter store: Redis, never in-process memory (workers multiply the limit).
- Two layers: per-IP (unauthenticated) and per-user (authenticated).
- Dedicated scope — not the default rate — for: login, OTP generate/check, password reset, signup, webhooks, callbacks. Admin routes get their own scope, not the public one.

## Tokens

- Access: short-lived, **5–15 minutes**, verified on the server (signature + expiry).
- Refresh: rotated on every use. Backend may send it in an httpOnly cookie (BFF) or in the JSON body; the frontend rule owns storage. Never put access or refresh in `localStorage` / `sessionStorage`.
- Object-level ownership: "logged in" is not enough.

## Dispatch callback token

When the backend queues work and later receives a result (n8n or any worker):

- Generate a per-dispatch token at send time. Store it with the dispatch.
- The worker returns that same token unchanged. Hitting the callback URL is not proof.
- Verify the token before trusting the payload. Idempotent: a retry must not double-apply.

## Logs

- Non-blocking handler (`QueueHandler`). Separate app vs error streams.
- Request context (request id, user, path) on every line via middleware. Do not log with bare `logging` and no context.
- APIs that return raw logs are admin-only.

## Docs surface (FastAPI)

- Production: disable or protect `/docs`, `/redoc`, `/openapi.json`. One home — FastAPI `tooling.md` only describes how to fill OpenAPI metadata.
