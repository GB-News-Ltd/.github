# Branching, naming & protection

Canonical policy for **Backend, Frontend Web, Android, iOS, Services, Pipelines** repos. Derived from *Git Branching Strategy, Naming Convention & Branch Protection* (Engineering Team Management).

Org meta-repo (`GB-News-Ltd/.github`) may use `main`; product repos use the model below.

## Model

```
master  ←── staging  ←── dev  ←── feature/* | bugfix/* | chore/* | spike/*
  ▲                            ▲
  └── hotfix/* ──cherry-pick───┴── also into staging
```

- **`dev` and `staging` are both cut from `master`**, not from each other.
- Solid path: `feature|bugfix` → `dev` → `staging` → `master`.
- Emergency path: `hotfix/*` cut from `master` → merge to `master` (or `staging` if not yet in prod) → cherry-pick into `staging` and `dev`.

## Branch definitions

| Branch | Environment | Accepts merges from | Reviews | CI focus |
|--------|-------------|---------------------|---------|----------|
| `master` | Production | `staging` only; `hotfix/*` under emergency process | **2** approvals incl. QA Lead | Smoke / sanity on merge; auto-deploy prod |
| `staging` | Staging / UAT | `dev` (release-ready subset); `hotfix/*` emergency | **1** + QA Lead sign-off | Full regression before cut to `master` |
| `dev` | Dev / Integration | `feature/*`, `bugfix/*`, `chore/*`, `spike/*`, `hotfix/*` | **1** team Lead / designated | Branch-sanity, build, lint, unit |
| `feature/*` | — (no env) | n/a (cut from `dev`) | 1 before merge to `dev` | Lint, unit, build on push |
| `bugfix/*` | — | cut from `dev` → `dev` | same as feature | same |
| `hotfix/*` | — | cut from `master` → `master` (+ cherry-picks) | 1 Lead + QA Lead ack | Full CI; not skippable |

Staging bugs that cannot wait for the normal flow are fixed via **`hotfix/*` from `master`**, then cherry-picked into `staging` and `dev`.

## Naming

```
<type>/<ticket-id>-<short-description>
```

| Type | Use |
|------|-----|
| `feature` | New capability |
| `bugfix` | Non-emergency fix found on `dev` / normal QA |
| `hotfix` | Staging or production emergency |
| `chore` | Tooling, deps, cleanup |
| `spike` | Time-boxed investigation |
| `release` | Optional release-prep branch |

Rules:

- Lowercase kebab-case only (no spaces, underscores, camelCase).
- **Always include a ticket/issue ID** — Jira (`GBN-1234`), Linear, or GitHub issue (`1234` / `#1234`). Jira specifically is optional; an untracked ID is discouraged (`no-ticket` only for trivial work).
- Description: 3–6 words.

Examples:

```
feature/GBN-1423-add-google-login
bugfix/GBN-1487-fix-cart-total-rounding
hotfix/GBN-1502-payment-gateway-timeout
chore/GBN-1490-upgrade-eslint-config
spike/GBN-1499-evaluate-graphql-gateway
```

## Merge strategy

**Squash merge** at every level (`feature|bugfix|hotfix` → `dev`, `dev` → `staging`, `staging` → `master`). PR titles become the squash commit message — keep them Conventional Commit style (`feat:`, `fix:`, `chore:`).

Enable **Automatically delete head branches** on every product repo.

## Environments

| Branch | Deploys to | Notes |
|--------|------------|--------|
| `master` | Production | Auto on merge after smoke; tag `vX.Y.Z`; changelog |
| `staging` | Staging / UAT | Auto on merge; notify QA Lead |
| `dev` | Dev / Integration | Auto on merge; branch-sanity suite |
| `feature/*`, `bugfix/*`, `hotfix/*` | **None** | No preview/ephemeral envs — local + CI only |

## CODEOWNERS

Per-team repos: **flat ownership** — one Lead (or team) for `*`. Path-scoped owners only when a repo outgrows single-Lead review (likely Backend / Services first).

```
* @GB-News-Ltd/<team-lead-or-team>
```

QA Lead must be in the review path for merges into `staging` and `master` (team + `@GB-News-Ltd/qa-leads` or equivalent).

## Emergency merges

1. Branch `hotfix/<ticket>-…` from `master`.
2. PR into `master` (or `staging` if not in prod yet).
3. Expedited approval: **1 Lead + QA Lead acknowledgement** (Slack-logged ack is OK if recorded on the PR).
4. Automated sanity **must** pass — not skippable.
5. Cherry-pick into `staging` and `dev` immediately.
6. Label PR `emergency-merge`; review in the next retro.

## CI map (GitHub Actions)

| Trigger | Pipeline |
|---------|----------|
| Push to `feature/*` / `bugfix/*` / `chore/*` / `spike/*` | Lint, unit, build |
| PR targeting `dev` | Lint, unit, build, CodeQL |
| Merge to `dev` | Deploy Dev + branch-sanity |
| Merge to `staging` | Deploy Staging + full regression; notify QA |
| Merge to `master` | Deploy Prod + smoke; tag; changelog |
| `hotfix/*` PR | Full CI + expedited review; notify on-call/Leads |

Enforce allowed merge sources with the reusable [merge-source-guard](../../.github/workflows/merge-source-guard.yml) workflow (GitHub cannot natively restrict source branch).

## Branch protection (summary)

Full checklist: [branch-protection.md](branch-protection.md). Prefer **org rulesets** over per-repo rules so all six stack repos stay consistent.

## Related

- [CONTRIBUTING.md](../../CONTRIBUTING.md)
- [branch-protection.md](branch-protection.md)
- Source doc: `Git-Branching-Strategy` (Engineering Team Management)
