# Dimension: Concurrency

## What to ask

- Is there shared mutable state accessible from multiple goroutines/threads?
- Are there race conditions on read-modify-write operations?
- Is optimistic or pessimistic locking used where needed?
- Could concurrent requests create duplicate records?
- Are there potential deadlocks (multiple locks acquired in inconsistent order)?
- Are background workers or async operations bounded (max concurrency, queue depth)?
- Are there atomicity requirements that span multiple operations or services?
