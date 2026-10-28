# Agent rule install

This folder is the source. Agents must **not** auto-load every file. Always-on is `global.md` + one `<stack>/core.md`. Recipes load only when that core file names them.

Relative imports below assume this directory is available to the agent as `Prompts/development/`. Adjust the prefix if you copy the files into a project.

## What to attach

| Session | Attach |
|---|---|
| Any | `global.md` |
| Django | `django/core.md` (also `backend/contract.md` — core tells you to) |
| FastAPI | `fastapi/core.md` + `backend/contract.md` |
| Frontend | `frontend/core.md` |
| n8n | `n8n/core.md` |

Do not attach `legacy/`. Do not attach a whole stack folder.

## Claude Code

- Put `global.md` contents (or `@` this file) in the user-level `CLAUDE.md`.
- In the project `CLAUDE.md`, import only the stack you are in, for example `@Prompts/development/django/core.md` and `@Prompts/development/backend/contract.md`.
- Do not `@` recipe files in `CLAUDE.md`. Core's index is the trigger. Path-scoped `.claude/rules/` may point at a recipe with a glob if you want automatic attach when those files are open.

Imported files load with the parent — do not import the whole tree.

## Cursor

Copy into `.cursor/rules/` as `.mdc` with frontmatter (plain `.md` in that folder is ignored):

- `global.mdc`: `alwaysApply: true`
- `<stack>-core.mdc`: `alwaysApply: true` only in a repo that is that stack. Otherwise `alwaysApply: false` and a `description` so the agent can pull it.
- Each recipe: `alwaysApply: false` plus a one-line `description` matching the trigger line in `core.md` (agent-requested). Optional `globs` if the recipe should attach when those files are open.

Example recipe frontmatter:

```yaml
---
description: Django domain patterns — payments, webhooks, ledgers, callbacks, dashboards, metering, debug routes
alwaysApply: false
---
```

## Other tools

If the product is not Claude Code or Cursor, attach by **file name** (`global.md` + `<stack>/core.md`), not by folder. If you meant a specific product named Integrity, its rule format is not in this toolkit — add an install block here when you have it.

## Layout

```
global.md
README.md          ← this file; do not auto-load
backend/
  contract.md
  domain-patterns.md
django/ fastapi/ frontend/ n8n/
  core.md
  structure.md testing.md domain-patterns.md tooling.md
  background-jobs.md   ← django, fastapi only
  limits.md backups.md ← n8n only
legacy/                ← old monoliths; do not load
```
