# Contributing

## Two tiers of dimensions

### Core dimensions (this repo)

Core dimensions live in `dimensions/` and apply broadly across most engineering contexts.

To propose a change or addition:
1. Open a PR with the change and a short rationale
2. The question must be universally applicable — not specific to one team, language, or company
3. If the question only matters in your context, use a local extension instead (see below)

Good core dimension questions:
- "Is there a timeout on this outbound call?" — universal
- "Is the consumer idempotent?" — universal

Not suitable for core:
- "Does this follow our internal naming convention X?" — team-specific
- "Is this service registered in our internal registry?" — company-specific

### Local extension dimensions

Teams can maintain their own dimension files without contributing to core.

Add a `--dimensions-path <dir>` argument when invoking the skill to include local dimensions alongside core ones:

```
/what-else-review --pr https://... --dimensions-path ./our-team/dimensions
```

Local dimension files follow the same format as core dimensions. They are merged with the selected core dimensions during review.

---

## Versioning

- Core dimensions are released as GitHub tags (e.g. `v1.0.0`)
- The skill file specifies which dimension version it fetches
- A new dimension release is cut when a meaningful set of changes accumulates
- Patch releases (`v1.0.1`) for corrections; minor releases (`v1.1.0`) for new dimensions or significant additions

---

## Skill changes

The skill (`skills/what-else-review.md`) is versioned independently from dimensions.

- Skill version tracks in the file header: `**Skill version:** vX.Y.Z`
- Breaking argument changes (rename, removal) require a migration note in the header
- Dimension version used by the skill is pinned explicitly and updated deliberately

---

## What not to contribute

- Language-specific checklists (Go, Python, Java) — these belong in local extensions
- Company-specific tooling references — same
- Formatting or style opinions — out of scope entirely
