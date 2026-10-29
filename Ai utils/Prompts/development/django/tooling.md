# Django tooling

> Last verified: 2026-10-29 — Django 5.2 LTS. Read when changing Docker, env, CI, Bruno, lockfile, or project docs.

## Env, Docker, CI

- `env_config.py` is the only place env vars are read and validated. `settings.py` builds Django settings from those values — no scattered `os.environ.get()`.
- Separate compose files: `db` vs `api`, `local` vs `production`. Never point local at the production compose file.
- Lockfile: `uv.lock`, or `pip-compile --generate-hashes`. CI: lint + tests + `pip-audit` on every PR. CI Docker should match production closely enough that "passes CI" predicts production.

## Docs

- `developer_guide/` — how a new feature should be built (response/service/utility conventions). Follow it when it exists.
- `docs/` — architecture, setup, onboarding.
- `developers_files/` — test results, evaluations, security audits, if kept.
- A repeated manual step becomes a CLI command in the project toolkit, not a README line that gets forgotten.

## Bruno (only if the project uses it)

Collection in `bruno-collection/`. Tree mirrors Django apps.

1. Audience: `Admin/`, `User/`, or `Public/` — who calls it.
2. Django app folder name (or the project's established short name, e.g. `auth`). Never a duplicate folder.
3. Optional third level for a large app.
4. Every folder: `folder.bru` with `meta { name: ... }`.

- One `.bru` per endpoint, kebab-case from the view action. Source of truth: `<app>/v1/urls.py`, views, serializers. Never invent an endpoint, field, or status. URL: `{{BASE_URL}}/v1/...`.
- `Public/`: `auth: none`. `User/`: `auth: inherit`. `Admin/`: explicit bearer `{{ADMIN_ACCESS_TOKEN}}` — inherit sends the user token and 403s.
- Tokens: `bru.setVar()` only. Never `bru.setEnvVar()` (writes JWTs into tracked env files). Secrets: declare under `vars:secret`, never a real value in the file.
- Every request: `docs { }` with title, method, auth, permission, body/query (types and limits), response shape, errors by status, note for non-obvious behavior.
- After folder or env-var add/rename: update `bruno-collection/README.md`.
