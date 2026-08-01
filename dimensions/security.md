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
