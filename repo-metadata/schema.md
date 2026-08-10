# repo-metadata schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | GitHub repository name |
| `description` | string | yes | Short purpose |
| `domain` | enum | yes | `backend` \| `frontend-web` \| `android` \| `ios` \| `services` \| `pipelines` \| `platform` \| `security` \| `other` |
| `criticality` | enum | yes | `critical` \| `high` \| `medium` \| `low` |
| `owners.github_team` | string | yes | e.g. `GB-News-Ltd/backend` (flat Lead team) |
| `owners.escalate` | string | no | Secondary team (often `qa-leads` / platform) |
| `default_branch` | string | no | Product repos: `master`. Meta: `main` |
| `long_lived_branches` | string[] | no | Product: `[master, staging, dev]` |
| `contacts.oncall` | string | no | URL or schedule name |
| `contacts.slack` | string | no | Channel name |
| `jira.project` | string | no | e.g. `GBN` (optional tracker) |
| `languages` | string[] | no | Primary languages |
| `lifecycle` | enum | no | `active` \| `maintenance` \| `deprecated` \| `archived` |
| `notes` | string | no | Free text |
