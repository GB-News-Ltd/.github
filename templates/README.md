# Templates

Copy these into new **product** repositories (Backend, Frontend Web, Android, iOS, Services, Pipelines) and replace placeholders.

| Path | Use |
|------|-----|
| [`README.example.md`](README.example.md) | Service README (`master` / `dev` / `staging`) |
| [`CODEOWNERS`](CODEOWNERS) | Flat Lead (+ QA) ownership |
| [`workflows/ci.yml`](workflows/ci.yml) | CI + CodeQL + merge-source guard for the branch model |
| [`workflows/merge-source-guard.yml`](workflows/merge-source-guard.yml) | Standalone guard caller (optional) |
| [`adr/NNNN-title.md`](adr/NNNN-title.md) | Architecture Decision Record |
| [`RELEASE_NOTES.md`](RELEASE_NOTES.md) | Release / changelog skeleton (tag on `master`) |
| [`dependabot.yml`](dependabot.yml) | App-oriented Dependabot starter |

After copy:

1. Create `master`, `staging`, `dev` (cut last two from `master`).
2. Apply [branch protection](../docs/handbook/branch-protection.md) / org rulesets (squash-only, delete head branches).
3. Mark required checks: CI + **Merge source guard**.

Also copy org community files only when you need to **override** defaults (SECURITY, CONTRIBUTING, issue forms).
