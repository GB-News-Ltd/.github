# GB News — `.github`

Org-wide defaults, reusable workflows, and repository templates for **GB-News-Ltd**.

These community health files apply automatically to repositories that do not define their own. Reusable workflows and composite actions are **opt-in** — call them from each repo.

Product repos (Backend, Frontend Web, Android, iOS, Services, Pipelines) follow the [branching strategy](docs/handbook/branching.md): **`dev` → `staging` → `master`**, squash merges, ticket-prefixed branch names.

## Layout

| Path | Purpose |
|------|---------|
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Issue forms (bug, feature, incident, chore) |
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | PR checklist |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How we ship |
| [`docs/handbook/branching.md`](docs/handbook/branching.md) | Branch model, naming, CI map |
| [`docs/handbook/branch-protection.md`](docs/handbook/branch-protection.md) | GitHub rulesets / protection checklist |
| [`SECURITY.md`](SECURITY.md) | Vulnerability reporting |
| [`SECURITY_POLICY.md`](SECURITY_POLICY.md) | Detailed security expectations |
| [`CODEOWNERS`](CODEOWNERS) | Default review routing |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | Conduct |
| [`SUPPORT.md`](SUPPORT.md) | Where to get help |
| [`.github/dependabot.yml`](.github/dependabot.yml) | Dependabot defaults (copy into app repos) |
| [`.github/workflows/`](.github/workflows/) | Reusable CI, CodeQL, merge-source guard |
| [`.github/actions/`](.github/actions/) | Composite actions |
| [`docs/handbook/`](docs/handbook/) | Team → domain → repo map |
| [`repo-metadata/`](repo-metadata/) | Canonical ownership YAML |
| [`templates/`](templates/) | Starter files for new repos |

## Quick start (new product repo)

1. Create long-lived branches: `master`, `staging`, `dev` (cut `staging` and `dev` from `master`).
2. Copy from [`templates/`](templates/): `CODEOWNERS`, Dependabot, [`workflows/ci.yml`](templates/workflows/ci.yml).
3. Apply [branch protection / org rulesets](docs/handbook/branch-protection.md) — squash-only, delete head branches.
4. Set flat CODEOWNERS to the repo Lead (+ QA Lead for release paths).
5. Register the repo in [`repo-metadata/repos.yml`](repo-metadata/repos.yml).

```yaml
jobs:
  ci:
    uses: GB-News-Ltd/.github/.github/workflows/reusable-ci.yml@main
    secrets: inherit
  merge-source-guard:
    uses: GB-News-Ltd/.github/.github/workflows/merge-source-guard.yml@main
```

## Conventions

- Branch names: `<type>/<ticket-id>-<short-description>` (`feature`, `bugfix`, `hotfix`, `chore`, `spike`).
- Ticket/issue ID required (Jira **or** Linear **or** GitHub Issue). Jira alone is not mandatory.
- Squash merge at every level; Conventional Commit PR titles.
- No preview environments for `feature/*` / `bugfix/*` / `hotfix/*`.
- Prefer YAML issue forms over free-form markdown.

## Ownership

Maintained by Platform Engineering. Propose changes via PR against this repository.
