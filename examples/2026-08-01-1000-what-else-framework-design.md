# Engineering Decision Record

**Feature:** What-Else Framework — initial design
**Date:** 2026-08-01 10:00
**Author:** amit-sharma-5
**Status:** accepted
**Related PRs:** initial commit
**Services touched:** N/A — framework tooling, no production service
**Previous:** first run

---

## Feature Summary

Designed and built the What-Else Framework: a contextual questioning framework that improves engineering decision quality at every stage of the development lifecycle. The first implemented skill (`what-else-review`) reviews completed features and produces Engineering Decision Records (EDRs).

## Business Context

Post-implementation engineering context (assumptions, trade-offs, failure modes) is routinely lost. Months after a feature ships, teams cannot answer why decisions were made. This framework makes capturing that context lightweight and AI-assisted rather than relying on engineer discipline.

## Assumptions

- Engineers will run the skill manually per feature — no automated trigger at MVP
- The AI (Claude Code) is available in the engineer's workflow
- Markdown files committed to service repos are a sufficient knowledge store at this stage
- The framework is generic enough to apply across teams and tech stacks without customisation

## Decisions Made

| Decision | Rationale |
|----------|-----------|
| Skill as markdown instruction file, not code | No install, no runtime, works in any Claude Code session |
| EDRs versioned by timestamp, not updated in place | Engineers can iterate (fix → re-run) without overwrite risk during validation |
| Dimensions fetched at runtime from pinned GitHub tag | No local install required; pinned tag ensures reproducibility |
| Framework-first, skill-second | The dimensions and template are the core value; the skill is the delivery mechanism |
| `what-else-review` built before RFC/ADR skills | Validate the loop on the most concrete use case before expanding scope |

## Alternatives Considered

| Alternative | Why rejected |
|-------------|-------------|
| CI/CD integration, auto-run on merge | Too much friction for MVP; adoption gap before value is proven |
| Central EDR repository (not per-service) | EDRs belong next to the code; per-service makes them discoverable and git-blame-able |
| Single EDR file updated in place | Harder to iterate during validation; timestamp versioning is lower risk at this stage |
| AI writes the EDR from scratch | Framework value is in challenging documentation, not generating it |

## Trade-offs Accepted

- Manual trigger means inconsistent adoption — acceptable until value is proven
- Single maintainer (bus factor 1) — acceptable at MVP
- No automated test for dimension coverage — real test is engineer usefulness

## What Else?

### Critical — fix now

- [resolved 2026-08-01] Dimensions unpinned from `main` — now pinned to `v1.0.0`
- [resolved 2026-08-01] EDR sensitive content guidance missing — `SECURITY.md` added
- [resolved 2026-08-01] URL arguments accepted arbitrary domains — `--contract` now domain-restricted

### Debt — fix later

- [resolved 2026-08-01] RFC/ADR triggers undefined — triggers and output format now defined in `what-else-framework.md`
- [resolved 2026-08-01] No governance model — `CONTRIBUTING.md` added with two-tier dimension model
- [resolved 2026-08-01] EDR staleness undefined — `[resolved]` convention added to template
- [resolved 2026-08-01] No feedback loop — `## Outcome` section added to EDR template
- [resolved 2026-08-01] Dimension selection invisible — `Dimensions applied:` added to EDR output format
- [resolved 2026-08-01] `--services` filter silent — warning added to skill
- [resolved 2026-08-01] Skill has no version identifier — `**Skill version:**` header added
- [resolved 2026-08-01] `--trusted-domain` not extensible — argument added to skill

### Accepted — documented trade-off

- AI hallucination on findings — mitigated by engineer review before commit
- No automated tests for dimensions — YAGNI at this stage
- Generic by design — language/company-specific questions belong in local extensions

## Known Limitations

- Framework scope is currently limited to `what-else-review` (feature/PR). RFC, ADR, and incident skills are defined but not yet built.
- EDR versioning strategy (timestamp files) will need consolidation to single-file once the workflow is validated across the team.

## Cross-Service Dependencies

N/A — no production services. Runtime dependency on GitHub raw content API for dimension files.

## Observability

- Logs: N/A
- Metrics: N/A
- Traces: N/A
- Alerts: N/A

## Future Improvements

- `what-else-rfc`, `what-else-adr`, `what-else-incident` skills
- `--dimensions-path` argument for local team extensions
- Searchable EDR knowledge base across services
- AI reads all EDRs to surface recurring gaps across features

## Open Questions

- When should EDR versioning move from timestamp-per-run to single-file-with-history?
- Should the dimension upgrade path be `--dimensions-version` argument or manual tag edit?

## Outcome

- All critical findings resolved on 2026-08-01 before team sharing
- All debt items resolved on 2026-08-01 as part of pre-share cleanup
- Framework shared with team after first real EDRs validated on PAM-5163/5164/5165
