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

2. For each linked PR or branch, get the diff. If the repo is local, run:
   git -C <repo-path> pull
   git -C <repo-path> diff origin/main...<branch>
   If not local, fetch via GitHub CLI: gh pr diff <PR_URL>

3. Identify which services are involved. We own 2 of the 3 services — skip the schema registry or any service we do not own.

4. For each owned service with changes, run the What-Else-Review skill:

   Skill instructions are at: ~/Documents/project/what-else/what-else-review/skills/what-else-review.md
   Dimension files are at:    ~/Documents/project/what-else/what-else-review/dimensions/
   EDR template is at:        ~/Documents/project/what-else/what-else-review/templates/engineering-decision-record.md

   Feed the skill:
   - The diff for that service
   - Any cross-service contracts (proto files, OpenAPI specs) found in the diff or linked tickets
   - The feature summary and business context from the Jira tickets

5. Generate one EDR per owned service. Save each EDR to:
   <service-repo-root>/edrs/YYYY-MM-DD-<short-feature-name>.md

6. After writing both EDRs, print a brief summary:
   - Service name
   - Critical findings (fix now)
   - Debt findings (fix later)
   - EDR file path
```

---

## Notes

- Replace `<TICKET_URL_N>` with actual URLs before running.
- If repos are not local, clone them first or run from the directory where they exist.
- The skill will auto-select relevant dimensions based on detected technologies.
- Do not generate an EDR for services you do not own (e.g. schema registry).
