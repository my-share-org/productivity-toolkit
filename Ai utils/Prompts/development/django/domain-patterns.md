# Django domain patterns (syntax)

> Last verified: Django 5.x. Load with `../backend/domain-patterns.md`. This file is only the Django API names.

- Ledger lock: `transaction.atomic()` + `select_for_update()`.
- Debug routes: `if settings.DEBUG:` (or env != production) include. Custom `django.core.checks` register that **errors** when those urls are wired and `DEBUG` is False. Deploy command: `python manage.py check --deploy --fail-level WARNING`. Default `check --deploy` only warns and does not know your debug urls.
- Dashboards: string FKs and read-only querysets across apps are allowed (`../backend/domain-patterns.md` D).
