# Dimension: API

## What to ask

- Is the endpoint authenticated? Is authorization checked at the right level?
- Are inputs validated? What happens with missing, null, or malformed fields?
- Are error responses consistent and meaningful (not leaking internals)?
- Is the contract versioned? What breaks if the schema changes?
- Are there missing business scenarios — what happens on partial success?
- Is idempotency needed? (POST that creates — can it be called twice safely?)
- Is pagination required for list endpoints?
- Are there rate limits or throttling considerations?
- Is the response shape stable for consumers downstream?

## Cross-service boundary checks

- Does auth context propagate from HTTP into the downstream call (gRPC, MQ, etc.)?
- Is trace context (correlation ID) forwarded?
- Does the HTTP layer handle downstream failures gracefully (timeout, 503, partial response)?

## Additional checks

- Is the HTTP method semantically correct? (GET is safe and idempotent, POST is not)
- Is API versioning in place? What breaks for existing consumers if the schema changes?
- Are response caching headers set correctly (Cache-Control, ETag)?
- Is there a consumer-driven contract test for shared APIs?
- Is rate limiting applied — both for performance and to prevent abuse?
- Are CORS headers configured if this endpoint is browser-facing?
