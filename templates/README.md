# Templates

Copy these into new **product** repositories (Backend, Frontend Web, Android, iOS, Services, Pipelines) and replace placeholders.

| Path | Use |
|------|-----|
| [`README.example.md`](README.example.md) | Service README (`master` / `dev` / `staging`) |
| [`CODEOWNERS`](CODEOWNERS) | Flat Lead (+ QA) ownership |
| [`workflows/ci.yml`](workflows/ci.yml) | CI + merge-source guard + **CI gate** (the required check) for the branch model |
| [`workflows/codeql.yml`](workflows/codeql.yml) | CodeQL on PRs into `dev` (separate from CI) |
| [`workflows/deploy.yml`](workflows/deploy.yml) | Deploy-after-CI pattern: deploy jobs `needs: ci` |
| [`workflows/merge-source-guard.yml`](workflows/merge-source-guard.yml) | Standalone guard caller (optional) |
| [`adr/NNNN-title.md`](adr/NNNN-title.md) | Architecture Decision Record |
| [`RELEASE_NOTES.md`](RELEASE_NOTES.md) | Release / changelog skeleton (tag on `master`) |
| [`dependabot.yml`](dependabot.yml) | App-oriented Dependabot starter |

After copy:

1. Create `master` (or `main`), `staging`, `dev` (cut last two from the production branch). Workflows accept either name.
2. Apply [branch protection](../docs/handbook/branch-protection.md) / org rulesets (squash-only, delete head branches).
3. Opt in to the required check: set the custom property `ci_gate` to `true` on the repo. The org ruleset `require-ci-gate` then requires **CI gate** on `master`/`main`, `staging` and `dev`. Only do this once `ci.yml` with the `CI gate` job is on those branches, or every PR is blocked.

Also copy org community files only when you need to **override** defaults (SECURITY, CONTRIBUTING, issue forms).
