# Backend domain patterns

> Last verified: Django 5.x, FastAPI 0.1xx. Load when the task is payments, ledgers, callbacks, dashboards, metering, or debug routes. Do not create a domain because this file exists.

Syntax (locks, Depends, `check --deploy`) lives in the stack `domain-patterns.md`. This file is the contract.

## A. Payments / billing

- Provider SDK only from that domain's `services/` — never from a view or router.
- Verify the provider signature with the signing secret on every webhook. No user auth is not "no security."
- Idempotent on provider event ID. Providers resend.
- Never trust a client-supplied amount. Compute from stored price. Never store raw card data — hosted checkout/Elements.
- Log every payment state transition.

## B. Ledger (credits, wallets, points)

- Append-only entries. Balance is derived, or a cache reconciled against the ledger — not a lone balance column.
- Balance writes run in a transaction with a row lock (stack file names the API).
- A refund is a new entry, never an edit of an old one.
- Packages, tiers, prices: fixtures or a management command, not migrations.

## C. External dispatch and callbacks

- Per-dispatch token: `contract.md`. Idempotent. Max retries, then a compensating action (usually a ledger refund) — never a silent drop.

## D. Read-only dashboards

- Never modify data. Cache expensive aggregations in Redis with a TTL that matches how fresh the number must be.
- Watch N+1 in aggregation loops. Admin permission only.
- Cross-app reads: string FK (`"otherapp.Model"`) and read-only queries are allowed. Writes still go through the owning app's `services/`.

## E. Third-party metering

- API key from env only. Sync is idempotent by period. Backfill uses the same function as the scheduled task. Back off on the provider's rate limits.

## F. Dev-only / debug routes

- Conditional include when env is non-production. Convention is not a guard.
- A custom Django/FastAPI system check must **error** if those routes would load under a production-configured env. Do not rely on `check --deploy` alone — it does not know your debug URLs.
- Dev-only still uses real credentials if it touches a real service.
