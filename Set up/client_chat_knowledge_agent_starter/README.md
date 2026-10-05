# Client Chat Knowledge Agent

This package is a starter system for a folder-based AI agent operated through Gemini CLI / Indy Gravity IDE.

## Operating model

1. Put source chat exports in `01_Source_Chats/`.
2. Put client-provided artifacts in `03_Attachments/Available/`.
3. Run the initial ingestion prompt once.
4. For future exports, run the incremental prompt and identify the new file/folder.
5. The agent updates Markdown + JSON knowledge files.

## Current project phases

- 1A-Initial
- 1B-Second_data_entry
- 1C_Admin_panel_enhancement
- Mobile application with the same backend

Do not add a new phase merely because the AI thinks one exists. Add phases only from authoritative project material or explicit user instruction.

## Important design principle

The system stores evidence separately from interpretation:
- Important Messages = what was said and when.
- KB records = what that evidence means for project tracking.
- JSON = machine-readable state/index.
- Source anchors = traceability.
- Attachment records = evidence availability.

This prevents the knowledge base from becoming an untraceable AI-generated summary.
