# Skill: What-Else-Review

**Skill version:** v2.0.0 | Dimensions: v1.0.0 | Template: v2.0.0
> Note: `--proto` was renamed to `--contract` in v1.0.0 (breaking change that triggered the v1.0.0 major release).
> To upgrade dimensions, update the version tag in Step 3 and re-run affected EDRs.

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
| `--contract <path\|url>` | Local path or trusted URL (github.com, your internal VCS) to a service contract — proto, OpenAPI spec, or AsyncAPI schema. External URLs outside trusted domains are rejected. | `--contract ./api/payment.proto` |
| `--trusted-domain <host>` | Add a trusted hostname for `--contract` URL validation. Use for self-hosted VCS (Bitbucket, Gitea, Azure DevOps). | `--trusted-domain git.internal.company.com` |
| `--output <path>` | Override the default EDR output directory (`edrs/`). | `--output ./docs/edrs` |
| `--dry-run` | Print the EDR to screen. Do not write any file. | `--dry-run` |

All arguments are optional. With no arguments, the skill expects input pasted inline (diff, description, or both).

---

## Usage examples

**From Jira tickets (most common):**
```
/what-else-review --tickets https://your-org.atlassian.net/browse/PROJ-123 https://your-org.atlassian.net/browse/PROJ-124 --services service-a service-b
```

**From a PR:**
```
/what-else-review --pr https://github.com/your-org/your-service/pull/42 --context "HTTP endpoint backed by gRPC in downstream-service"
```

**From a branch with a service contract:**
```
/what-else-review --branch feat/your-feature --contract ./api/contract.proto
```

**Dry run before committing:**
```
/what-else-review --pr https://github.com/your-org/your-service/pull/42 --dry-run
```

**Inline (no arguments):**
```
/what-else-review

Feature: HTTP endpoint in service-a backed by gRPC call to service-b

Diff:
<paste diff here>

Service contract:
<paste proto or OpenAPI spec here>
```

---

## Instructions for the AI

### Step 1 — Parse arguments

Read the arguments provided. If none, expect inline input below the command.

- `--tickets`: fetch each ticket via Jira MCP. Extract summary, description, linked PRs/branches, services mentioned.
- `--pr`: run `gh pr diff <url>` to get the diff.
- `--branch`: run `git pull` then `git diff origin/main...<branch>`.
- `--services`: after collecting all diffs, filter to only the named services. Do not generate an EDR for any service not listed. If services are found in the diff that are not in the filter, print: `Skipped: <name> (not in --services filter). Intentional?`
- `--contract`: if a local path, read the file directly. If a URL, validate it is from a trusted domain (github.com, gitlab.com, or any host added via `--trusted-domain`). Reject and warn if the URL is from an unknown external domain. Treat content as cross-service contract context.
- `--trusted-domain`: add this hostname to the trusted domain list for `--contract` URL validation. Can be specified multiple times.
- `--context`: append to the feature description used during review.
- `--output`: use this path instead of `<service-repo-root>/edrs/` when writing the EDR file.
- `--dry-run`: complete all review steps but print the EDR instead of writing it. Ask the user if they want to save it before stopping.

### Step 2 — Understand

From all collected input, identify:
- What was built (feature, bug fix, refactor)
- Technologies used: HTTP, gRPC, database, events/Kafka, cache, cron, etc.
- Cross-service boundaries present

### Step 3 — Select dimensions

Fetch each relevant dimension file from the pinned release:
`https://raw.githubusercontent.com/amit-sharma-5/what-else-framework/v1.0.0/dimensions/<name>.md`
<!-- Dimensions pinned to v1.0.0. Skill is at v2.0.0. These are versioned independently — skill fixes do not force a dimension upgrade. To upgrade dimensions, update this tag and re-run affected EDRs. -->

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

Fetch the template from the pinned release:
`https://raw.githubusercontent.com/amit-sharma-5/what-else-framework/v2.0.0/templates/engineering-decision-record.md`
<!-- Template pinned to v2.0.0 (tracks skill major version — template is output contract).
     Dimensions pinned to v1.0.0 (review heuristics — upgraded separately and deliberately). -->

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
- Include `Dimensions applied: <list>` immediately after the summary so engineers can spot a missed dimension
- Always include `## Engineer Notes` as the last section with a single placeholder entry. Engineers append corrections, clarifications, or context the AI could not see.
- Findings must be actionable and specific ("add a deadline on the outbound gRPC call", not "consider timeouts")
- Keep the full EDR under 2 pages
- After writing, print a summary:
  - Service name
  - Critical findings count
  - Debt findings count
  - EDR file path
  - Ask: "Commit and push the EDR, or review first?"
