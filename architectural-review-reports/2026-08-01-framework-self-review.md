# Architectural Review: What-Else Framework

**Date:** 2026-08-01
**Reviewer:** Architect (self-review — framework applied to itself)
**Status:** draft
**Previous:** first run

---

## Summary

The framework is conceptually sound and the core idea is validated with real EDRs. The first skill (`what-else-review`) is functional. However, the framework has gaps that will cause real problems as adoption grows: no versioning contract for dimension files, no governance model, EDR staleness undefined, and scope ambiguity between stages. These are solvable iteratively.

---

## What Else?

### Critical — fix now

**1. Dimensions fetched at runtime from `main` — no version pinning**

The skill always pulls dimension files from the `main` branch. If a dimension changes, all past EDRs become unreproducible — re-running the skill on the same feature will produce different findings with no indication that the dimension changed.

*Fix:* Pin the skill to a tagged release. Release dimensions as versioned snapshots. Equivalent of a lockfile.

---

**2. EDR contains sensitive information with no access control guidance**

EDRs document failure modes, accepted security gaps, known limitations, and open questions — in plain markdown committed to service repos. In a public or broadly-accessible repo, EDRs become a roadmap for attackers.

*Fix:* Add explicit guidance: Critical findings must be resolved before committing an EDR to a public repo. Mark sensitive findings with `[SENSITIVE]`. Document that these must stay in private repos.

---

**3. Arguments accepting external URLs should restrict to trusted domains**

Any skill argument that accepts a URL (e.g. for fetching contracts or specs) should validate against known trusted domains — your internal VCS, github.com, known API registries. Accepting arbitrary external URLs sets a bad precedent and could escalate as the framework evolves toward automation.

*Fix:* Restrict URL arguments to local file paths or explicitly trusted domains. Document this restriction in the skill.

---

### Debt — fix later

**4. No governance model for dimensions**

As teams adopt the framework they will want to add questions, modify existing ones, or add team-specific dimensions. No process exists for proposing changes, reviewing universal vs team-specific applicability, or preventing dimension bloat.

*Fix:* Add `CONTRIBUTING.md` with two tiers — core dimensions (this repo, PR required) and custom dimensions (extend locally in your own repo).

---

**5. EDR staleness is undefined**

An EDR that says "circuit breaker: missing (debt)" is actively misleading if the circuit breaker was added in a later PR without a new EDR run. No convention exists for marking findings resolved outside of a full re-run.

*Fix:* Add a `resolved` status inline: `- [resolved 2026-09-01, PR #87] circuit breaker added`. Engineers update without a full re-run.

---

**6. RFC and ADR stages are underspecified — trigger is undefined**

The framework lists RFC and ADR as stages but provides no definition of when each applies, who triggers the review, or what the output artifact looks like. Without a clear trigger, engineers will skip these stages.

*Fix:* Define triggers before building the skills. RFC trigger: author runs before sharing for review. ADR trigger: decision made, not yet implemented. Output: annotated version of the original document, not a new file.

---

**7. No quality signal — EDRs have no feedback loop**

EDRs are written, committed, and forgotten. No mechanism exists to know whether critical findings were acted on, whether debt items were picked up, or whether the EDR was useful at all.

*Fix:* Add `## Outcome` section to the EDR template. Filled in after action is taken: `finding X → fixed in PR #N | deferred to ticket Y | accepted`. This is also the future data source for AI pattern analysis across EDRs.

---

**8. Dimension selection not visible in output**

The skill selects dimensions automatically but never shows which ones were selected. If the AI misses a dimension, the EDR silently skips it with no indication.

*Fix:* Log selected dimensions in the EDR header: `Dimensions applied: api, observability, reliability, security, testing-strategy`.

---

**9. Service filter produces no warning when silently dropping services**

When `--services` filters out services found in the diff, nothing is printed. A typo in a service name silently drops its entire review.

*Fix:* When services are filtered out, print: `Skipped: service-b (not in --services filter). Intentional?`

---

**10. No self-referential example**

The framework has no EDR for itself. Its own design decisions — why versioned EDRs over single-file, why dimensions are fetched at runtime, why the skill is AI-instruction not code — are undocumented. The canonical example in `examples/` should be an EDR for the framework itself.

*Fix:* Write one EDR for the framework. Use it as the `examples/` entry.

---

### Accepted — documented trade-offs

**AI hallucination on findings** — The skill can generate plausible but incorrect findings. Accepted because EDRs are reviewed by engineers before committing, and a false positive is less damaging than a missed critical finding.

**Single maintainer** — Bus-factor of 1 is acceptable at MVP stage. Revisit when the team formally adopts it.

**No automated test for dimensions** — Dimension files are not tested for coverage or consistency. Accepted at MVP. The real test is whether engineers find the questions useful.

**Framework is generic by design** — The framework intentionally has no language-specific or company-specific dimensions. Team-specific concerns belong in local extension dimensions, not in core. This is a deliberate boundary.

---

## Decisions Made

| Decision | Rationale |
|----------|-----------|
| Skills fetch dimensions from GitHub at runtime | No install step required. Trade-off: unpinned from `main` (see finding #1) |
| EDRs versioned by timestamp, not updated in place | Easier to iterate without overwrite anxiety during validation phase |
| Framework is AI-independent by design | If AI disappears, the questioning structure still works |
| `what-else-review` built first before RFC/ADR skills | Validate the core loop on the most concrete use case before expanding |

---

## Open Questions

- Should dimensions be versioned as GitHub releases or a separate `versions/` directory?
- Should the `Outcome` section be added now (before team rollout) or after first feedback cycle?
- Should custom team dimensions be a separate repo convention or a `--dimensions-path` argument to the skill?

---

## Next Actions

| Finding | Action | Priority |
|---------|--------|----------|
| #1 Unpinned dimensions | Add GitHub release tags, update skill URLs | High |
| #2 Sensitive EDR guidance | Add `SECURITY.md` or section in README | High |
| #3 URL restriction | Document in skill, add note to arguments table | High |
| #4 Governance | Add `CONTRIBUTING.md` | Medium |
| #5 EDR staleness | Add `resolved` status to template | Medium |
| #6 RFC/ADR triggers | Define in `what-else-framework.md` | Medium |
| #7 Feedback loop | Add `Outcome` section to EDR template | Medium |
| #8 Dimension visibility | Update skill output format | Low |
| #9 Service filter warning | Update skill instructions | Low |
| #10 Self-referential example | Write EDR for framework, add to `examples/` | Low |
