# Django structure

> Last verified: Django 5.x. Read when creating an app, adding a module, or deciding where a file goes. Endpoint behavior is in `core.md`.

Two apps are expanded only to show file shape: `authentication/` (almost every backend needs one) and `payments/` (webhook-driven domain). Neither is mandatory. Build the domains you actually need, in this shape.

```
├── authentication/            # Custom user model, JWT, password flows, admin user management
│   ├── docs/
│   ├── managers/
│   │   ├── __init__.py
│   │   └── user.py                # UserManager — named after the model, not managers.py
│   ├── migrations/
│   │   ├── __init__.py
│   │   └── 0001_initial.py, 0002_..., ...
│   ├── services/
│   │   ├── __init__.py
│   │   ├── admin.py                 # admin-triggered logic — NOT root admin.py
│   │   ├── password.py
│   │   └── permissions.py
│   ├── v1/
│   │   ├── __init__.py
│   │   ├── serializers.py
│   │   ├── urls.py
│   │   └── views.py
│   ├── __init__.py
│   ├── admin.py                    # Django admin site (ModelAdmin)
│   ├── apps.py                       # AppConfig; connect signals in ready()
│   ├── models.py
│   ├── signals.py                     # use sparingly — see core.md
│   ├── tests.py                        # or tests/ once it outgrows one file
│   └── throttles.py                    # only if this app needs a custom scope
├── payments/                    # example webhook domain — see domain-patterns.md
│   ├── docs/
│   ├── migrations/
│   ├── services/
│   ├── v1/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   └── tests.py                        # add signals/throttles/tasks only if this domain needs them
├── <domain>/, ...                  # otp, emails, notifications, ledger, dispatch, reporting, debug — same shape
├── core/                          # shared foundation, no domain logic
│   ├── health/
│   ├── services/
│   ├── middleware/                    # admin_throttle
│   ├── cache/                          # redis_client
│   ├── utils/                           # response, exception_handler, pagination, permissions, ...
│   ├── templates/
│   ├── models.py                          # BaseModel
│   └── docs/
├── <project_name>/
│   ├── configs/                      # celery_schedules.py
│   ├── settings.py
│   ├── env_config.py
│   ├── urls.py
│   ├── urls_v1.py
│   └── celery.py
├── bruno-collection/                # if used — see tooling.md
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
└── uv.lock   # or hashed requirements from pip-compile --generate-hashes; not a floating requirements.txt
```

- `migrations/`: one file per schema change, applied in order.
- Per-app `docs/`: short domain notes. Not `developer_guide/` (shared conventions) and not root `docs/` (full project docs). See `tooling.md`.
- `tests.py` until it exceeds **150 lines** or covers more than one concern; then a `tests/` package.
