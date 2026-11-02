# FastAPI structure

> Last verified: 2026-10-29 — FastAPI 0.162. Read when creating a domain or deciding where a file goes. Endpoint behavior is in `core.md`.

`apps/authentication/` and `apps/payments/` show shape only. Neither is mandatory. Every domain package lives under `apps/`, not the repository root. `alembic/` and `project_config/` stay at the root.

```
├── apps/                          # every domain package lives here, not at the repository root
│   ├── __init__.py
│   ├── authentication/
│   │   ├── docs/
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   ├── admin.py
│   │   │   ├── password.py
│   │   │   └── permissions.py
│   │   ├── v1/
│   │   │   ├── __init__.py
│   │   │   ├── schemas.py
│   │   │   └── routers.py
│   │   ├── __init__.py
│   │   ├── models.py
│   │   ├── dependencies.py
│   │   ├── tests.py
│   │   └── throttles.py                     # only if this domain needs a custom scope
│   ├── payments/
│   │   ├── docs/
│   │   ├── services/
│   │   ├── v1/
│   │   ├── __init__.py
│   │   ├── models.py
│   │   └── tests.py
│   ├── <domain>/, ...                  # same shape; add dependencies.py / throttles.py / tasks.py only if needed
│   └── core/
│       ├── health/
│       ├── services/
│       ├── middleware/
│       ├── cache/
│       ├── utils/
│       ├── templates/
│       ├── models.py
│       └── docs/
├── alembic/                         # one migration history for the whole project — not per domain
│   └── versions/
├── project_config/
│   ├── configs/
│   ├── settings.py
│   ├── main.py
│   ├── api_v1.py
│   └── celery.py
├── developer_guide/
├── docs/
├── developers_files/
├── project_script/
├── docker/
├── .github/workflows/
├── local-db.docker-compose.yml
├── local-api.docker-compose.yml
├── production-db.docker-compose.yml
├── production-api.docker-compose.yml
├── pyproject.toml
└── uv.lock                           # or hashed pip-compile output; not a floating requirements.txt
```

- Domain packages live under `apps/`. Import them as plain packages (`apps.authentication`, `apps.core`). There is no Django app label — the Python path is the only name.
- `alembic/` stays at the repository root. One `versions/` tree for the project, not a `migrations/` folder inside each domain.
- `project_config/` stays at the repository root. It wires routers in `main.py` and `api_v1.py`; it is not a domain.
- `tests.py` until it exceeds **150 lines** or covers more than one concern; then a `tests/` package.
- Per-domain `docs/`: short notes. Shared conventions: `developer_guide/`. Full docs: root `docs/` (`tooling.md`).
