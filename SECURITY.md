# Security Guidance

## EDR sensitivity

Engineering Decision Records document failure modes, accepted security gaps, known limitations, and open questions. This information can be sensitive.

### Before committing an EDR

- **Critical findings must be resolved** before committing an EDR to a public repository. An unresolved critical finding in a public EDR is a disclosed vulnerability.
- **Mark sensitive findings** with `[SENSITIVE]` in the finding text. These include: accepted auth gaps, unmonitored failure paths, known data exposure risks.
- **Private repos only** for EDRs containing `[SENSITIVE]` findings that remain unresolved.

### What's safe to commit publicly

- Resolved findings (fix already shipped)
- Accepted trade-offs with no active exploit path
- Debt items that are non-security (missing metrics, pagination, caching)

---

## URL arguments

Skill arguments that accept URLs (`--contract`, `--tickets`, `--pr`) only fetch from trusted domains:

- `github.com`
- `gitlab.com`
- Your internal VCS hostname

URLs from unknown external domains are rejected. If you need to add a trusted domain, pass the content as a local file instead.

---

## Dimension files

Dimensions are fetched from a pinned release tag (e.g. `v1.0.0`), not from `main`. This ensures:

- EDRs are reproducible — re-running the skill on the same feature uses the same questions
- A dimension change cannot silently alter the review of a past feature
- Upgrading is a deliberate act (update the version in the skill, re-run affected EDRs)

To upgrade to a new dimension version, update the version tag in `skills/what-else-review.md` and re-run the skill on active features.
