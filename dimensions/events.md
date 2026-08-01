# Dimension: Events

## What to ask

- Is the consumer idempotent? What happens if the same message is delivered twice?
- Does message ordering matter? Is it guaranteed by the broker?
- What happens on consumer failure — is there retry logic?
- Is there a Dead Letter Queue (DLQ) for poison messages?
- Is the message schema versioned? What happens when it evolves?
- Are events published transactionally with the DB write (outbox pattern needed)?
- What is the retention policy? Is that enough for replay/recovery?
- Are there consumers downstream that are not owned by this team?

## Failure mode checks

- What happens if the broker is unavailable at publish time?
- What happens if the consumer is down for 1 hour? 1 day?
- Is there an alert on DLQ depth or consumer lag?
