# Skill: What-Else-Review

## Purpose

Run a structured engineering review on a completed feature or bug fix.
Produce an Engineering Decision Record (EDR) that captures what was considered, what was missed, and what trade-offs were accepted.

This is not a code review. Do not comment on formatting, style, or obvious bugs.
Focus only on: what else should have been considered?

---

## Arguments

| Argument | Description | Example |
|----------|-------------|---------|
| `--tickets <url>...` | One or more Jira ticket URLs. Fetch via Jira MCP. | `--tickets https://...` |
| `--pr <url>` | GitHub PR URL. Fetch diff via `gh pr diff <url>`. | `--pr https://github.com/...` |
| `--branch <name>` | Branch name. Run `git pull` then `git diff origin/main...<name>`. | `--branch feat/payment-api` |
| `--services <name>...` | Restrict review to named services only. Skip all others. | `--services service-a service-b` |
| `--context "<text>"` | Free-text context the diff cannot show: cross-service contracts, business rules, constraints. | `--context "HTTP calls gRPC in service-b"` |
| `--proto <path\|url>` | Path or URL to a proto/OpenAPI spec for cross-service boundary review. | `--proto ./api/payment.proto` |
| `--output <path>` | Override the default EDR output directory (`edrs/`). | `--output ./docs/edrs` |
| `--dry-run` | Print the EDR to screen. Do not write any file. | `--dry-run` |

All arguments are optional. With no arguments, the skill expects input pasted inline (diff, description, or both).

---

## Usage examples

**From Jira tickets (most common):**
```
/what-else-review --tickets https://hellofresh.atlassian.net/browse/PAM-5163 https://hellofresh.atlassian.net/browse/PAM-5164 --services service-a service-b
```

**From a PR:**
```
/what-else-review --pr https://github.com/org/service-a/pull/42 --context "HTTP endpoint backed by gRPC in billing-service"
```

**From a branch with proto:**
```
/what-else-review --branch feat/payment-api --proto ./api/payment.proto
```

**Dry run before committing:**
```
/what-else-review --pr https://github.com/org/service-a/pull/42 --dry-run
```

**Inline (no arguments):**
```
/what-else-review

Feature: HTTP endpoint in service-a backed by gRPC call to service-b

Diff:
<paste diff here>

gRPC contract:
<paste proto here>
```

---

## Instructions for the AI

### Step 1 — Parse arguments

Read the arguments provided. If none, expect inline input below the command.

- `--tickets`: fetch each ticket via Jira MCP. Extract summary, description, linked PRs/branches, services mentioned.
- `--pr`: run `gh pr diff <url>` to get the diff.
- `--branch`: run `git pull` then `git diff origin/main...<branch>`.
- `--services`: after collecting all diffs, filter to only the named services. Do not generate an EDR for any service not listed.
- `--proto`: read the file or fetch the URL and treat as cross-service contract context.
- `--context`: append to the feature description used during review.
- `--output`: use this path instead of `<service-repo-root>/edrs/` when writing the EDR file.
- `--dry-run`: complete all review steps but print the EDR instead of writing it. Ask the user if they want to save it before stopping.

### Step 2 — Understand

From all collected input, identify:
- What was built (feature, bug fix, refactor)
- Technologies used: HTTP, gRPC, database, events/Kafka, cache, cron, etc.
- Cross-service boundaries present

### Step 3 — Select dimensions

Fetch each relevant dimension file from:
`https://raw.githubusercontent.com/amit-sharma-5/what-else-framework/main/dimensions/<name>.md`

Select based on detected technologies:
- `api.md` — if HTTP endpoints are present
- `database.md` — if DB queries or migrations are present
- `events.md` — if Kafka/messaging is present
- `observability.md` — always include
- `reliability.md` — always include
- `security.md` — always include
- `performance.md` — if data retrieval or heavy operations are present
- `concurrency.md` — if shared state, locking, or parallel operations are present
- `operations.md` — if deployment, cron, or rollback concerns exist
- `data-integrity.md` — if writes span multiple services or distributed consistency is involved
- `testing-strategy.md` — always include

Skip dimensions that clearly do not apply. Do not generate noise.

### Step 4 — Ask "What Else?"

For each selected dimension, review the input against the dimension's questions.
Identify gaps: things that were not addressed in the diff or context provided.

Classify each finding:
- **Critical** — missing something that will likely cause an incident, data loss, or security issue in production
- **Debt** — missing something that should be addressed but is not immediately dangerous
- **Accepted** — a known trade-off that is reasonable given the context

### Step 5 — Generate EDR

Fetch the template from:
`https://raw.githubusercontent.com/amit-sharma-5/what-else-framework/main/templates/engineering-decision-record.md`

Fill in:
- What was built (from the input)
- Findings organized by classification
- Cross-service dependencies table if a boundary was detected
- Observability section based on what is or isn't present in the diff

Each run creates a new versioned file:
`<output-dir>/YYYY-MM-DD-HHmm-<short-feature-name>.md`

Example: `edrs/2026-08-01-1430-payment-http-grpc.md`

If a previous EDR exists for the same feature in the output directory, read it first and include a `## What Changed` section at the top of the new file summarising what was resolved since the last run and what is new. Link to the previous version with `Previous: <filename>`.

If `--dry-run` is set, print the EDR and ask: "Save to file?"

---

## Output format

- Produce the EDR as a markdown file ready to commit
- Lead with a one-paragraph summary of the review
- Findings must be actionable and specific ("add a timeout on the gRPC call in service-a", not "consider timeouts")
- Keep the full EDR under 2 pages
- After writing, print a summary:
  - Service name
  - Critical findings count
  - Debt findings count
  - EDR file path
  - Ask: "Commit and push the EDR, or review first?"
