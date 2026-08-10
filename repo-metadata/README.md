# Repository metadata

Canonical ownership and classification for GB-News-Ltd repositories.

## Files

| File | Purpose |
|------|---------|
| [`repos.yml`](repos.yml) | repo → domain, owners, criticality, contacts |
| [`schema.md`](schema.md) | Field definitions |

## Rules

1. Every production repo **must** have a row before first prod deploy.
2. `owners.github_team` must match a real `@GB-News-Ltd/...` team.
3. Prefer updating this file in the same PR that creates a new repo.
4. Jira project keys are optional metadata, not a gate.
