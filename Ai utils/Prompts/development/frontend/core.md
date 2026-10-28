# Frontend core (React / Next.js)

> Last verified: Next.js 16, React 19. Always loaded with `global.md`. Feature folders match backend domains you actually have — `auth/` in `structure.md` is the expanded example, not a required list.

**Read a recipe before you write if the task matches.** Several matches: read all of them. Do not open a recipe the task does not match.

- New route, new feature folder, or "where does this file go" → `structure.md`
- Tests → `testing.md`
- Payments, balances, dashboards, notification feeds → `domain-patterns.md`
- Env, CSP/headers, styling, animations, performance → `tooling.md`

There is no `background-jobs.md` on this stack. HTTP error shape and 429 headers: `../backend/contract.md`.

## Server vs client (Next 16)

Default is a **Server Component**. `"use client"` is a leaf, not a page default. Push it as far down the tree as the interactivity needs.

**Stay on the server (no `"use client"`) when the work is:**

- First paint of a route: landing, marketing, docs, article, product detail, SEO pages.
- Data fetch from the backend or DB, including secrets and API keys (never send those to the browser).
- Auth gates in `(app)/layout.tsx` and `(admin)/layout.tsx`.
- Reducing JS: large markdown, syntax highlighters, date libs used only to render HTML.

**Add `"use client"` only when the work is:**

- State, `onClick` / `onChange`, `useEffect`, `useRef`.
- Browser APIs: `window`, `document`, `localStorage` (UI only — never tokens), geolocation, media.
- Client-only libraries (Motion, a map widget, a chart that must run in the browser).
- React Query hooks that refetch after load (filters, infinite scroll, live dashboards).

**Page recipe**

| Page / task | Render |
|---|---|
| Landing, docs, blog, static marketing | Server page. Client leaves: header menu, cookie banner. |
| Login, signup, password reset | Server page shell. Client leaf: the form (`react-hook-form`). |
| Auth app shell (nav, session) | Server layout checks session. Client leaf: interactive nav bits. |
| List / table / dashboard numbers | Server fetch for first paint. Client leaf: filters, charts, pagination controls. Wrap uncached async in `<Suspense>`. |
| Checkout | Server page. Card input is the provider's hosted Element (client leaf). Never a custom card form. |
| Settings forms | Server page. Client leaf: the form. |

Do not fetch in a Client Component when a Server Component can pass props. Do not call a Route Handler from a Server Component (extra hop to yourself) — call the backend (or a server module) directly. Route Handlers exist for the browser and for cookie-setting BFF.

`proxy.ts` (not `middleware.ts`) runs on **Node**, not Edge. Cookie *presence* check only; full JWT verify stays in the Route Handler / Server Component. Export `proxy`. Keep `middleware.ts` only if you must stay on Edge; it is deprecated in Next 16.

## Data pipeline

`app/` is routing only. Each `page.tsx` composes hooks and components from `features/<domain>/`. It does not fetch or format except a Server Component data call that then passes props.

| Layer | Job |
|---|---|
| `api/` | Only `fetch` for that domain. Returns raw backend DTOs. |
| `dto/` | Types of the exact backend shape. Only `api/` and `mappers/` import these. |
| `mappers/` | Pure DTO → view model. No fetch, no side effects. |
| `models/` | Types the UI uses. Components never take DTOs. |
| `hooks/` | React Query: `api/` → mapper → `{ data, isLoading, error }`. Cache, throttle, pagination live here. |
| `components/` | Presentational. Typed props. No `fetch`, no `api/` or `dto/` imports. |

If rendered data is wrong, the bug is in `mappers/` or `hooks/`, not a component that also fetches.

- Shared `components/`: no domain knowledge. Domain-aware UI lives in that feature's `components/`.
- Reusable UI takes content through props or `children`/slots, not boolean-prop explosion. Interactive shared components: semantic HTML, keyboard, ARIA.
- Forms: zod schema in the feature `schemas/`, wired with `react-hook-form` (or Server Actions). Backend still validates.
- State: `useState` for local UI. React Query for server data — do not copy it into Zustand. Zustand only for cross-cutting UI that is not server data.

## Tokens and routes

- Refresh: `httpOnly` + `Secure` + `SameSite=Lax`/`Strict` cookie, set only by a Route Handler. Access: in memory. On load, refresh via the cookie. Storage rule: `../backend/contract.md`.
- JWT verify with `jose` on Node (`proxy.ts` and Route Handlers). Client user info: `GET /api/me`.
- Login: `POST /api/auth/login` → cookie + access token in JSON. 401 → interceptor `POST /api/auth/refresh` once, retry; if refresh fails, clear and redirect. Logout: Route Handler clears the cookie and memory.
- `proxy.ts`: explicit `matcher`. Admin: role/claim check, not only "logged in."
- Await route and search params (they are Promises). Track backend API version in `dto/` types.

## Fetch, cache, 429, pagination, errors

- Next 15+: `fetch` is **not** cached by default. Opt in with `cache: 'force-cache'`, or if `cacheComponents: true`, wrap cached work in `'use cache'` (`cacheLife` / `cacheTag`) — do not use `fetchCache` / `export const fetchCache`.
- First paint: Server Component fetch. React Query for data that changes after load — set `staleTime`/`gcTime` from how often it changes.
- Classify every call in `lib/api-client.ts`: `throttleBehavior: 'silent'` (default GET/list/dashboard) or `'visible'` (login, register, password reset, OTP only).
- Silent 429: do not throw. Honor `Retry-After` exactly, keep last cached data, one retry. No "rate limited" toast. Visible 429: show the message, never auto-retry.
- Page-number: query keyed by page, `placeholderData: keepPreviousData`, dim via `isPlaceholderData` — do not unmount the list.
- Infinite: `useInfiniteQuery`. Sentinel at **80%** of loaded content (item 40 of a 50-item page), then `fetchNextPage()`. Prefetch at most one page ahead.
- `lib/api-client.ts` normalizes errors to `{ status, message, code }` except silent 429. Validation is **422** (`../backend/contract.md`). `error.tsx` for unexpected; React Query `error` for recoverable. Map `code` to friendly text — never a raw backend string.

## Before you write

- Server page with a client leaf, or did `"use client"` leak to the page?
- New UI: props/slots, or it belongs in the feature folder. No `fetch` inside a presentational component.
- HTML / CSP / tokens — `tooling.md` and `../backend/contract.md`.
- New API call: silent vs visible 429. New list: page or lazy-load at 80%.
- If it does not map, say so.

## Load again

Same list as the top. If this change matches a line, read that file before writing. If several match, read all. If none match, stop here.
