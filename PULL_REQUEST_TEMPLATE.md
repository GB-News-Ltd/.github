## Summary

<!-- Intent first (why), then what changed. 1–3 bullets. No file dumps. -->

-

## Traceability

| Field | Value |
|-------|--------|
| Ticket / issue | <!-- `GBN-1234` · Linear · GitHub `#123` — required on the branch name; Jira specifically optional --> |
| Head → base | <!-- e.g. `feature/GBN-1234-add-oauth` → `dev` --> |
| Related PRs / ADRs | <!-- n/a or links --> |

## Change class

- [ ] `feat` — new capability
- [ ] `fix` — bugfix / hotfix
- [ ] `refactor` — behaviour-preserving restructure
- [ ] `chore` — deps, CI, tooling, spike
- [ ] `docs` — documentation only
- [ ] `security` — hardening / vuln remediation
- [ ] **Breaking** — callers or contracts change (call out migration below)

## Merge path

Select the path this PR is on (must match [branching policy](https://github.com/GB-News-Ltd/.github/blob/main/docs/handbook/branching.md)):

- [ ] **Integration** — `feature|bugfix|chore|spike/*` → `dev`
- [ ] **Release cut** — `dev` → `staging`
- [ ] **Production** — `staging` → `master`
- [ ] **Emergency** — `hotfix/*` → `master` or `staging`  
      - [ ] Labelled `emergency-merge`  
      - [ ] Cherry-pick plan for `staging` **and** `dev` documented below  
      - [ ] QA Lead acknowledgement recorded on this PR

## Validation

### Test plan

<!-- Commands a reviewer can run + expected signal (exit code, assertion, panel). -->

```bash
# e.g. npm test -- <scope>
# e.g. curl -sS "$BASE_URL/health" | jq .
```

- [ ] Unit / component
- [ ] Integration / e2e (if applicable)
- [ ] Manual / exploratory (steps below)

Manual steps:

1.
2.

### Observability & evidence

<!-- Attach or link: screenshots, traces, logs, Grafana/Datadog, before/after. -->

-

## Risk & operability

| Dimension | Assessment |
|-----------|------------|
| Blast radius | <!-- services, surfaces, audience segments --> |
| Feature flag | <!-- name · default off? · n/a --> |
| Rollback | <!-- revert squash · previous tag · flag off — required for `staging`/`master` --> |
| Data / migrations | <!-- none · expand/contract · dual-write · backfill --> |
| Perf / cost | <!-- n/a · expected delta --> |
| Security / privacy | <!-- authz, PII, secrets — n/a if untouched --> |

## Checklist

- [ ] Branch: `<type>/<ticket-id>-<short-kebab-description>`
- [ ] PR title is squash-ready (`feat:`, `fix:`, `chore:`, …) — it becomes the commit on the target
- [ ] Self-reviewed diff; no secrets, credentials, or audience PII
- [ ] Tests updated for behaviour changes; CI green
- [ ] Docs / runbooks updated if operators need to know
- [ ] Merge-source guard will pass for this head → base
- [ ] CODEOWNERS / QA path will request the right reviewers
