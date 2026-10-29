# Django domain patterns (syntax)

> Last verified: 2026-10-29 — Django 5.2 LTS. Load with `../backend/domain-patterns.md`. This file is only the Django API names.

- Ledger lock: `transaction.atomic()` + `select_for_update()`.
- Debug routes: `if settings.DEBUG:` (or env != production) include. Custom `django.core.checks` register with `deploy=True` that **errors** when those urls are wired and `DEBUG` is False — without `deploy=True`, `--deploy` will not run it. Deploy command: `python manage.py check --deploy --fail-level WARNING`. That flag also fails on existing deploy warnings (HSTS, SSL redirect); fix those settings, or silence a check on purpose with `SILENCED_SYSTEM_CHECKS`. Default `check --deploy` only warns and does not know your debug urls.
- Dashboards: string FKs and read-only querysets across apps are allowed (`../backend/domain-patterns.md` D).
