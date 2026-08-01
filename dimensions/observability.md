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

## Logging quality

- Are logs structured (JSON / key-value)? No free-text log lines that can't be queried.
- Is log level used correctly? ERROR for actionable failures, WARN for unexpected-but-recoverable, INFO for key business events.
- Are errors logged with stack trace and enough context to reproduce?

## Sampling and cost

- Is trace sampling rate configured? 100% sampling in production is expensive at scale.
- Is there an SLO defined for this feature? If not, what metric would alert on it?
