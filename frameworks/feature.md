# Framework: Feature — What Else after implementing?

**Trigger:** Feature complete — all PRs merged, deployed or ready to deploy.
**Output:** Engineering Decision Record (EDR) saved to `edrs/` in the service repo.
**Skill:** `/what-else-review` (implemented — see `skills/what-else-review.md`)

This framework is fully implemented. See the skill for full argument reference and usage examples.

## Quick reference

```
/what-else-review --pr <url>
/what-else-review --tickets <url>... --services <name>...
/what-else-review --branch <name> --dry-run
```

## What the skill checks

Dimensions selected automatically based on detected technologies:
- API, Database, Events, Observability, Reliability, Security,
  Performance, Concurrency, Operations, Data Integrity, Testing Strategy
