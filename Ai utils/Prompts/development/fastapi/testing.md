# FastAPI testing

> Last verified: 2026-10-29 — FastAPI 0.162. `core.md` is already loaded. Webhooks/ledgers: also `domain-patterns.md`.

- Client: `TestClient` or `httpx.AsyncClient`. Override deps with `app.dependency_overrides` — do not patch `Depends` by rewriting routes.
- Runner: **pytest** + factories (factory_boy or equivalent). Convert `tests.py` to `tests/` when the file exceeds **150 lines** or covers more than one concern.
- Assert the shared error shape (`../backend/contract.md`) on every new endpoint.
- Authorization: another user's resource → **403**. Rate limit: dedicated scope exceeded → **429** + `Retry-After`.
- Ledger/callback: concurrent requests, one effect. Mock Stripe with `respx` / `httpx.MockTransport` — never the live API.
