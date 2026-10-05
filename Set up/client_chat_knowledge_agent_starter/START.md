# START.md — New Project Setup Guide

## Purpose

This folder is a reusable template for a folder-based AI agent that organizes client conversations into a structured, traceable project knowledge base.

This file is for the developer starting a **new project from this template**.

The goal is to create a completely fresh project without carrying knowledge, state, messages, attachments, or assumptions from the previous project.

---

# 1. Copy the Template

Copy the complete template folder to a new project folder.

Example:

```text
Client_Chat_Knowledge_Agent_Template/
        ↓
My_New_Project/
```

Do not work directly inside the original reusable template.

The copied folder becomes the project's own knowledge base.

---

# 2. Start Fresh

Before processing any project information, remove all project-specific data from the copied template.

The following folders must start empty unless deliberately populated with the new project's source material:

```text
01_Source_Chats/
02_Knowledge_Base/Important_Messages/
02_Knowledge_Base/Fixes/
02_Knowledge_Base/Requirements/
02_Knowledge_Base/Requirement_Updates/
02_Knowledge_Base/Enhancements/
02_Knowledge_Base/Decisions/
02_Knowledge_Base/Unknown_Needs_Review/
03_Attachments/Available/
03_Attachments/Missing_or_Unavailable/
04_Index/Source_Anchors/
07_Logs/
```

The previous project's processing state must also be removed/reset.

Reset:

```text
05_State/processing_state.json
```

It must represent a new project with no previously processed messages.

Do not copy:
- old chat exports;
- old message IDs;
- old important-message records;
- old requirements/fixes;
- old attachment records;
- old logs;
- old JSON indexes/state;
- old project-specific phase information.

---

# 3. Create the New Project Overview

Create or replace:

```text
Overview.md
```

This file contains only authoritative high-level information about the new project.

Include, when available:

- project name;
- short project description;
- client/company name;
- development team or relevant roles;
- current applications/products;
- backend/shared services;
- known environments;
- current project phases;
- important project terminology;
- important constraints;
- links to authoritative project documentation.

Do not invent information.

Unknown information should remain unknown until supplied.

---

# 4. Define the New Project Phases

Update:

```text
04_Index/phase_reference.md
```

Replace the template/example phases with the actual phases of the new project.

For example:

```text
1. Phase A — Initial Development
2. Phase B — Admin Enhancement
3. Mobile Application
4. Backend Modernization
```

Use the names supplied by the project/client/team.

Do not invent phases from the conversation merely because the AI thinks they would be useful.

If the project has no formal phases, say so explicitly.

The agent may use:

```text
Cross-Phase
Unknown / Needs Review
```

when a message cannot safely be assigned to a specific phase.

---

# 5. Add Source Conversations

Put original client conversations under:

```text
01_Source_Chats/
```

Recommended organization:

```text
01_Source_Chats/
├── WhatsApp/
└── Legacy_Fiverr/
```

Other communication sources can be added when needed, for example:

```text
01_Source_Chats/
├── WhatsApp/
├── Legacy_Fiverr/
├── Email/
└── Telegram/
```

Do not rewrite the original source files.

Keep the original exports whenever possible.

You may also create a normalized Markdown copy when the original format does not support reliable message-level linking.

---

# 6. Add Client Files and Attachments

Put files supplied by the client under:

```text
03_Attachments/Available/
```

Examples:

```text
03_Attachments/Available/
├── requirements.zip
├── design.pdf
├── reference.xlsx
└── screenshots/
```

Do not fabricate missing files.

If a conversation references a file that is not available, the agent must record it under:

```text
03_Attachments/Missing_or_Unavailable/
```

and ask the developer/user to provide it when necessary.

If the user confirms that the file is lost or unavailable, record that fact and continue using only supported information from the conversation.

---

# 7. Do Not Modify AGENT.md Unless Necessary

```text
AGENT.md
```

contains the core operating rules.

It is designed to be reusable across projects.

Normally, leave it unchanged.

Modify it only when the new project genuinely requires additional rules that are not project-specific data.

Project-specific information belongs in:

```text
Overview.md
04_Index/phase_reference.md
```

not in the generic operating rules.

---

# 8. The Important Message Principle

The system has a central evidence/history layer:

```text
02_Knowledge_Base/Important_Messages/
```

This is where materially important client/project messages are tracked.

Each important message should retain:

- source;
- medium;
- timestamp;
- sender;
- stable message ID;
- evidence/excerpt;
- significance;
- links to affected knowledge records.

Knowledge-base records must link back to their source messages.

This provides traceability from:

```text
Client Message
      ↓
Important Message
      ↓
Requirement / Fix / Enhancement / Decision
```

Never create fake source links or invent timestamps/anchors.

---

# 9. Preserve History

Historical records must not be deleted merely because the client later changes the requirement.

Example:

```text
Original Requirement
        ↓
Client changes requirement
        ↓
New Requirement Update
```

The original record remains in the knowledge base and is marked as:

```text
Superseded / Changed Later
```

It must link to the later record.

The later record should link back to the original record.

This preserves the project's decision history.

---

# 10. Duplicate Handling

Repeated client messages do not automatically require duplicate knowledge-base records.

If a later message adds nothing new:

- retain the important message when appropriate;
- link it to the existing record;
- do not create unnecessary duplicate requirement/fix records.

If the later message adds new information, update the relevant record or create a requirement-update record.

---

# 11. Never Guess

This is a mandatory project rule.

The AI agent must not invent:

- requirements;
- client intent;
- technical details;
- phase assignments;
- dates;
- attachment contents;
- implementation decisions;
- dependencies;
- approvals;
- priorities.

When evidence is insufficient, use:

```text
Unknown / Needs Review
```

or ask the user when the missing information materially affects correct processing.

---

# 12. Initial Project Ingestion

Once the project setup is complete, run:

```text
PROMPT_INITIAL_INGESTION.md
```

The initial ingestion should process all historical source conversations supplied for this project.

Typical instruction:

```text
Read AGENT.md.

Perform the initial ingestion using PROMPT_INITIAL_INGESTION.md.

Process all available source conversations under:
01_Source_Chats/

Use Overview.md and 04_Index/phase_reference.md as project context.

Do not guess missing information.
```

The agent should then build the project's initial knowledge base.

---

# 13. Future Conversation Updates

When a new WhatsApp export or other conversation arrives:

1. Put the new source under the appropriate folder in `01_Source_Chats/`.
2. Put any supplied client files under `03_Attachments/Available/`.
3. Run `PROMPT_INCREMENTAL.md`.
4. Identify the new file/folder in the instruction.

Example:

```text
Read AGENT.md.

Process the newly added conversation using PROMPT_INCREMENTAL.md.

New source:
01_Source_Chats/WhatsApp/2026-10-10/
```

The agent must compare new information with the existing project knowledge base.

It must not treat the new conversation as an isolated project.

---

# 14. Before the First Run

The developer should verify:

```text
[ ] Old project chats removed
[ ] Old attachments removed
[ ] Old KB records removed
[ ] Old logs removed
[ ] processing_state.json reset
[ ] Overview.md updated
[ ] phase_reference.md updated
[ ] New source chats added
[ ] New client attachments added, if available
[ ] AGENT.md present
[ ] PROMPT_INITIAL_INGESTION.md present
[ ] PROMPT_INCREMENTAL.md present
[ ] PROMPT_MISSING_ATTACHMENT.md present
```

Only after these checks should the initial ingestion begin.

---

# 15. Recommended Project Rule

Treat the following as different things:

### Source
What the client actually said.

### Evidence
Important messages selected from the source conversation.

### Knowledge
What those messages mean for project tracking.

### State
What the AI agent has already processed.

### Attachments
External artifacts referenced by the conversation.

Keeping these separate prevents the knowledge base from becoming an untraceable AI-generated summary.

---

# 16. Final Principle

This system is designed to make a messy client conversation manageable without losing historical context.

The agent should always prefer:

```text
Evidence > assumption
Traceability > convenience
History > deletion
Explicit uncertainty > guessing
Structured records > uncontrolled summaries
```

The developer's job when starting a new project is mainly to supply the correct project context and source material.

The agent's job is to organize, classify, link, preserve, and maintain the project knowledge base according to `AGENT.md`.
