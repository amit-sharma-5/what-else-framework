# Dimension: Data Integrity

Covers correctness of the data model across the system — not DB mechanics (see database.md)
but whether invariants and consistency are properly owned and enforced.

## What to ask

- Are domain invariants enforced at the right layer — DB constraint, service layer, or both?
- Across service boundaries, which service is the source of truth for this data?
- Is there data duplication across services? If so, how is it kept in sync?
- What is the consistency model — strong, eventual, causal? Is it appropriate for the use case?
- Are there distributed writes across multiple services or DBs?
  - If yes: is a saga pattern with compensation logic needed?
  - If a step fails mid-saga, is the system left in a consistent state?
- Is there a risk of ghost records — data that exists in one service but not another after a failure?
- Are there business rules that span multiple entities? Are they enforced atomically?
- Is the data model additive (new fields, new tables) or breaking (type changes, field removal)?
  - Breaking changes across service boundaries need a migration plan, not just a code change.

## Cross-service checks

- If service A writes and service B reads the same domain entity, is the contract explicit (proto, schema registry, OpenAPI)?
- Is there a canonical event or API that both services agree on, or is each syncing its own copy?
- What happens to service B's data if service A's write is rolled back?
