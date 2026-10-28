# Frontend tooling

> Last verified: Next.js 16, React 19. Read when changing env, security headers, styling, animation, or performance.

CI: lint + tests + `npm audit` (or `pnpm audit`) on every PR. Lockfile: `package-lock.json` / `pnpm-lock.yaml` — pin exact versions (`global.md`).

## Env

- `NEXT_PUBLIC_*` is shipped to the browser. Never put a secret behind that prefix.
- Backend base URL from env, not hardcoded.

## Security

- Never `dangerouslySetInnerHTML` with unsanitized HTML. User HTML: DOMPurify immediately before the sink; keep DOMPurify updated.
- Strict CSP: per-request nonce + `strict-dynamic`. No `unsafe-inline` / `unsafe-eval`. Add `require-trusted-types-for 'script'` where supported.
- Third-party `<script src>`: `integrity=`. Headers on every response: `frame-ancestors 'none'` (or `X-Frame-Options: DENY`), `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy`, HSTS.
- CSRF: `SameSite` first; state-changing Route Handlers also check origin. `window.postMessage`: validate `event.origin`.
- User uploads: separate origin from authenticated pages. Never log tokens, even in dev.

## Styling and motion

- One styling approach (Tailwind default for new projects). Colocate styles with the component — no giant global sheet.
- CSS first (`transition`, `@starting-style`, `scroll-timeline`) for fades, hovers, skeletons.
- `motion/react` (not `framer-motion`) for gesture, layout, `AnimatePresence`. View Transitions API for route-level shared elements.
- Animate `transform` and `opacity` only. `prefers-reduced-motion` everywhere (`useReducedMotion()` or the media query). Variants live next to the component, not in a shared `animations.ts`.

## Performance

- React Compiler for memoization; `useMemo`/`useCallback` only after a profiled problem.
- `next/image` for every image. `next/dynamic` for heavy rare features.
