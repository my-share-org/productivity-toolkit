# Requirements Knowledge Base (`04_Requirements/`)

## 1. Purpose & Standards
This directory contains the authoritative specification files for every client feature, capability, and business rule across **{{PROJECT_NAME}}**.

### Core Rule: One File Per Requirement (No Duplicates)
- **Never create separate "update" files** (e.g., do NOT create `REQ-01_update.md` or `REQ-01_v2.md`).
- When a client amends or clarifies a requirement, **update the existing file**.
- **Top of the file:** Keep the `Current Finalized Specification` updated so anyone can read lines 1–25 and immediately understand what needs to be built right now.
- **Bottom of the file:** Append an entry to `Requirement Evolution & History Log` documenting what changed, who requested it, when, and linking to the source incident/chat.

---

## 2. File Naming Standard
```text
REQ-[SERIAL]_[snake_case_title].md
```
Examples:
- `REQ-01_user_authentication.md`
- `REQ-02_payment_gateway_integration.md`
- `REQ-03_admin_reporting_dashboard.md`

---

## 3. Standard Template for New Requirements
A ready-to-copy template is provided at:
**[`REQ-00_template_feature_specification.md`](REQ-00_template_feature_specification.md)**
