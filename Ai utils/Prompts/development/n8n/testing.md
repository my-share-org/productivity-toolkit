# n8n testing

> Last verified: n8n 1.x. `core.md` is already loaded.

- Pin data or use Manual Trigger. Do not re-fire a real webhook or spend live API credits.
- Test a sub-workflow alone in the editor before wiring it into a parent.
- Never test against production credentials or a live payment/CRM. Staging instance or sandbox credentials.
- HMAC: confirm Raw Body is on and a bad signature stops before any write node.
