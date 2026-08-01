# Dimension: Reliability

## What to ask

- Are timeouts set on all outbound calls (HTTP, gRPC, DB, cache)?
- Is there retry logic? Is it safe (idempotent operations only)?
- Is there a circuit breaker for downstream dependencies?
- What happens if a downstream service is slow (not down, just slow)?
- Is there graceful degradation — does the feature partially work if a dependency fails?
- What is the blast radius if this feature fails? Does it affect unrelated flows?
- Is the feature safe to deploy incrementally (feature flag, canary)?

## Specific to gRPC / internal services

- Are deadlines propagated from the HTTP request into the gRPC call?
- If the gRPC service is unavailable, does the HTTP endpoint return a useful error or hang?
- Is there a fallback if the gRPC response is partial or malformed?
