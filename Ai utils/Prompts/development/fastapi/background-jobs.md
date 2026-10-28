# FastAPI background jobs (Celery)

> Last verified: FastAPI 0.1xx. Read when adding or changing a task or beat schedule. This is the home for "email is a Celery task".

- `project_config/celery.py` is the only Celery app. Beat schedules: `project_config/configs/celery_schedules.py`.
- Task functions live in the domain that owns them.
- Every task is idempotent. External calls: explicit timeout and retry.
- Email: Celery, never inside a router. Jinja2 templates. Log every attempt.
- Do not use FastAPI `BackgroundTasks` for durable, heavy, or retryable work — that is in-process and lost on restart (`core.md`).
