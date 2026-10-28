# Global rules

Apply in every session, every stack. Stack mechanics live in `<stack>_rules.md`. Load that file for the current stack. Load a recipe only when that stack file names it.

## Agent behavior

- If a technical decision, architecture, or code is wrong, say so. Do not agree to impress. Objective engineering first.
- If we disagree, say why, with industry practice or a concrete standard, so the correction can be checked.
- Production quality only. Reject a hack, a "somehow" fix, or any change that lowers quality to ship faster.
- Flag security holes, edge cases, and structural weaknesses as soon as you see them — even if not asked.
- If a requirement is unclear, stop and ask. Do not guess, and do not lower quality to force an answer.
- Do not silently change, remove, or "improve" working code outside what was asked.
- When more than one valid approach exists, state the trade-off. Do not pick in silence.
- Never invent library, API, or framework behavior. Verify it before using it.
- Follow the project's existing patterns. Do not introduce a new style without a reason.
- Match complexity to the actual feature. Do not over-engineer.

## How to talk (chat only)

English is not the user's first language (intermediate, especially reading). This block applies to chat explanations and summaries, not to code, comments, or technical names.

- Use short, plain English. Prefer a simple word ("use", "fix") over a fancy one ("utilize", "remediate").
- Prefer bullets or short paragraphs.
- For a new or complex idea, add a short real-life analogy.

## Architecture

- One job per function or module. Split unrelated work.
- Modules talk through interfaces (signatures, APIs), not each other's internals.
- Keep related logic in the same module. Split business logic, data access, and presentation.
- Build for today's requirement. Do not add abstraction, config, or patterns for a future need.
- Do not repeat logic, but do not force a shared helper if it makes the code harder to follow.
- Match the project's naming and folder style. Do not mix feature-based and layer-based layouts.

## Code quality

- Names describe purpose (`getUserById`, not `getData`).
- Comments explain why, not what.
- Aim for functions under ~40 lines. More than 3 nested if/for levels: use early returns.
- Follow the project's linter/formatter. Add docstrings/type hints on public functions, especially backend and shared libraries.

## Security

- Secrets, API keys, DB passwords, and tokens come from env vars or a secret manager — never from code.
- Validate every input (user, API, file). Parameterized queries or ORM only — never concatenated SQL. Encode output to prevent XSS.
- Least privilege on DB users, IAM roles, and API keys.
- Access tokens: short-lived (15 min–1 hour), verified on the server (signature + expiry). Refresh tokens: longer-lived, httpOnly cookie (not localStorage), rotated on use. If auth uses httpOnly cookies, CSRF protection is required (SameSite or CSRF tokens).
- Rate-limit every public endpoint. CORS: known origins only — never `*` in production with credentials.
- Never log passwords, tokens, card numbers, or personal data. Mask if it must appear.
- Before installing any package, in any language: check recent advisories, malware reports, and unusual publish activity for that exact version. Do not default to "latest." If the version is flagged, walk backward one version at a time with the same check. If no version is clean, pick another package and run the same process. Check significant transitive dependencies too. Pin the chosen version in the lock file.

## Errors, performance, tests

- Handle or raise every error. No empty catches. Time out every external call. Retry transient failures with exponential backoff. Writes that can be retried must be idempotent. Log internals; return a generic message to the client — never a stack trace.
- Watch N+1 queries. Paginate every large list. Cache only with a clear invalidation plan. Prefer non-blocking I/O for network and disk. Do not optimize without a measured bottleneck.
- Test core business logic, especially money, auth, and data integrity. Cover empty, null, huge, and wrong-type input. Skipping tests is not a shortcut.

## Config, infra, git

- URLs, keys, flags, and limits come from env vars. Separate values for dev, staging, and production. Missing required config must fail startup.
- Prefer infrastructure as code. Never commit `.env` or credentials. CI runs lint + tests before merge to main. Add monitoring, structured JSON logs, and alerting from the start. Have a rollback plan before a production deploy. Cloud: least-privilege security groups, no public databases, snapshot before resize or clone.
- Small atomic commits. Message says what and why. Check for secrets before every commit. Branch names: `feature/`, `fix/`, `hotfix/`.
