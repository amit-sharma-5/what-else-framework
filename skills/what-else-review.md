# Skill: What-Else-Review

## Purpose

Run a structured engineering review on a completed feature or bug fix.
Produce an Engineering Decision Record (EDR) that captures what was considered, what was missed, and what trade-offs were accepted.

This is not a code review. Do not comment on formatting, style, or obvious bugs.
Focus only on: what else should have been considered?

---

## How to invoke

Provide any combination of:
1. A git diff (`git diff main..your-branch`)
2. Relevant proto/interface definitions for cross-service boundaries
3. A short description of what was built and why

Example:
```
/what-else-review

Feature: HTTP endpoint in service-a backed by gRPC call to service-b

Diff:
<paste diff here>

gRPC contract:
<paste proto or interface here>
```

---

## Instructions for the AI

### Step 1 — Understand

Read all provided input. Identify:
- What was built (feature, bug fix, refactor)
- Technologies used: HTTP, gRPC, database, events/Kafka, cache, cron, etc.
- Cross-service boundaries present

### Step 2 — Select dimensions

Based on detected technologies, select the relevant review dimensions from:
- `api.md` — if HTTP endpoints are present
- `database.md` — if DB queries or migrations are present
- `events.md` — if Kafka/messaging is present
- `observability.md` — always include
- `reliability.md` — always include
- `security.md` — always include
- `performance.md` — if data retrieval or heavy operations are present
- `concurrency.md` — if shared state, locking, or parallel operations are present
- `operations.md` — if deployment, cron, or rollback concerns exist

Skip dimensions that clearly do not apply. Do not generate noise.

### Step 3 — Ask "What Else?"

For each selected dimension, review the input against the dimension's questions.
Identify gaps: things that were not addressed in the diff or context provided.

Classify each finding:
- **Critical** — missing something that will likely cause an incident, data loss, or security issue in production
- **Debt** — missing something that should be addressed but is not immediately dangerous
- **Accepted** — a known trade-off that is reasonable given the context

### Step 4 — Generate EDR

Fill in the EDR template (`templates/engineering-decision-record.md`) with:
- What was built (from the input)
- Findings organized by classification
- Cross-service dependencies table if a boundary was detected
- Observability section based on what is or isn't present in the diff

Output the completed EDR as markdown.
Suggest a filename: `edrs/YYYY-MM-DD-short-feature-name.md`

---

## Output format

- Produce the EDR as a markdown file ready to commit
- Lead with a one-paragraph summary of the review
- Findings should be actionable, not vague ("add a timeout on the gRPC call in service-a", not "consider timeouts")
- Keep the full EDR under 2 pages
