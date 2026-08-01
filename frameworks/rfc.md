# Framework: RFC — What Else before building?

**Trigger:** Author runs before sharing the RFC for team review.
**Output:** Annotated RFC with gaps and unanswered questions added inline.
**Skill:** `what-else-rfc` (roadmap — not yet built)

## What to ask

### Functional
- What requirements are missing from this design?
- What edge cases are not handled?
- What happens if Product changes this requirement in 3 months?

### Scale
- How does this behave at 10× current traffic?
- What is the bottleneck at scale?

### Data
- What is the migration path from current state?
- What is the rollback plan if the RFC is abandoned mid-implementation?
- What are the data retention implications?

### Events / Async
- Are there ordering requirements not addressed?
- What happens with duplicate events?

### Security
- Does this design introduce new trust boundaries?
- Is PII involved? Is that addressed?

### Operations
- What runbooks are needed?
- What metrics and alerts are implied but not specified?
- What is the on-call impact?

### Assumptions
- What assumptions does this RFC depend on that could be wrong?
- What would invalidate this RFC in 6 months?

### Alternatives
- What alternatives were not considered?
- Why is this approach better than the simplest possible solution?
