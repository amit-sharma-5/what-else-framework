# Dimension: Performance

## What to ask

- Is caching applicable? If so, what is the invalidation strategy?
- Are large collections paginated or streamed?
- Are N+1 query patterns present (loop calling DB or downstream per item)?
- Are heavy operations (file I/O, serialization, external calls) done synchronously on the hot path?
- What is the expected throughput? Has it been load-tested or estimated?
- Are there unbounded operations — loops or queries with no size limit?
- Is there resource cleanup (connections, file handles, goroutines) after use?
