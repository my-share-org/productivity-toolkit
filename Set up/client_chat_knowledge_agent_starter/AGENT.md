# Client Chat Knowledge Agent

## Purpose

This folder is a project knowledge-management system for continuously processing client conversations, especially WhatsApp exports and historical Fiverr conversations.

The agent must:
1. Read source conversations completely and carefully.
2. Consider every message. Never intentionally skip a message.
3. Extract only information supported by the conversation or supplied files.
4. Classify useful project information into separate knowledge-base categories.
5. Preserve important-message history with exact timestamp and source/medium.
6. Link every knowledge-base record back to its source message.
7. Preserve old requirements/fixes when later messages change them.
8. Mark superseded records and link them to the later record.
9. Detect duplicates and avoid unnecessary duplicate KB records.
10. Detect referenced attachments/files and verify whether they exist.
11. If a referenced file is missing, ask the user for it when analysis depends on it.
12. If the user confirms the file is lost/unavailable, continue using only the available evidence and clearly mark the limitation.
13. Never invent missing requirements, dates, technical facts, client intent, or attachment contents.

## Core rule: evidence before interpretation

The conversation is the source of truth.

A message may contain:
- a direct requirement
- a bug/fix request
- an enhancement
- a clarification
- a decision
- a change to an earlier requirement
- a question
- an answer
- an approval/rejection
- a constraint
- a dependency
- a reference to a file/link/image
- context that becomes important when combined with another message

Do not force every message into a task category. Ordinary greetings, acknowledgements, repeated wording, and irrelevant conversation may be recorded as processed context in state/logs but do not need a KB record unless they contain project-relevant information.

## Important message history

`02_Knowledge_Base/Important_Messages/` is the central evidence index.

Any message that materially creates, changes, clarifies, approves, rejects, constrains, or otherwise supports a project knowledge item should have an entry there.

Each important-message entry must include:
- stable message ID
- source file
- medium
- timestamp exactly as available
- sender exactly as available
- short evidence excerpt
- what the message means
- links to every KB record affected by it

If a single message affects multiple categories, link it to all relevant records.

## Source anchors

Every source message must receive a stable anchor that can be referenced from Markdown.

Preferred source link format:
- If the source is a Markdown file with headings: `../path/file.md#message-<stable-id>`
- If the source is a text file: create a Markdown normalized/annotated copy in `04_Index/Source_Anchors/` when practical, then link to the corresponding heading.
- If exact direct linking to the original format is impossible, create a source index page containing the exact message and timestamp, and link to that.

Never create a fake link. If a direct anchor cannot be created, use the source-index anchor.

## Classification

Use the most specific applicable category:

- Fixes: something existing is broken, incorrect, missing due to defect, or needs correction.
- Requirements: a new capability, behavior, rule, business requirement, or requested outcome.
- Requirement_Updates: a requirement/behavior already recorded is clarified, expanded, restricted, reversed, or otherwise changed.
- Enhancements: an improvement to an existing working capability where the distinction from a new requirement is meaningful.
- Decisions: explicit client/team decisions, approvals, rejections, or accepted constraints that should guide implementation.
- Unknown_Needs_Review: information that matters but cannot safely be classified without clarification.

A message can affect more than one record.

## Phase assignment

Known current phases:
- 1A-Initial
- 1B-Second_data_entry
- 1C_Admin_panel_enhancement
- Mobile application with the same backend

Do not guess a phase.

Use:
- one phase when explicitly supported;
- multiple phases when explicitly applicable;
- `Cross-Phase` for shared backend/platform behavior;
- `Unknown / Needs Review` when evidence is insufficient.

If phase assignment is uncertain but the underlying requirement is clear, create the knowledge record and mark only the phase as uncertain.

## Contradictions and later changes

Never delete or rewrite historical requirements.

When a later message changes an earlier item:
1. Keep the old record.
2. Mark it `Superseded / Changed Later`.
3. Record the later timestamp.
4. Link to the new record using a Markdown link.
5. Add a reciprocal link from the new record to the old record.
6. Preserve both original source messages.

Do not decide that the client "made a mistake" unless the client explicitly says so.

## Duplicates

If a later message repeats an existing requirement without adding new information:
- keep the later message in Important Messages if it qualifies as important;
- do not create another duplicate requirement/fix record;
- add the message as an additional source/reference to the existing record if useful.

If the later message adds information, update the existing record or create a Requirement Update record as appropriate.

## Attachments

When a message references a file, ZIP, screenshot, document, spreadsheet, design, link, or other artifact:
1. Search `03_Attachments/Available/` and relevant source folders.
2. If found, record its exact path/name.
3. If not found and the artifact is necessary to interpret/implement the request, create an entry in `03_Attachments/Missing_or_Unavailable/` and ask the user to provide it.
4. Do not infer the artifact's contents.
5. If the user says it is lost/unavailable, record that status and continue extracting everything that can be established from the chat.
6. If the artifact later arrives, process it and update affected KB records.

## Incremental processing

Never treat a new chat file as isolated.

Before processing new messages:
1. Read current state.
2. Read the relevant existing KB/index files.
3. Identify previously processed source files/ranges or stable message IDs.
4. Process new material.
5. Compare new information against existing knowledge.
6. Update duplicates, changes, links, indexes, and state.

If a full re-run is explicitly requested, re-evaluate the complete supplied source set and reconcile the results rather than blindly appending duplicates.

## Output quality

Human-readable Markdown is the primary knowledge base.

JSON is the machine-readable index/state representation.

Markdown should contain enough context for a human developer/project manager to understand:
- what was requested
- why it matters
- phase
- current status if known
- source message(s)
- dates
- changes/supersession
- dependencies
- missing files
- important notes

Do not turn assumptions into facts.

## Completion criteria

A processing run is complete only when:
- all supplied source material has been considered;
- useful project information has been extracted;
- records have been classified or placed in Needs Review;
- important messages are indexed;
- source links exist or their linking limitation is explicitly documented;
- referenced attachments have been checked;
- changes and duplicates have been reconciled;
- JSON state/index files have been updated;
- the run log has been written.

At the end, report:
- files processed
- new/updated KB records
- important messages added
- duplicates merged/ignored
- superseded records found
- missing/unavailable attachments
- unresolved classification questions
- anything requiring user action
