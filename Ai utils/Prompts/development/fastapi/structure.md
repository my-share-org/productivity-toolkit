# FastAPI structure

> Last verified: 2026-10-29 — FastAPI 0.162. Read when creating a domain or deciding where a file goes. Endpoint behavior is in `core.md`.

`authentication/` and `payments/` show shape only. Neither is mandatory.

```
├── authentication/
│   ├── docs/
│   ├── services/
│   │   ├── __init__.py
│   │   ├── admin.py
│   │   ├── password.py
│   │   └── permissions.py
│   ├── v1/
│   │   ├── __init__.py
│   │   ├── schemas.py
│   │   └── routers.py
│   ├── __init__.py
│   ├── models.py
│   ├── dependencies.py
│   ├── tests.py
│   └── throttles.py                     # only if this domain needs a custom scope
├── payments/
│   ├── docs/
│   ├── services/
│   ├── v1/
│   ├── __init__.py
│   ├── models.py
│   └── tests.py
├── <domain>/, ...                  # same shape; add dependencies.py / throttles.py / tasks.py only if needed
├── alembic/
│   └── versions/
├── core/
│   ├── health/
│   ├── services/
│   ├── middleware/
│   ├── cache/
│   ├── utils/
│   ├── templates/
│   ├── models.py
│   └── docs/
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

- `tests.py` until it exceeds **150 lines** or covers more than one concern; then a `tests/` package.
- Per-domain `docs/`: short notes. Shared conventions: `developer_guide/`. Full docs: root `docs/` (`tooling.md`).
