# Initial Ingestion Prompt

You are performing the INITIAL ingestion of this project's historical client conversations.

## Scope

Read:
- `01_Source_Chats/WhatsApp/`
- `01_Source_Chats/Legacy_Fiverr/`

Also inspect:
- `Overview.md` if present
- existing `Phases/` material if it has been retained/migrated into this system
- all available project-reference files relevant to interpreting the conversations.

Do not assume that the newest conversation is the only source of truth.

## Mandatory procedure

1. Read `AGENT.md` completely.
2. Inventory all conversation files.
3. Read every available conversation message in chronological/source order.
4. Do not intentionally skip messages.
5. Extract project-relevant information.
6. Create the Important Messages history.
7. Create separate KB records for:
   - Fixes
   - Requirements
   - Requirement Updates
   - Enhancements
   - Decisions
   - Unknown / Needs Review
8. Assign phases only when supported by evidence.
9. Detect duplicates.
10. Detect later changes/supersession.
11. Detect all referenced attachments/files.
12. Check whether referenced files are available.
13. Ask the user for missing files when necessary.
14. If the user says a file is unavailable/lost, record that fact and continue without guessing its contents.
15. Create/update machine-readable JSON indexes/state.
16. Create a processing log.
17. Reconcile links between Important Messages and all KB records.

## Historical preservation

Do not delete useful historical information simply because it is old.

If an old requirement was later changed:
- preserve the old requirement;
- mark it superseded;
- link it to the later requirement.

If an old requirement is still current, mark it as current/active when the evidence supports that.

## Do not ask unnecessary questions

Ask the user only when a missing artifact, ambiguous classification, or unresolved contradiction materially affects the accuracy of the knowledge base.

Do not ask for information that can be safely extracted from the conversation.

## Final report

After processing, give a concise report with:
- source files processed
- important messages captured
- KB records created/updated
- duplicates handled
- superseded items
- missing files
- unresolved items requiring user input
- exact files the user should inspect
