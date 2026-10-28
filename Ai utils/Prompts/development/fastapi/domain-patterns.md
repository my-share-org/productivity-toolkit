# FastAPI domain patterns (syntax)

> Last verified: FastAPI 0.1xx. Load with `../backend/domain-patterns.md`. This file is only the FastAPI API names.

- Ledger lock: transaction + `with_for_update()`.
- Admin dashboards: `Depends(require_admin)`.
- Debug routes: include in `main.py` only when `settings.ENV != "production"`. Startup must **error** if those routers are mounted and env is production. Do not treat `check --deploy` as a FastAPI feature.
- Metering keys: `project_config/settings.py`.
