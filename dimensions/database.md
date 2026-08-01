# Dimension: Database

## What to ask

- Are mutations wrapped in a transaction where needed?
- What is the isolation level? Is it appropriate for the use case?
- Are there missing indexes on columns used in WHERE / JOIN / ORDER BY?
- Could any query cause a full table scan at scale?
- Is locking used? Could it cause deadlocks or contention under load?
- Is there a data migration? Is it safe to run while the service is live (zero-downtime)?
- Are soft deletes needed, or is hard delete acceptable?
- Is there a risk of duplicate records? Is a unique constraint in place?
- What happens to existing data when new columns or constraints are added?

## ACID checks

- **Atomicity**: does a failure mid-operation leave partial state in the DB?
- **Consistency**: are all DB constraints in place to enforce business rules (NOT NULL, UNIQUE, FK, CHECK)?
- **Isolation**: what is the actual isolation level? Is it sufficient for the read/write pattern (e.g. READ COMMITTED vs REPEATABLE READ vs SERIALIZABLE)?
- **Durability**: is replication/WAL sufficient for the criticality of this data? Is synchronous replication needed?

## Scale checks

- What is the expected row count in 6 months? Does the schema hold?
- Are large result sets paginated or streamed, or loaded fully into memory?
- Could connection pool exhaustion occur under peak load?
- Are query-level timeouts set (separate from application-level timeouts)?

## Data compliance

- Does this table store PII? Is the retention policy defined?
- Are GDPR deletion requirements handled (soft delete vs hard delete)?
