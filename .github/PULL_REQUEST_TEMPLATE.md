## Summary

<!-- What changed and why (1–3 bullets). Lead with intent, not file lists. -->

-

## Ticket / issue ID

<!-- Required: Jira (e.g. GBN-1234), Linear, or GitHub issue #. Jira specifically is optional. -->

-

## Target branch

- [ ] `dev` ← `feature/*` | `bugfix/*` | `chore/*` | `spike/*`
- [ ] `staging` ← `dev` (release cut) or `hotfix/*`
- [ ] `master` ← `staging` or `hotfix/*` (emergency)
- [ ] Emergency: labelled `emergency-merge`; cherry-pick plan for `staging` + `dev`

## Type of change

- [ ] Bug fix (`bugfix` / `hotfix`)
- [ ] New feature
- [ ] Breaking change
- [ ] Chore / spike / docs / CI
- [ ] Security hardening

## Test plan

<!-- Exact steps a reviewer can run. Prefer commands + expected signals. -->

- [ ]
- [ ]

## Risk & rollout

| Area | Notes |
|------|--------|
| Blast radius | <!-- services / audiences touched --> |
| Feature flag | <!-- name / default off? / n/a --> |
| Rollback | <!-- required for risky / master-bound changes --> |
| Data / migrations | <!-- none / forward-only / dual-write --> |
| Screenshots (UI) | <!-- attach below or n/a --> |

## Checklist

- [ ] Branch named `<type>/<ticket-id>-<short-description>`
- [ ] Self-reviewed; no secrets, credentials, or PII
- [ ] Tests added/updated where behaviour changed
- [ ] Docs / runbooks updated if operators need to know
- [ ] CODEOWNERS / QA Lead will auto-request where required
- [ ] Squash-ready PR title (`feat:`, `fix:`, `chore:`, …)
- [ ] Hotfix only: QA Lead acknowledgement recorded on this PR

## Screenshots / evidence (if UI or observable)

<!-- Before/after, Grafana panel, trace, curl output -->
