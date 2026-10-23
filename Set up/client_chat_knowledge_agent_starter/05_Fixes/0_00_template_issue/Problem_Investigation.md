# Problem Investigation: [Issue Title]

## 1. Verified System State & Entity Context
Record verified IDs, database rows, endpoints, and environment context:
- **Affected URL / Endpoint:** `POST /api/v1/...`
- **Entity IDs:** User ID `123`, Session ID `456`, Order ID `789`
- **Environment:** Production / Staging
- **Database Tables Involved:** `users`, `orders`, `transactions`

---

## 2. Chronological Failure Sequence
Trace the exact step-by-step execution path leading to failure:
1. User triggers action at timestamp `HH:MM:SS`.
2. Endpoint receives payload: `{ ... }`.
3. Handler executes query: `SELECT ...`.
4. Exception thrown in `service.py:124`: `NullPointerException / KeyError`.

---

## 3. Root Cause Analysis
[Technical explanation of why the bug occurs. Distinguish between symptoms and the true underlying cause.]
