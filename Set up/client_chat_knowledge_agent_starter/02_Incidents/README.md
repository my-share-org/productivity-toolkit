# Incidents Knowledge Base (`02_Incidents/`)

## 1. What This Folder Does
This directory hosts the central chronological event timeline of the entire project: **[Incidents.md](Incidents.md)**.

Every conversation batch, client review, security advisory, scope agreement, or instruction from the client is logged here as a distinct **Incident**.

---

## 2. Structure of `Incidents.md`
`Incidents.md` is structured for fast reading and instant navigation:
1. **Top Table (Quick Jump Index):** A high-level overview table indexing every incident with Date, Title, Type, References, and Target Action. Clicking an incident title jumps directly to its detailed section.
2. **Bottom Incident Cards:** Detailed sections providing the complete context, verbatim excerpts, summary, and action items.

---

## 3. How to Add a New Incident

When new communication arrives:
1. **Assign Sequential Number:** If the highest incident is `Incident 01`, the new one will be `Incident 02`.
2. **Add Entry to Top Table:**
   ```markdown
   | [02](#incident-02-short-title) | YYYY-MM-DD HH:MM | Short Title | Type | [Chat Ref](../01_Source_Chats/...) | [Attach Ref](../03_Attachments/...) | [Target File](../05_Fixes/...) |
   ```
3. **Append Incident Card Below:**
   ```markdown
   ---

   ### Incident [XX]: [Short Title]
   - **Date-Time:** YYYY-MM-DD HH:MM
   - **Sender & Medium:** [Name / WhatsApp / Slack / Email]
   - **Chat Reference:** `01_Source_Chats/...`
   - **Attachment Reference:** `03_Attachments/...` (or None)
   - **Type:** Fix / New Requirement / Requirement Update / Query / Other
   - **Target Action File:** [Link to Fix or Requirement file]

   #### Summary
   [2-4 sentence summary of what happened]

   #### What We Need to Do / What Was Told to Do:
   1. [Action item 1]
   2. [Action item 2]
   ```
4. **Link to Target Record:**
   - If it is a bug $\rightarrow$ Create or link in `05_Fixes/`
   - If it is a new or updated requirement $\rightarrow$ Create or update in `04_Requirements/`
