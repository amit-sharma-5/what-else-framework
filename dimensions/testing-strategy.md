# Dimension: Testing Strategy

Not about unit test coverage — that belongs in code review.
This dimension asks: are the right tests in place to catch failures that unit tests cannot?

## What to ask

- Is there a contract test for the API or event schema?
  - If the schema changes, will consumers know before they break in production?
- Is there an integration test that crosses the service boundary (HTTP → gRPC, service → DB)?
- Is there a smoke test that can be run immediately post-deploy to confirm the feature is alive?
- Is there a rollback test — how do you verify the rollback actually worked?
- Do the critical findings in this EDR have test coverage? If a gap was found, is a test the right fix?

## Cross-service checks

- If service A calls service B, is there a test that runs against a real (or contract-verified) instance of service B?
- Are consumer-driven contracts (e.g. Pact) in use, or is compatibility verified only manually?
- Is there a test for the failure path — what happens when service B is down or slow?

## Missing scenario checks

- Are the edge cases identified in this review covered by any test?
- Is there a test for the rollback / undo path if the feature has one?
- Are load or stress tests needed given the expected traffic?
