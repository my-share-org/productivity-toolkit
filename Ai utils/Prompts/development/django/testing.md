# Django testing

> Last verified: 2026-10-29 — Django 5.2 LTS. `core.md` is already loaded. Webhooks/ledgers: also `domain-patterns.md`.

- Runner: **pytest** + `pytest-django`. Factories: **factory_boy** (or model-bakery). No raw `Model.objects.create(...)` sprawl in tests.
- Each app: `tests.py` until that file exceeds **150 lines** or covers more than one concern; then a `tests/` package (`structure.md`).
- Assert the shared error/success shape (`../backend/contract.md`) on every new endpoint.
- Authorization: request another user's resource; expect **403**.
- Rate limit: exceed the dedicated scope; expect **429** and `Retry-After`.
- Ledger/callback: two concurrent requests; assert one ledger effect (idempotency). Mock Stripe (or the provider) with `responses` / `httpretty` — never hit the live API.
