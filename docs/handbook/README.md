# Engineering handbook

Operating manual for GB News technology. Keep this short and link out.

## Principles

1. **Audience first** — reliability of live and breaking coverage beats clever architecture.
2. **Small reversible changes** — flags, forward-compatible migrations, clear rollback.
3. **Owners are explicit** — every repo has a Lead/team in [`repo-metadata/repos.yml`](../../repo-metadata/repos.yml).
4. **Tracked work** — every branch carries a ticket/issue ID; Jira specifically is optional.
5. **One path to prod** — `dev` → `staging` → `master` (see [branching.md](branching.md)).

## Branching & protection

| Doc | Contents |
|-----|----------|
| [branching.md](branching.md) | Model, naming, reviews, emergency hotfixes, CI map |
| [branch-protection.md](branch-protection.md) | GitHub rulesets, required checks, squash-only |

## Domains / stacks

Per-team repos (not a monorepo by default):

| Stack / domain | Owning Lead team (GitHub) |
|----------------|---------------------------|
| Backend | `@GB-News-Ltd/backend` (or Lead handle) |
| Frontend Web | `@GB-News-Ltd/frontend-web` |
| Android | `@GB-News-Ltd/android` |
| iOS | `@GB-News-Ltd/ios` |
| Services | `@GB-News-Ltd/services` |
| Pipelines | `@GB-News-Ltd/pipelines` |
| Platform / org defaults | `@GB-News-Ltd/platform-engineering` |
| QA Lead (release sign-off) | `@GB-News-Ltd/qa-leads` |
| Security | `@GB-News-Ltd/security` |

Update slugs when org teams are finalised. CODEOWNERS stay **flat** (`* @lead`) until a repo outgrows single-Lead review.

## Team → repo map

Canonical machine-readable map: [`repo-metadata/repos.yml`](../../repo-metadata/repos.yml).

| Repo | Domain | Owner | Criticality |
|------|--------|-------|-------------|
| `.github` | Platform | platform-engineering | High (org defaults) |
| _add product repos_ | | | |

## Criticality levels

| Level | Meaning | Expectations |
|-------|---------|--------------|
| **Critical** | Direct audience impact if down | On-call, SLO, CodeQL, rapid Dependabot SLA |
| **High** | Newsroom-facing or shared platform | On-call or business-hours pager |
| **Medium** | Internal tools with workaround | Business-hours support |
| **Low** | Experiments / archives | Best effort |

## Environments

Only long-lived branches map to running environments:

| Branch | Environment | Deploy |
|--------|-------------|--------|
| `master` | Production | Auto on merge after smoke; tag `vX.Y.Z` |
| `staging` | Staging / UAT | Auto on merge; QA regression |
| `dev` | Dev / Integration | Auto on merge; branch-sanity |
| `feature/*`, `bugfix/*`, `hotfix/*` | — | No preview env — local + CI only |

## Related docs

- [CONTRIBUTING.md](../../CONTRIBUTING.md)
- [SECURITY_POLICY.md](../../SECURITY_POLICY.md)
- [SUPPORT.md](../../SUPPORT.md)
