# \<Service name\>

> One-line purpose. Audience-facing? Internal? Library?

## Status

![CI](https://github.com/GB-News-Ltd/<repo>/actions/workflows/ci.yml/badge.svg)

| | |
|--|--|
| **Stack** | Backend / Frontend Web / Android / iOS / Services / Pipelines |
| **Owner (Lead)** | `@GB-News-Ltd/<team-or-lead>` |
| **Default branch** | `master` |
| **Integration** | `dev` → `staging` → `master` |
| **On-call** | Link to schedule |
| **Runbook** | Link |
| **Ticket project** | e.g. GBN (Jira optional) |

## What this repo does

-

## Branching

See org [branching handbook](https://github.com/GB-News-Ltd/.github/blob/main/docs/handbook/branching.md).

```
feature|bugfix/<ticket>-slug  →  dev  →  staging  →  master
hotfix/<ticket>-slug          →  master  (+ cherry-pick staging, dev)
```

Squash merge only. No preview environments for feature/bugfix/hotfix branches.

## Local development

```bash
# prerequisites: …
cp .env.example .env
npm ci
npm run dev
```

## Deploy

| Branch | Environment | Trigger |
|--------|-------------|---------|
| `dev` | Dev / Integration | Merge to `dev` |
| `staging` | Staging / UAT | Merge to `staging` |
| `master` | Production | Merge to `master` (after smoke) |

## Contributing

Org [CONTRIBUTING.md](https://github.com/GB-News-Ltd/.github/blob/main/CONTRIBUTING.md).
