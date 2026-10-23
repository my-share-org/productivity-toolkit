# Client Attachments (`03_Attachments/`)

## 1. What This Folder Does
This directory stores all non-chat documents, specifications, spreadsheets, design assets, developer packs, screenshots, and assets delivered by the client for **{{PROJECT_NAME}}**.

---

## 2. Subfolder Structure
- **`Available/`**: Every file or folder actually received and verified.
- **`Missing/`**: Contains `missing_files.md` tracking files referenced in client chats that have not yet been provided.

---

## 3. Naming Standard for New Attachments in `Available/`
Format:
```text
[MM-DD]_[hh-mm-am/pm (optional)]_[descriptive_title].[ext]
```
Examples:
- `10-11_requirements_brief.pdf`
- `10-15_01-18-am_database_schema_v2.xlsx`
- `10-20_ui_redesign_assets/` (folder)

### Rules:
1. Always use 2-digit month numbers (`01`, `02`, ..., `12`) for natural chronological sorting.
2. Never use spaces in filenames. Use hyphens (`-`) or underscores (`_`).
3. If an attachment is referenced in an incident, link to it in `02_Incidents/Incidents.md` and any relevant file in `04_Requirements/` or `05_Fixes/`.
