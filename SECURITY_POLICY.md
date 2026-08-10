# Security policy (engineering)

Companion to [SECURITY.md](SECURITY.md). This document is the engineering baseline for GB-News-Ltd repositories.

## Identity & access

- Prefer short-lived credentials (OIDC to cloud, fine-scoped tokens).
- No long-lived cloud keys in Actions when OIDC is available.
- Repository secrets are scoped to the environment that needs them (`production`, `staging`).
- CODEOWNERS must cover auth, infra-as-code, and workflow directories.

## Dependencies

- Enable Dependabot (see [`.github/dependabot.yml`](.github/dependabot.yml) template).
- Merge security updates within SLA:

  | Severity | Target |
  |----------|--------|
  | Critical | 7 days |
  | High | 14 days |
  | Medium | 30 days |
  | Low | Best effort / next dep cycle |

- Pin Actions to full commit SHAs for high-trust workflows; tags are acceptable for internal org actions when immutable releases are used.

## CI / supply chain

- Required status checks on `master`.
- Disallow `pull_request_target` with untrusted checkout + privileged secrets.
- CodeQL (or equivalent SAST) on default branch for first-party application repos — use [`codeql-analysis.yml`](.github/workflows/codeql-analysis.yml).
- Block commits that introduce secrets (org secret scanning + push protection).

## Data & privacy

- No production audience PII in logs by default; redact tokens, emails, IP where not required.
- Test fixtures must be synthetic.
- Cross-border data handling follows GB News legal/privacy guidance (internal).

## Change management

- Production (`master`) changes: PR from `staging` (or `hotfix/*` under emergency process) + required reviews + CI green — see [branching.md](docs/handbook/branching.md).
- Prefer feature flags for risky launches; default flag **off** until soak.
- Security hotfixes use `hotfix/<ticket-id>-…` from `master`, label `emergency-merge`, cherry-pick into `staging` and `dev`.
- Incidents follow the [incident issue form](.github/ISSUE_TEMPLATE/incident.yml) and team runbooks.
- Sanity/smoke suites are **not skippable**, including emergencies.

## Vulnerability handling (internal)

1. Triage severity (CVSS-informed, but impact-on-audience wins).
2. Open private tracking (GitHub Security Advisory and/or Jira — Jira optional).
3. Patch, release, and notify relevant owners from [`repo-metadata/repos.yml`](repo-metadata/repos.yml).
4. Post-incident: root cause + prevention item within 5 business days for SEV-1/2.

## Exceptions

Temporary exceptions require Platform Engineering + service owner acknowledgement, with an expiry date recorded in the owning team's tracker.
