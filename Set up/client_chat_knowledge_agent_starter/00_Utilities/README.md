# Utilities & Environment Configuration (`00_Utilities/`)

## 1. What This Folder Does
This folder stores essential operational, environment, and repository configurations needed to work on **{{PROJECT_NAME}}**.

It serves as the central reference point for both human developers and AI coding agents to locate repositories, understand codebase paths, and access relevant servers safely without searching through chat histories or external notes.

---

## 2. Directory Contents & Navigation

| File | Purpose & Contents | Primary Consumers |
|---|---|---|
| **[`Repository locations.md`](Repository%20locations.md)** | Absolute and relative paths to all project codebases (Backend, Frontend, Mobile, AI/Microservices). | Developers & AI Agents |
| **[`Server access.md`](Server%20access.md)** | SSH access commands, IP addresses, usernames, and key paths for Main Production and Staging/Test environments. | DevOps, Developers & AI Agents |

---

## 3. Detailed File Summaries

### A. Repository Locations (`Repository locations.md`)
Maintains the exact local directory paths for each component of the project:
- **Backend:** Active production backend service and any auxiliary APIs.
- **Frontend:** Web client portal, admin management dashboard, or landing page.
- **Mobile:** Mobile client codebase (iOS / Android / Flutter / React Native).
- **Services / Workers:** AI engines, background queue workers, or data pipelines.

> **Rule for updating:** Whenever repositories are cloned to new paths, renamed, or new services are added, update `Repository locations.md` immediately.

### B. Server Access (`Server access.md`)
Documents the exact SSH commands and connection details for all deployed server environments:
- **Main Server:** Production environment.
- **Test Server:** Staging and QA testing environment.
- **Auxiliary Servers:** Database hosts, AI processing machines, or worker clusters.

---

## 4. Strict Security & Operational Rules

1. **Never Expose or Print Key Files (`.pem` / SSH keys):**
   - **NEVER** read, dump, print, or copy `.pem` file contents into chat transcripts, incident files, or git commits.
   - Only reference the key's file path when executing SSH or deployment commands.
2. **Keep Credentials Confidential:**
   - Never store raw passwords, secret tokens, or API private keys in plain text in this folder.
   - Use environment variables (`.env`) inside the code repositories themselves.
3. **Keep Paths Accurate:**
   - All repository paths must point to existing local directories. If a folder moves or is archived, update the file status accordingly (e.g., mark as `[Archived]` or `[Deprecated]`).
4. **AI Agent Usage:**
   - Before searching for code or making edits, AI agents should check `Repository locations.md` to identify the right codebase and avoid editing wrong or deprecated rebuild directories.
