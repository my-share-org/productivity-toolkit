# n8n git and backups

> Last verified: n8n 1.x. Read when exporting, changing a live workflow, or taking backups.

## Version control

- Export workflow JSON and commit it. n8n's internal history is not a reviewable diff.
- Editing an active production webhook workflow is a deploy. Review before save + activate.

## When to back up

Trigger if the user asked for a backup, **or** all of:

1. The workflow JSON changed after the last backup (last backup time < last change time), **and**
2. The change is one of: a node added or removed, a credential changed, webhook auth changed, or **3 or more** nodes edited.

## How to back up

1. List versions for the main workflow and each related sub-workflow.
2. Check last backup time per version.
3. Use MCP to inspect those workflows against the backup rules.
4. If a needed workflow has MCP off: ask the user to enable it and wait for "Continue", or continue only for MCP-enabled workflows.

Folder and file names: `structure.md`.
