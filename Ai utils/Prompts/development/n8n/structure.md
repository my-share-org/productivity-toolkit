# n8n backup store

> Last verified: n8n 1.x. Read when placing or naming workflow backup JSON.

```
backups/
├── main_workflow/
│   ├── v1/
│   │   └── 1_26-09-03_22-50_successful-image-upload-done.json
│   ├── v2/
│   └── ...
└── sub_workflow/
    ├── process-webflow-tags/
    │   ├── v1/
    │   │   └── 1_26-09-03_22-55_tags-updated.json
    │   └── ...
    ├── send-confirmation-email/
    │   ├── v1/
    │   │   └── 1_26-09-03_22-56_email-sent.json
    │   └── ...
    └── ...
```

1. Root: `backups/`
2. `main_workflow/` or `sub_workflow/`
3. Each sub-workflow: a folder named without a version tag
4. Inside that: `v1`, `v2`, `v3`, … — the `.json` file lives **inside** the version folder, never beside it.

File name: `{backup_number}_{yy-mm-dd_hh-mm}_{short-state}.json`

When and how to take backups: `backups.md`.
