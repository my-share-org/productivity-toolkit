# n8n scale and outbound limits

> Last verified: n8n 1.x. Read when the workflow loops on an external API, runs in production load, or is a scheduled batch.

- **HTTP Request node:** use its Batching options — Items per Batch and Batch Interval (ms) — sized to the provider's documented limit.
- **Other nodes** (no Batching option): `Split in Batches` / `Loop Over Items` + `Wait`. Honor `Retry-After` when the provider sends it.
- Production: **queue mode** (main + workers), Postgres not SQLite, env separate from single-process. Set execution concurrency on purpose — default unbounded concurrency exhausts DB pools when one upstream is slow.
- Watch queue depth as a metric. Growing depth means arrivals beat workers, before executions start failing.
