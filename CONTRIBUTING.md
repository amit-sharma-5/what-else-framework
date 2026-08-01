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

This repo uses [Semantic Versioning](https://semver.org): `MAJOR.MINOR.PATCH`

| Change | Version bump | Examples |
|--------|-------------|---------|
| Breaking change — argument renamed/removed, template structure changed | **MAJOR** `X.0.0` | `--proto` → `--contract` rename |
| New functionality — new argument, new dimension, new template section | **MINOR** `x.Y.0` | `--trusted-domain` added, `Engineer Notes` added |
| Bug fix — stale comment, doc sync, diagram fix, version header correction | **PATCH** `x.y.Z` | Fixing wrong version comment, README alignment |

**Rules:**
- Tag and skill version header are bumped in the same commit
- Dimensions and template are pinned independently in the skill (dimensions = review heuristics, template = output contract)
- Breaking changes require a migration note in the skill header
- A new dimension release is cut when a meaningful set of changes accumulates

---

## Skill changes

The skill (`skills/what-else-review.md`) is versioned independently from dimensions.

- Skill version tracks in the file header: `**Skill version:** vX.Y.Z | Dimensions: vA.B.C | Template: vX.Y.Z`
- Breaking argument changes (rename, removal) require a migration note in the header
- Template version tracks skill major version — they share the same output contract
- Dimension version is pinned separately — only updated when review heuristics change

---

## What not to contribute

- Language-specific checklists (Go, Python, Java) — these belong in local extensions
- Company-specific tooling references — same
- Formatting or style opinions — out of scope entirely
