# Contributing to GB News engineering

We ship reliable software for a live newsroom. Precision beats volume.

Branching, protection, and naming are defined in the [branching handbook](docs/handbook/branching.md). This page is the day-to-day short form.

## Before you start

1. Confirm ownership in [`repo-metadata/repos.yml`](repo-metadata/repos.yml) and the [handbook](docs/handbook/README.md).
2. Open tracked work with a **ticket/issue ID** (Jira, Linear, or GitHub Issue). **Jira itself is optional** — any of those IDs satisfies naming.
3. For architecture-affecting work, draft an [ADR](templates/adr/NNNN-title.md) when blast radius spans more than one service.

## Branch model (product repos)

| Branch | Role |
|--------|------|
| `master` | Production source of truth |
| `staging` | Release candidate / UAT |
| `dev` | Integration |
| `feature/*`, `bugfix/*`, `chore/*`, `spike/*` | Cut from `dev` → merge to `dev` |
| `hotfix/*` | Cut from `master` → merge to `master` (emergency), then cherry-pick to `staging` + `dev` |

`dev` and `staging` are both cut from `master`, not from each other.

### Naming

```
<type>/<ticket-id>-<short-description>
```

Examples: `feature/GBN-1423-add-google-login`, `bugfix/GBN-1487-fix-cart-rounding`, `hotfix/GBN-1502-payment-timeout`.

Lowercase kebab-case only. Prefer small, frequent merges; delete head branches after squash merge.

### Reviews (minimums)

| Into | Approvals |
|------|-----------|
| `dev` | 1 (team Lead) |
| `staging` | 1 + QA Lead sign-off |
| `master` | 2 including QA Lead |
| `hotfix/*` → `master` | 1 Lead + QA Lead ack; label `emergency-merge` |

**Squash merge only** at every level. Details: [branch-protection.md](docs/handbook/branch-protection.md).

## Commits

[Conventional Commits](https://www.conventionalcommits.org/) — PR titles become squash messages on `dev` / `staging` / `master`.

```
feat(cms): allow homepage modules to schedule 90 days ahead

GBN-1234
```

Use Draft PRs for WIP so CI runs without inviting review.

## Pull requests

- Fill the PR template — **ticket ID**, test plan, rollback for risky changes.
- No direct pushes / force-pushes to `dev`, `staging`, or `master`.
- Merge-source guard must pass (wrong source branch fails CI).
- `feature/*` / `bugfix/*` / `hotfix/*` have **no** preview environments — validate via local + CI, then on `dev`.

### Review bar

- Correctness and edge cases (timezone, empty states, authz)
- Observability where failure modes matter
- Security (authn/authz, injection, dependency risk)
- Operability (flags, migrations, rollback)

## CI

```yaml
jobs:
  ci:
    uses: GB-News-Ltd/.github/.github/workflows/reusable-ci.yml@main
    with:
      node-version: "22"
    secrets: inherit
  merge-source-guard:
    uses: GB-News-Ltd/.github/.github/workflows/merge-source-guard.yml@main
```

Copy the full caller from [`templates/workflows/ci.yml`](templates/workflows/ci.yml).

## Security

Report vulnerabilities privately — see [SECURITY.md](SECURITY.md).

## Code of conduct

[CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

## Questions

[SUPPORT.md](SUPPORT.md) · [branching.md](docs/handbook/branching.md)
