# Framework: Bug Fix — What Else after fixing?

**Trigger:** Bug fix merged and deployed.
**Output:** EDR saved to `edrs/` focused on the systemic gap the bug exposed.
**Skill:** `/what-else-review` (same skill as feature — use `--context "bug fix: <summary>"`)

## What to ask beyond the fix itself

- Was this a symptom of a deeper systemic issue?
- Could the same bug exist elsewhere in the codebase?
- What test would have caught this before it reached production?
- Was there a missing alert or metric that would have surfaced this sooner?
- Was there a missing runbook that would have shortened the MTTR?
- Did the fix introduce any new risk (regression, race condition, performance)?
- Should this fix be backported to other versions or services?
- Was the root cause documented, or only the symptom fixed?
