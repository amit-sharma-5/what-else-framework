# Architectural Review: What-Else Framework — v2

**Date:** 2026-08-01
**Reviewer:** Architect (self-review — framework applied to itself)
**Status:** draft
**Previous:** 2026-08-01-framework-self-review.md
**Dimensions applied:** reliability, security, operations, data-integrity, testing-strategy

---

## What Changed

| Finding | Previous status | This run |
|---------|----------------|----------|
| #1 Dimensions unpinned from `main` | Critical | Resolved — skill now fetches from `v1.0.0` |
| #2 EDR sensitive info, no guidance | Critical | Resolved — `SECURITY.md` added |
| #3 URL arguments accept arbitrary domains | Critical | Resolved — `--proto` renamed to `--contract`, domain restriction documented |

---

## Summary

All three critical findings from v1 are resolved. The framework is now safe to share with the team. However, the fixes introduced three new issues worth addressing before wider adoption, and the seven debt items from v1 remain open. The most important new finding is a version coherence gap: the skill is at `v1.1.0` but fetches dimensions from `v1.0.0`, with no mechanism for users to know which version of either they have installed.

---

## What Else?

### Critical — fix now

*None. All previous criticals resolved.*

---

### Debt — newly introduced by fixes

**N1. Skill has no version identifier**

The installed skill file (`~/.claude/commands/what-else-review.md`) has no version header. A user running `/what-else-review` cannot tell if they have `v1.0.0` or `v1.1.0`. When a bug is reported, there is no way to know what version caused it.

*Fix:* Add a version line to the skill file header: `**Skill version:** v1.1.0`. Update on every release.

---

**N2. Skill version and dimension version are decoupled with no explanation**

The skill is at `v1.1.0` but fetches dimensions from `v1.0.0`. This is intentional (skill fixes shouldn't force a dimension upgrade) but undocumented. Users reading the skill file will be confused by the mismatch.

*Fix:* Add a comment in the skill under Step 3: `# Dimensions pinned to v1.0.0. To upgrade dimensions, update this tag and re-run affected EDRs.`

---

**N3. Trusted domains in `--contract` are hardcoded with no extension mechanism**

`SECURITY.md` lists `github.com` and `gitlab.com` as trusted. Teams using self-hosted VCS (Bitbucket Server, Gitea, Azure DevOps) have no way to add their domain without editing the skill file directly — which breaks on next `curl` upgrade.

*Fix:* Add `--trusted-domain <hostname>` argument to the skill, or document that teams should fork the skill and add their VCS hostname to the trusted list before installing.

---

**N4. `--proto` renamed to `--contract` with no migration note**

Anyone who already had `--proto` in a script or prompt will get a silent failure (unrecognised argument, no EDR produced). Early stage so few users affected, but worth documenting.

*Fix:* Add a one-line note in the skill: `Note: --proto was renamed to --contract in v1.1.0.`

---

### Debt — carried over from v1 (unchanged)

**#4 No governance model for dimensions**
Teams will want to add questions. No process for proposing changes or preventing bloat.
*Fix:* Add `CONTRIBUTING.md` — two tiers: core (PR required) vs local extension.

---

**#5 EDR staleness undefined**
A resolved finding in code has no way to be marked resolved in the EDR without a full re-run.
*Fix:* Add `[resolved YYYY-MM-DD, PR #N]` convention to the EDR template.

---

**#6 RFC and ADR stages underspecified — triggers undefined**
The framework lists these stages but provides no trigger definition or output format.
*Fix:* Define triggers before building RFC/ADR skills.

---

**#7 No feedback loop — EDR findings have no outcome tracking**
No mechanism to know whether critical findings were acted on.
*Fix:* Add `## Outcome` section to EDR template.

---

**#8 Dimension selection not visible in EDR output**
Which dimensions were applied is never shown in the EDR.
*Fix:* Log `Dimensions applied: ...` in the EDR header.

---

**#9 `--services` filter silently drops services**
Typo in a service name drops its entire review with no warning.
*Fix:* Print `Skipped: <name> (not in --services). Intentional?`

---

**#10 No self-referential example EDR**
`examples/` is empty. The framework's own design decisions are undocumented.
*Fix:* Write one EDR for the framework itself as the canonical example.

---

### Accepted — carried over

- AI hallucination on findings — mitigated by engineer review before commit
- Single maintainer — acceptable at current stage
- No automated test for dimensions — acceptable at MVP
- Framework is generic by design — team-specific dimensions belong in local extensions

---

## Decisions Made This Run

| Decision | Rationale |
|----------|-----------|
| Renamed `--proto` to `--contract` | More generic — applies to OpenAPI, AsyncAPI, not just proto |
| Skill fetches dimensions from `v1.0.0`, not `v1.1.0` | Skill fixes (security, arg rename) shouldn't force a dimension re-evaluation on existing EDRs |
| `SECURITY.md` as top-level file | GitHub renders it prominently and links it from the repo security tab automatically |

---

## Next Actions

| Finding | Action | Priority |
|---------|--------|----------|
| N1 Skill version identifier | Add version header to skill file | High |
| N2 Version decoupling undocumented | Add comment to skill Step 3 | High |
| N3 Trusted domain not extensible | Add `--trusted-domain` arg or fork guidance | Medium |
| N4 `--proto` rename undocumented | Add migration note to skill | Low |
| #5 EDR staleness | Add `resolved` convention to template | Medium |
| #7 Feedback loop | Add `Outcome` section to EDR template | Medium |
| #8 Dimension visibility | Log applied dimensions in EDR header | Low |
| #9 `--services` silent filter | Add warning to skill | Low |
| #4 Governance | Add `CONTRIBUTING.md` | Medium |
| #10 Self-referential example | Write EDR for framework in `examples/` | Low |
| #6 RFC/ADR triggers | Define before building skills | Medium |
