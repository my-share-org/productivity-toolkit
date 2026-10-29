# n8n domain patterns

> Last verified: 2026-10-29 — n8n 2.45. Read only when the workflow has this shape. HMAC and webhook auth: `core.md`. Callback token: `../backend/contract.md`. Pattern C on the backend: `../backend/domain-patterns.md`.

## A. Inbound webhook (payment provider, CRM, git host)

- Full webhook security from `core.md` (Raw Body + HMAC + `crypto` allowlist). Verify → fast ack → async process. Never run business logic before verification succeeds.

## B. External dispatch + callback (leads)

- Backend sends a per-dispatch token (`../backend/contract.md`). n8n returns that same token unchanged. n8n does not invent a second auth scheme for the callback leg.
- A third party calling *into* n8n is pattern A, not this.
- Do not re-fire a callback that already succeeded in the same execution when a later node retries.
- Failure after retries → backend compensating action (refund), not a silent drop.

## C. Scheduled sync / batch

- Idempotent by period key. Re-run must not double-apply. Batch and wait (`limits.md`).

## D. Notification / alerting

- The Error Workflow in `core.md` is this pattern: one workflow whose job is a human-visible alert.
