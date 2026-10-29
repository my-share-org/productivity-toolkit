# Frontend testing

> Last verified: 2026-10-29 — Next.js 16, React 19, Vitest. `core.md` is already loaded.

- Unit: **Vitest**. Mapper tests are the cheapest high-value tests (`mappers/` are pure).
- Component/hook: mock `api/` with **MSW**. Do not scatter `fetch` mocks.
- E2E: **Playwright** for login, refresh-on-401, and checkout redirect.
- Token-refresh-on-401: interceptor retries once, then redirects.
- Throttle: silent endpoint 429 → last cached data, no error UI. Login/register 429 → visible message, no auto-retry.
- Lazy-load: prefetch fires at 80% of loaded items, not only at the last row.
