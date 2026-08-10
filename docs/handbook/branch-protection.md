# Branch protection & rulesets

Apply to every product repo (Backend, Frontend Web, Android, iOS, Services, Pipelines). Prefer **Organization rulesets** so enforcement is identical across repos.

Also set repo-wide: **Allow squash merging only** (disable merge commits & rebase), **Automatically delete head branches**.

## Shared settings (all of `master`, `staging`, `dev`)

- Require a pull request before merging
- Do not allow bypassing the above settings (except break-glass admins, audited)
- Block force pushes
- Block deletions
- Require status checks to pass
- Require branches to be up to date before merging
- Require conversation resolution before merging (recommended)
- Require signed commits (recommended, optional)

Wire required checks to the workflows in [`templates/workflows/`](../../templates/workflows/).

## `master` (production)

| Rule | Value |
|------|--------|
| Approvals | **2** |
| Code owners | Required (Lead + QA Lead via CODEOWNERS) |
| Who can merge | Release managers / QA Lead / Eng Leads only |
| Required checks | `CI / Lint · Test · Build`, `Merge source guard`, smoke/sanity job |
| Allowed sources | `staging`, `hotfix/*` — enforced by merge-source-guard |
| Deploy | Automatic after smoke; tag `vX.Y.Z` |

## `staging` (UAT)

| Rule | Value |
|------|--------|
| Approvals | **1** + QA Lead sign-off (CODEOWNERS) |
| Required checks | `CI / …`, `Merge source guard`, full regression/sanity |
| Allowed sources | `dev`, `hotfix/*` |

## `dev` (integration)

| Rule | Value |
|------|--------|
| Approvals | **1** (team Lead / designated via CODEOWNERS) |
| Required checks | `CI / …` (lint, unit, build), CodeQL on PRs, `Merge source guard` |
| Allowed sources | `feature/*`, `bugfix/*`, `chore/*`, `spike/*`, `hotfix/*` |

## `feature/*`, `bugfix/*`, `hotfix/*`, …

No branch protection on the head branch itself. Targets (`dev` / `master`) enforce PR + CI. Head branches auto-delete after squash merge.

## Org ruleset sketch

Create three rulesets targeting default + named branches:

1. **protect-master** — ref `refs/heads/master`
2. **protect-staging** — ref `refs/heads/staging`
3. **protect-dev** — ref `refs/heads/dev`

Include repositories: Backend, Frontend Web, Android, iOS, Services, Pipelines (and future product repos). Exclude `GB-News-Ltd/.github` if it remains on `main`.

## Required workflow: merge-source-guard

Add to each product repo (see [`templates/workflows/merge-source-guard.yml`](../../templates/workflows/merge-source-guard.yml)):

```yaml
jobs:
  guard:
    uses: GB-News-Ltd/.github/.github/workflows/merge-source-guard.yml@main
```

Mark **Merge source guard** as a required status check on `master`, `staging`, and `dev`.

## Break-glass

Admin bypass of rulesets must page Platform / Security and be logged. Emergency code fixes still use `hotfix/*` + `emergency-merge` label — do not disable checks to skip sanity.
