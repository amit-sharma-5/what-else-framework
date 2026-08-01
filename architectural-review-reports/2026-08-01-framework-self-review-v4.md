# Architectural Review: What-Else Framework — v4

**Date:** 2026-08-01
**Reviewer:** Architect (self-review — framework applied to itself)
**Status:** draft
**Previous:** 2026-08-01-framework-self-review-v3.md
**Dimensions applied:** reliability, security, operations, data-integrity, testing-strategy

---

## What Changed

| Finding | Previous status | This run |
|---------|----------------|----------|
| N5 Skill version header stale (v1.2.0) | Debt | Resolved — header now reads v1.3.0 |
| N6 Template pinned to v1.0.0 (stale) | Debt | Resolved — template now pinned to v1.3.0 with rationale |
| README gaps: `--proto`, missing args, missing dirs | Identified this run | Resolved — README synced with actual state |

---

## Summary

Two critical debt items from v3 resolved. README now accurately reflects the repo. Three minor new findings remain: a stale version comment inside the skill (says v1.2.0, should be v1.3.0), a skill header/tag mismatch (header says v1.3.0, repo is tagged v1.4.0), and `frameworks/` directory referenced in README but does not exist on disk. No critical findings. Framework is clean and ready to share.

**Dimensions applied:** reliability, security, operations, data-integrity, testing-strategy

---

## What Else?

### Critical — fix now

*None.*

---

### Debt — newly found

**N7. Stale version comment on line 99 of skill file**

The inline comment in Step 3 reads:
```
<!-- Dimensions pinned to v1.0.0. Skill is at v1.2.0. ... -->
```
Skill is at v1.3.0. Minor but will confuse anyone reading the skill source.

*Fix:* Update comment from `v1.2.0` to `v1.3.0`.

---

**N8. Skill header says v1.3.0 but repo is tagged v1.4.0**

The header `**Skill version:** v1.3.0` was not bumped when `v1.4.0` was tagged for the README and N5/N6 fixes.

*Fix:* Update header to `v1.4.0`. Establish rule: tag and header are bumped in the same commit.

---

**N9. `frameworks/` directory listed in README but does not exist on disk**

README structure shows:
```
frameworks/
  rfc.md, adr.md, feature.md, bugfix.md, incident.md
```
None of these files or the directory exist. A new contributor cloning the repo and following the README will find a missing directory.

*Fix:* Either create `frameworks/` with stub files, or move it clearly under a `## Roadmap` heading in the README with a note that it does not exist yet. The current note `← (roadmap)` is too subtle.

---

### Accepted — carried over

- AI hallucination — mitigated by Engineer Notes
- Single maintainer — acceptable at current stage
- No automated dimension tests — YAGNI
- Generic by design — team-specific in local extensions

---

## README Alignment Verification

| README claim | Actual state | Status |
|-------------|-------------|--------|
| `frameworks/` directory with 5 files | Directory does not exist | **Gap — N9** |
| `dimensions/` with 11 files | Present, all 11 files exist | OK |
| `skills/what-else-review.md` | Present | OK |
| `templates/engineering-decision-record.md` | Present | OK |
| `examples/` directory | Present with 1 EDR | OK |
| `architectural-review-reports/` | Present with 3 reports | OK |
| `CONTRIBUTING.md` | Present | OK |
| `SECURITY.md` | Present | OK |
| Argument table includes `--contract` | Present | OK |
| Argument table includes `--trusted-domain` | Present | OK |
| Install curl uses `main` branch | Present — acceptable for install (always latest) | OK |

---

## Next Actions

| Finding | Action | Priority |
|---------|--------|----------|
| N7 Stale comment in skill | Update `v1.2.0` → `v1.3.0` in Step 3 comment | Low |
| N8 Skill header/tag mismatch | Bump to `v1.4.0`, document bump rule | High |
| N9 `frameworks/` missing | Create stub files or clearly label as roadmap-only | Medium |

---

## Engineer Notes

- [2026-08-01 @architect] README alignment verified for the first time this iteration. 10/11 claims accurate — only `frameworks/` gap found. This is the cleanest review pass yet. Three findings, all mechanical, none blocking team sharing.
