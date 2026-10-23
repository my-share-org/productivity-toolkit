# Available Client Attachments (`03_Attachments/Available`)

## 1. Purpose & Overview
This directory is the verified repository for all client-supplied files, attachments, specifications, spreadsheets, and design documents provided for **{{PROJECT_NAME}}**.

### Core Principles
- **Immutable Evidence:** Files placed here represent the authoritative source material provided by the client. Never edit or rewrite original client documents.
- **Traceability:** Every requirement, fix, or architectural decision in the knowledge base links back to these files or source chat exports.
- **Strict Distinction from Missing Files:** Only files that are actually present belong in this folder. Any referenced file that is missing must be tracked in `../Missing/missing_files.md`.

---

## 2. Mandatory Naming Standard

Every file and folder directly located in the root of `03_Attachments/Available/` must follow this naming structure:

```text
[MM-DD]_[hour-minute (optional)]_title[. Extension if file]
```

| Component | Format | Required? | Description & Examples |
|---|---|:---:|---|
| **`MM-DD`** | `MM-DD` | **Yes** | 2-digit month and day for natural chronological sorting (e.g. `09-23`, `10-15`). |
| **`[hour-minute]`** | `hh-mm-am/pm` | **Optional** | Timestamp if known from the chat export (e.g. `11-24-pm`, `02-00-pm`). |
| **`title`** | `string` | **Yes** | Descriptive title of document or folder (e.g. `admin_specs`, `data_mapping`). |
| **`[. Extension]`** | `.ext` | **For files only** | File extension (`.pdf`, `.xlsx`, `.zip`, `.png`). Omit for folders. |

---

## 3. Current Attachments Manifest

| Name | Type | Date / Time | Source / Sender | Contents & Description | Related KB Records |
|---|:---:|---|---|---|---|
| `[MM-DD]_[example_document.pdf]` | **File** (.pdf) | YYYY-MM-DD | {{CLIENT_NAME}} | Example client specification document. | [REQ-01](../../04_Requirements/REQ-00_template_feature_specification.md) |

---

## 4. How to Ingest New Client Attachments

When a new client file or folder arrives:
1. **Place in Directory:** Move or copy the incoming file/folder into `03_Attachments/Available/`.
2. **Apply Naming Standard:** Rename the root item to `MM-DD_[hour-minute]_title[.ext]`.
3. **Check Missing Artifacts List:** Check `../Missing/missing_files.md`. If this incoming file resolves a previously missing artifact, mark it as resolved and link to its path.
4. **Log Incident:** Create or update the corresponding event in `02_Incidents/Incidents.md`.
5. **Update Manifest:** Add the new file/folder entry to the manifest table above.
