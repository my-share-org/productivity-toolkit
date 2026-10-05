# Incremental Chat Processing Prompt

A new client conversation export or chat file has been provided.

The user may say:
- "Process this new chat file"
- "Process this folder"
- "I added a new message backup"
- "Process <filename>"
- or similar.

## Mandatory procedure

1. Read `AGENT.md`.
2. Read `05_State/processing_state.json`.
3. Read the existing indexes and relevant KB records.
4. Identify exactly which new source files/messages are being processed.
5. Read every message in the supplied new material.
6. Compare each project-relevant message against existing knowledge.
7. Add important messages to the Important Messages history.
8. Create or update the appropriate KB record.
9. Do not create duplicate records for unchanged repeated information.
10. If new information modifies an existing record, preserve the old record and create/update the change history according to `AGENT.md`.
11. Add reciprocal Markdown links between old and new records when something was superseded.
12. Check every referenced attachment.
13. Update JSON indexes/state.
14. Write the processing log.
15. Do not modify unrelated records without a reason supported by the new conversation.

## Important

A new chat file may contain references to old requirements. Therefore, compare it against the entire existing knowledge base.

Do not assume:
- the latest message is automatically correct;
- the latest message automatically cancels an old requirement;
- a phase from a nearby message applies to another message;
- a referenced file is available;
- a client statement is technically feasible.

Record what the client said. Technical feasibility can be tracked separately if supported by project/developer information.

## Final response

Report:
- new important messages
- new KB records
- updated KB records
- duplicate/repeated messages
- changed/superseded requirements
- missing attachments
- unresolved items
- files modified
