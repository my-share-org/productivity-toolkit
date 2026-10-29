# Frontend domain patterns

> Last verified: 2026-10-29 — Next.js 16. Read only when the project has this shape. Do not add a feature because this file exists.

## A. Payments / billing

- Hosted checkout/Elements only. The frontend never collects raw card data.
- After redirect, do not trust the URL. Re-fetch status from the backend. The webhook is the source of truth. Show "processing" until confirmed.

## B. Ledger (credits, wallets, points)

- Never optimistic-update a balance. Show loading; update only from a confirmed server response.

## C. Dashboards

- First paint: Server Component fetch (`core.md`). Client `hooks/` for filters and live refresh. Cache window should match what the backend implies. Admin dashboards use the same admin route protection as other admin pages.

## D. Notifications / activity feeds

- Poll or SSE/WebSocket via a dedicated hook. "Mark as read" may be optimistic — this is not financial data.
