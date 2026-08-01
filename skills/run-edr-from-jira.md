# Prompt: Generate EDR from Jira Tickets

Use this prompt in any Claude Code session that has Jira MCP access.
Paste the block below and fill in the ticket URLs.

---

## Prompt

```
I want to run a What-Else-Review on a completed feature and generate Engineering Decision Records (EDRs).

## Jira tickets
- <TICKET_URL_1>
- <TICKET_URL_2>
- <TICKET_URL_3>

## What to do

1. Fetch each Jira ticket using the Jira MCP tool. From each ticket extract:
   - Summary (what was built)
   - Description (business context)
   - Linked PRs or branches
   - Services / components mentioned

2. For each linked PR or branch, get the diff:
   git -C <repo-path> pull
   git -C <repo-path> diff origin/main...<branch>
   Or via GitHub CLI: gh pr diff <PR_URL>

3. Identify which services are involved. We own 2 of the 3 services — skip the schema registry or any service we do not own.

4. For each owned service, run the What-Else-Review skill.
   Fetch the skill instructions and supporting files from the GitHub repo:

   Skill:      https://raw.githubusercontent.com/amit-sharma-5/what-else-review/main/skills/what-else-review.md
   Template:   https://raw.githubusercontent.com/amit-sharma-5/what-else-review/main/templates/engineering-decision-record.md
   Dimensions: https://raw.githubusercontent.com/amit-sharma-5/what-else-review/main/dimensions/

   Available dimension files: api.md, database.md, events.md, observability.md,
   reliability.md, security.md, performance.md, concurrency.md, operations.md

   Feed the skill:
   - The diff for that service
   - Any cross-service contracts (proto files, OpenAPI specs) found in the diff or linked tickets
   - The feature summary and business context from the Jira tickets

5. Generate one EDR per owned service using the template. Save each to:
   <service-repo-root>/edrs/YYYY-MM-DD-HHmm-<short-feature-name>.md

6. Print a summary:
   - Service name
   - Critical findings (fix now)
   - Debt findings (fix later)
   - EDR file path
   - Ask: "Commit and push the EDR, or review first?"
```
