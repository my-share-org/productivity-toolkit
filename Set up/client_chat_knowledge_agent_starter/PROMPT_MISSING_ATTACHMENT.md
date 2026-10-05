# Missing Attachment Handling Prompt

A conversation references a file/artifact that is not currently available.

Do not guess its contents.

Determine:
1. exact filename/name if stated;
2. message timestamp;
3. sender;
4. what the client said the artifact contains or is for;
5. which KB records depend on it;
6. whether processing can continue without it.

If the artifact is required to accurately interpret the request:
- create/update a record under `03_Attachments/Missing_or_Unavailable/`;
- mark the dependent KB item as `Blocked by Missing Artifact` or `Needs Artifact`;
- ask the user to upload it to `03_Attachments/Available/`.

If the user confirms the artifact is lost/unavailable:
- record `Unavailable - confirmed by user`;
- do not invent or infer its contents;
- extract all information that is independently supported by the conversation;
- continue processing the remaining material.

When the artifact is later provided:
- analyze it;
- link it to the original message;
- update affected KB records;
- preserve the previous missing/unavailable status as historical information.
