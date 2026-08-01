# Framework: Incident — What Else after an outage?

**Trigger:** Incident resolved and root cause identified. Run before the postmortem is written.
**Output:** Extended postmortem with systemic gaps captured, not just the immediate fix.
**Skill:** `what-else-incident` (roadmap — not yet built)

## What to ask beyond root cause

### Detection
- What signal did we miss that would have caught this earlier?
- What dashboard or metric was absent or misleading?
- How long was the issue present before it was detected?

### Response
- What runbook was missing or incorrect?
- What slowed down the diagnosis?
- Who needed to be involved but wasn't notified?

### Prevention
- What test would have caught this before production?
- What assumption failed silently?
- Is the same failure mode present in other services we own?

### Documentation
- What was undocumented that made this harder to debug?
- What should be added to the runbook as a result?

### Systemic
- Is this a one-off or a pattern?
- What engineering practice would prevent this class of incident?
- Was there a previous incident with the same root cause?
