# n8n core

> Last verified: 2026-10-29 — n8n 2.45. Always loaded with `global.md`. When this workflow is the other end of a backend dispatch, also read `../backend/contract.md` (callback token) and `../backend/domain-patterns.md` pattern C.

**Read a recipe before you write if the task matches.** Several matches: read all of them. Do not open a recipe the task does not match.

- Backup folder layout or naming → `structure.md`
- Tests, pinned data, Manual Trigger, staging credentials → `testing.md`
- Inbound webhooks, leads callback, scheduled sync, alerting domains → `domain-patterns.md`
- Queue mode, outbound rate limits, bulk loops → `limits.md`
- Git export, live-workflow change, automatic backup when/how → `backups.md`

## Graph

- Data is an array of items between nodes, even when there is one item.
- Triggers sit on the left with no input: Webhook, Schedule, Manual, Error, Execute Workflow Trigger.
- Prefer expressions and built-in nodes (`Set`, `IF`, `Switch`, `HTTP Request`) over a `Code` node.
- Pin node output while building so you do not re-hit a real webhook or spend API credits.
- Default node names (`HTTP Request1`, `IF1`) are never acceptable. Rename to the job: `Validate Lead Payload`.

## Modularity and names

- `Execute Workflow` (parent) + `Execute Workflow Trigger` (child) = reusable function.
- Extract a sub-workflow when **node count > 20** or **IF/Switch branches > 3**.
- One job per sub-workflow, one short sentence. Input fields on the trigger are a contract — check every parent before changing them.
- Sub-workflows buy memory isolation, isolated tests, and fix-once reuse. Do not extract a one-off trivial step.
- Workflow names describe the job, not a version or initials. Folders/tags follow backend domain names.
- Extract a shared sub-workflow when the same node group appears in **2 or more** workflows.

## Secrets and webhooks

- Credentials only through n8n's credential manager. Never a literal in `Code` / `Set` / `HTTP Request`, even "for a minute."
- One least-privilege credential per integration. At scale: Vault / AWS Secrets Manager.
- Before export/share/git: check JSON for a hardcoded key. Default Webhook auth is **none** — allowed locally, never in production.
- At least two layers: (1) Webhook Header/JWT auth (Basic only over HTTPS, low-sensitivity internal), (2) HMAC in a `Code` node, (3) IP allowlist at the proxy when ranges are known.
- HMAC setup: turn **Raw Body** on in the Webhook node. Hash the raw body, not re-serialized JSON. Timing-safe compare + timestamp anti-replay. On self-hosted n8n, set `NODE_FUNCTION_ALLOW_BUILTIN=crypto` so the Code node may `require('crypto')`. With external task runners, set `crypto` in the runners config file (`/etc/n8n-task-runners.json` env-override), not the container env — the container env is silently ignored.
- Verify before any expensive node. Ack `200`/`202` fast, then process async — slow responses cause provider retries. Webhook URLs are secrets; rotate them. Keep webhook intake off the editor network when you can.

## Errors and validation

- Per node On Error: `Continue` only for optional steps. Critical: stop or error-output.
- Retry on Fail: **3** retries. Built-in delay is **fixed** — no exponential backoff. For rate-limited APIs add a `Wait` with jitter so workers do not retry in lockstep.
- Every workflow: Error Workflow (Error Trigger) that logs (name, node, message, execution id, time, severity) to a durable store **and** alerts Slack/PagerDuty/email. Retries without an Error Workflow = a silent miss.
- Exhausted retries → dead-letter for manual reprocess, never a quiet stop. Ledger/payment failures must reach the backend compensating action (`../backend/domain-patterns.md`).
- Validate inbound shape before any write or dispatch. `Code` nodes stay small and single-purpose; credentials via expressions, never pasted.

## Before you write

- Node count > 20 or branches > 3 → extract. Same group in 2+ workflows → shared sub-workflow.
- Webhook → not default-none; Raw Body on if HMAC (`domain-patterns.md` if inbound provider). Credentials → manager only. Export → scan JSON.
- External calls: 3 retries + Error Workflow. Loops: `limits.md`. Failure → dead-letter.
- If it does not map, say so.

## Load again

Same list as the top. If this change matches a line, read that file before writing. If several match, read all. If none match, stop here.
