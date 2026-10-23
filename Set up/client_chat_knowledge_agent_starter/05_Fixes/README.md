# Fixes Knowledge Base (`05_Fixes/`)

## 1. What This Folder Does
This directory tracks, documents, and organizes all bug fixes, UI defects, API errors, and data corrections across **{{PROJECT_NAME}}** (`{{PROJECT_DOMAIN}}`).

### Core Principles
- **Instant Status Visibility:** Uses prefixes (`0_` vs `1_`) so developers and PMs can immediately see what is open and what is resolved directly from the file explorer.
- **Actionable Facts Only:** Contains exact URLs, API endpoints, error messages, parameters, and database tables needed to reproduce and fix issues without guesswork.
- **Traceability:** Links directly back to verified client messages in `02_Incidents/Incidents.md` and attachments in `03_Attachments/`.

---

## 2. Structure Standard & Lifecycle

### Dedicated Issue Subfolder Architecture
Every fix issue has its own dedicated subfolder containing the 5 standard files:

```text
05_Fixes/
└── [STATUS]_[SERIAL]_[title_snake_case]/
    ├── README.md                      <- Executive summary, status, metadata & file navigator
    ├── Problem_Statement.md           <- User/client communication, defect summary, observed vs. expected
    ├── Problem_Investigation.md       <- Server DB queries, logs, root cause analysis & failure chain
    ├── Proposed_Solutions.md          <- Evaluated architecture options, guard designs & trade-offs
    └── Implementation_Plan.md         <- Step-by-step code/DB changes, verification checklist & rollback
```

---

### Status Codes & Lifecycle

| Status Code | Meaning | When to Use | Folder Example |
|:---:|---|---|---|
| `0_` | **Pending / Not Fixed** | New issue identified; actively being investigated or coded. | `0_01_checkout_500_error/` |
| `1_` | **Done / Resolved** | Fix has been implemented, deployed, and verified. | `1_01_checkout_500_error/` |

#### Step-by-Step Fix Lifecycle:
1. **Create Subfolder with `0_` Prefix:** Find the next serial number (e.g., `0_01_issue_title/`) and copy the templates from `0_00_template_issue/`.
2. **Document Problem & Investigation:**
   - Record user/client messages and observed vs expected behavior in `Problem_Statement.md`.
   - Query databases and server logs, document IDs, and determine the root cause in `Problem_Investigation.md`.
3. **Architect Solutions & Implementation Plan:**
   - Evaluate options and select the cleanest approach in `Proposed_Solutions.md`.
   - Write concrete code changes and test checklists in `Implementation_Plan.md`.
4. **Implement & Verify:**
   - Execute the code changes and verify against the checklist.
5. **Mark Done & Rename:**
   - Update status to `Done` in the issue's `README.md`.
   - Rename folder prefix from `0_` to `1_` (e.g. `1_01_checkout_500_error/`).
   - Update entry status in `02_Incidents/Incidents.md` and the manifest below.

---

## 3. The 5 Standard Fix Files Explained

| File Name | Purpose & Contents |
|---|---|
| **`README.md`** | **Dashboard & Navigator:** Metadata (Status, Severity, Target Entities), quick summary, and jump links to the 4 detailed documents. |
| **`Problem_Statement.md`** | **Defect Specification:** Plain-English summary of what broke, exact client messages, evidence links, and clear Observed vs. Expected Behavior tables. |
| **`Problem_Investigation.md`** | **Technical Diagnosis:** Verified live server records (database IDs, endpoints, logs), failure sequence, root cause analysis, and critique of initial assumptions. |
| **`Proposed_Solutions.md`** | **Solution Architecture:** Multiple solution options evaluated with pros/cons, trade-offs, defensive design principles, and selected strategy. |
| **`Implementation_Plan.md`** | **Execution Blueprint:** Actionable phases, exact code modifications, verification test cases, deployment checklist, and rollback plan. |

---

## 4. Current Fixes Manifest

| Folder / Filename | Status | Serial | Issue Title | Priority |
|---|:---:|:---:|---|---|
| `0_00_template_issue/` | **Template** | `00` | Starter Template Issue | N/A |
