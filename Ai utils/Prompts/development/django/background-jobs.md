# Django background jobs (Celery)

> Last verified: Django 5.x. Read when adding or changing a task or beat schedule. This is the home for "email is a Celery task".

- `<project_name>/celery.py` is the only Celery app. All beat schedules live in `<project_name>/configs/celery_schedules.py` — do not scatter them.
- Task functions live in the app that owns them.
- Every task is idempotent — safe to run twice.
- External calls: explicit timeout and retry. A hung call with no timeout occupies a worker forever.
- Email: Celery, never in the request cycle. Body in templates. Log every attempt (success or failure).
