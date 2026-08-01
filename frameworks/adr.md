# Framework: ADR — What Else before deciding?

**Trigger:** Decision made but not yet implemented. Run before the ADR is merged.
**Output:** Gaps and open questions added to the ADR. Author resolves or explicitly accepts each before merging.
**Skill:** `what-else-adr` (roadmap — not yet built)

## What to ask

### The decision itself
- What problem does this decision solve?
- Is the problem well-defined, or is this a solution looking for a problem?
- What is the smallest decision that could be made here?

### Alternatives
- What alternatives were seriously considered?
- Why was each alternative rejected?
- What would it take to reverse this decision?

### Assumptions
- What must be true for this decision to be correct?
- What would make you revisit this ADR?

### Failure modes
- What happens if the chosen approach fails?
- What is the blast radius of a bad decision here?

### Operational impact
- Does this decision require new tooling, runbooks, or on-call procedures?
- Who else in the organisation is affected?

### Future
- Under what conditions does this decision become wrong?
- What metric or event should trigger a review of this ADR?
- What does good look like in 1 year if this decision was correct?
