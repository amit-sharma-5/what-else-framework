# Dimension: Security

## What to ask

- Is the endpoint authenticated?
- Is authorization enforced — not just "is the user logged in" but "can this user do this action"?
- Does auth context propagate through internal service calls, or do internal calls bypass auth?
- Is sensitive data (PII, credentials, tokens) logged anywhere?
- Are sensitive fields excluded from error responses?
- Is there an audit log for actions that modify data?
- Are inputs sanitized to prevent injection (SQL, command, template)?
- Are there any new secrets or credentials? Are they managed (not hardcoded)?
- Does this feature expose more data than the caller needs?
- Are third-party dependencies checked for known vulnerabilities (CVE / `go mod audit` / `npm audit`)?

## Public repository checklist

If this artifact (EDR, skill, framework file) will be committed to a public repo:

- Does it contain real internal ticket numbers, service names, or team names?
- Does it contain real hostnames, internal URLs, or VCS paths?
- Does it contain employee names, email addresses, or org-specific identifiers?
- Do the examples use generic placeholders (`your-org`, `service-a`, `PROJ-123`) not real names?
- Are unresolved critical findings present? (Must be resolved before public commit — see `SECURITY.md`)
