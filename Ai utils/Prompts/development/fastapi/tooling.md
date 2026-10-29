# FastAPI tooling

> Last verified: 2026-10-29 — FastAPI 0.162. Read when changing Docker, env, CI, OpenAPI, lockfile, or project docs.

## Env, Docker, CI

- `project_config/settings.py` (`BaseSettings`) reads and validates every env var. Fail startup if required vars are missing or malformed.
- Separate compose: `db` vs `api`, `local` vs `production`.
- Lockfile: `uv.lock` (or hashed pip-compile). CI: lint + tests + `pip-audit` on every PR. CI Docker close to production.
- Production ASGI: Uvicorn (or behind a process manager). Size workers from measured load, not a formula. `uvloop` only if the platform supports it and a benchmark shows a gain.

## Docs folders

- `developer_guide/` — shared conventions. `docs/` — architecture and setup. `developers_files/` — audits, if kept.
- Repeated manual work → CLI in the project toolkit (seed, backfill, setup).

## Swagger / OpenAPI

FastAPI builds `/docs` and `/redoc` from routers and schemas. Empty metadata is a miss.

- `tags` audience-first: `"Admin - Payments"`, `"User - Leads"`, `"Public - Auth"`.
- Every route: `summary`, `description` (or docstring) with permissions and non-obvious behavior, `response_model`, `responses={...}` for every non-200 with an example body. Field constraints on schemas (`max_length`, `ge`, …). Update those four in the same commit as the behavior change.
- Public: no security dependency. User: `Depends(get_current_user)` on the router. Admin: `Depends(require_admin)`. Two OpenAPI schemes (`UserAuth`, `AdminAuth`). Never a real token in an example.
- Production `/docs` protection: `../backend/contract.md`.
