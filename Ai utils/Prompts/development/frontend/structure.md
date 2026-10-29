# Frontend structure

> Last verified: 2026-10-29 — Next.js 16, React 19. Read when adding a route, a feature folder, or deciding where a file goes. Pipeline, SSR/CSR, and auth are in `core.md`.

```
src/
├── app/                          # App Router — routes ONLY
│   ├── (public)/                   # unauthenticated
│   │   ├── login/page.tsx
│   │   ├── signup/page.tsx
│   │   └── page.tsx
│   ├── (app)/                       # authenticated user shell
│   │   ├── layout.tsx                  # auth guard + nav (Server Component)
│   │   └── <domain>/page.tsx
│   ├── (admin)/                      # admin shell, if the project has one
│   │   └── admin/<domain>/page.tsx
│   ├── api/                            # Route Handlers — thin BFF
│   │   └── auth/
│   │       ├── login/route.ts
│   │       ├── refresh/route.ts
│   │       ├── logout/route.ts
│   │       └── me/route.ts
│   └── layout.tsx
├── features/                      # one folder per backend domain
│   ├── auth/
│   │   ├── api/
│   │   ├── dto/
│   │   ├── mappers/
│   │   ├── models/
│   │   ├── hooks/
│   │   ├── components/
│   │   └── schemas/
│   └── <domain>/, ...              # same shape; do not add a domain the backend does not have
├── components/                       # domain-agnostic UI
├── lib/
│   ├── api-client.ts
│   ├── auth/
│   └── utils.ts
├── hooks/
├── stores/
├── types/
├── styles/
├── proxy.ts                              # Next 16 (Node). Do not add middleware.ts.
└── .env.local
```

- `(public)/`, `(app)/`, `(admin)/` match backend Public/User/Admin. Parentheses do not change the URL.
- `components/` is Button/Modal/DataTable. The moment it knows a feature's data, it moves into that feature.
