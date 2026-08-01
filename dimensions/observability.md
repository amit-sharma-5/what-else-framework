# Dimension: Observability

## What to ask

- Are errors logged with enough context to diagnose in production?
- Is a correlation/trace ID present in logs and propagated across service boundaries?
- Are key business events logged (not just errors)?
- Are metrics emitted for: request count, latency, error rate?
- Are slow paths or heavy operations traced?
- Is there a dashboard for this feature?
- Are alerts defined for error spikes or latency regression?
- Can you tell from logs alone why a request failed?

## Cross-service checks

- Does trace context flow from HTTP → gRPC → DB?
- Are downstream call latencies measured separately from total request latency?
- Is the upstream caller identifiable in logs of the downstream service?
