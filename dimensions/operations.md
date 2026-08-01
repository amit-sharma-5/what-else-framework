# Dimension: Operations

## What to ask

- Is there a rollback strategy if the deployment goes wrong?
- Is the feature behind a feature flag for safe incremental rollout?
- Is there a runbook for on-call if this feature causes an incident?
- Has capacity been estimated — storage growth, request rate, downstream load?
- Are there scheduled jobs or crons? What happens if they overlap or fail?
- Are background tasks observable (last run, success/failure, duration)?
- Is the deployment zero-downtime? Are DB migrations backward-compatible?
- Is an SLO / error budget defined for this feature?
- Has on-call been notified this feature is going live? Is there a handoff note?
