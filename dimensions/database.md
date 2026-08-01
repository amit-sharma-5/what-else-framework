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

## Scale checks

- What is the expected row count in 6 months? Does the schema hold?
- Are large result sets paginated or streamed, or loaded fully into memory?
