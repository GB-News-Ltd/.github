# Security policy

GB News takes the security of our products, infrastructure, and audience data seriously.

## Supported versions

| Stream | Support |
|--------|---------|
| `master` / production releases | Security fixes actively applied |
| `staging` | Best-effort; report still welcome |
| `dev` / feature branches | Best-effort |
| Abandoned / archived repos | Not supported — please disclose anyway if impact is org-wide |

## Reporting a vulnerability

**Do not** open a public GitHub issue for security findings.

Prefer, in order:

1. **GitHub Private Vulnerability Reporting** on the affected repository (Security → Report a vulnerability), when enabled.
2. Email **security@gbnews.uk** with a clear subject: `[SECURITY] <service> — short title`.
3. If you are a staff engineer with access, raise via the internal security channel and link this policy.

### What to include

- Affected repository / service / URL
- Description and impact (confidentiality, integrity, availability)
- Reproduction steps or proof-of-concept (keep payloads minimal)
- Affected versions / commit SHAs / deploy IDs if known
- Your contact details and preferred disclosure timeline

We aim to acknowledge within **2 business days** and provide a remediation plan within **10 business days** for confirmed issues. Critical production exploits may be handled faster via incident process.

## Safe harbour

Good-faith research that follows this policy and avoids:

- Accessing or modifying audience/personal data beyond what is needed to demonstrate the issue
- Degrading production availability (no DoS / load testing against prod)
- Social engineering of staff or audiences

…will not result in legal action from GB News for that research.

## Detailed expectations

Operational and engineering requirements live in [SECURITY_POLICY.md](SECURITY_POLICY.md).
