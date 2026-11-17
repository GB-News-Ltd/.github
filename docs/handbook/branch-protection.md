# Branch protection & rulesets

Apply to every product repo (Backend, Frontend Web, Android, iOS, Services, Pipelines). Prefer **Organization rulesets** so enforcement is identical across repos.

Also set repo-wide: **Allow squash merging only** (disable merge commits & rebase), **Automatically delete head branches**.

## Shared settings (all of `master` / `main`, `staging`, `dev`)

The production branch is `master` or `main` (repos use one; workflows and rulesets cover both).

- Require a pull request before merging
- Do not allow bypassing the above settings (except break-glass admins, audited)
- Block force pushes
- Block deletions
- Require status checks to pass
- Require branches to be up to date before merging
- Require conversation resolution before merging (recommended)
- Require signed commits (recommended, optional)

Wire required checks to the workflows in [`templates/workflows/`](../../templates/workflows/). The org requires **one** check, `CI gate`; see [Required check: CI gate](#required-check-ci-gate).

## `master` / `main` (production)

| Rule | Value |
|------|--------|
| Approvals | **2** |
| Code owners | Required (Lead + QA Lead via CODEOWNERS) |
| Who can merge | Release managers / QA Lead / Eng Leads only |
| Required checks | `CI gate` (covers lint/test/build and Merge source guard), smoke/sanity job |
| Allowed sources | `staging`, `hotfix/*` — enforced by merge-source-guard |
| Deploy | Automatic after smoke; tag `vX.Y.Z` |

## `staging` (UAT)

| Rule | Value |
|------|--------|
| Approvals | **1** + QA Lead sign-off (CODEOWNERS) |
| Required checks | `CI gate`, full regression/sanity |
| Allowed sources | `dev`, `hotfix/*` |

## `dev` (integration)

| Rule | Value |
|------|--------|
| Approvals | **1** (team Lead / designated via CODEOWNERS) |
| Required checks | `CI gate` (lint, unit, build, Merge source guard); CodeQL on PRs (separate workflow) |
| Allowed sources | `feature/*`, `bugfix/*`, `chore/*`, `spike/*`, `hotfix/*` |

## `feature/*`, `bugfix/*`, `hotfix/*`, …

No branch protection on the head branch itself. Targets (`dev` / `staging` / `master` / `main`) enforce PR + CI. Head branches auto-delete after squash merge.

## Org ruleset sketch

Create three rulesets targeting default + named branches:

1. **protect-master** — refs `refs/heads/master` and `refs/heads/main`
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

The guard is one of the jobs `CI gate` depends on, so requiring `CI gate` also requires the guard.

## Required check: CI gate

Every product repo's `ci.yml` ends with a job named exactly **`CI gate`** ([template](../../templates/workflows/ci.yml)). It `needs` every other CI job, runs with `if: always()`, and fails unless each of them passed or was skipped. The org requires only this one name, so repos can add, rename or remove CI jobs without anyone changing a ruleset.

**Why a separate, opt-in ruleset.** A required check that never reports blocks every PR. Several org repos have no workflows, and the others name their jobs differently, so requiring checks in `protect-master` / `staging-protect` would block those repos. Instead, a repo opts in with a custom property once its `CI gate` job is in place.

### Org settings (owner)

1. **Organization → Settings → Repository → Custom properties → New property**
   - Name `ci_gate`, type **True/false**, default **false**, "Allow repository actors to set this property" **off**.
2. On each repo whose `ci.yml` has the `CI gate` job on `master`/`main`, `staging` (and `dev` if used): **Organization → Settings → Custom properties**, select the repo, set `ci_gate` = `true`.
3. **Organization → Settings → Rules → Rulesets → New branch ruleset**
   - Name `require-ci-gate`, Enforcement **Active**.
   - Bypass list: none. Admins can still deploy fixes through a `hotfix/*` PR.
   - Target repositories: **Include by property** `ci_gate` = `true`.
     If property targeting isn't offered, use **Include by name** and list the opted-in repos instead (add each repo when it gets its `CI gate` job).
   - Target branches: include `master`, `main`, `staging`, `dev` (by name; missing branches are ignored). The production branch may be called `master` or `main`, so list both.
   - Rule **Require status checks to pass**: add the check `CI gate`; leave "Require branches to be up to date" off (squash-only flow); "Do not require status checks on creation" on.
   - Leave everything else to `protect-master` / `staging-protect` / `protect-dev`.
4. Check it on one repo with `gh api repos/GB-News-Ltd/<repo>/rules/branches/staging`. The output must include `required_status_checks` with context `CI gate`.

## Production branch: `master` or `main`

The workflows and templates treat `master` and `main` the same: PRs into either may only come from `staging` or `hotfix/*` (the guard's `master_allowed`), and a deploy workflow deploys production from either. Rulesets must match. On 12 Nov 2026 no org ruleset protected `main` in any repo, including repos whose default branch is `main`, so anyone with write access could push to it. Add `refs/heads/main` to `protect-master` (and `require-ci-gate`) before any deploy workflow or `production` environment accepts `main`.

## Deploy only after CI

A required check stops a PR from merging, but not an admin bypass or a manual workflow run. Every deploy workflow must therefore call the repo's `ci.yml` on the same commit and deploy only if it passed ([template](../../templates/workflows/deploy.yml)):

```yaml
jobs:
  ci:
    uses: ./.github/workflows/ci.yml
    permissions: { contents: read, pull-requests: write }
    with: { skip-merge-guard: true }
  deploy:
    needs: ci
    if: needs.ci.result == 'success'
```

Keep CodeQL in its own workflow (`codeql.yml`). Otherwise every caller of `ci.yml` has to grant `security-events: write`.

## Break-glass

Admin bypass of rulesets must page Platform / Security and be logged. Emergency code fixes still use `hotfix/*` + `emergency-merge` label — do not disable checks to skip sanity.
