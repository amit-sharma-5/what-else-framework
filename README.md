# What-Else-Review

An AI-assisted engineering review workflow that runs **after implementation**.

Its goal is not to find coding bugs or replace code review.

Its purpose is to answer one question:

> **What else should we have considered?**

Every review produces an **Engineering Decision Record (EDR)** that lives next to the code it describes.

---

## Why

Most engineering workflows end after implementation, tests, and PR merge.
Over time, valuable context is lost: assumptions, trade-offs, operational concerns, known limitations.

Months later, teams ask:
- Why wasn't this logged?
- Why wasn't idempotency implemented?
- What happens if the downstream service is unavailable?

The answers are usually undocumented.

---

## Structure

```
what-else-review/
  README.md
  skills/
    what-else-review.md       # Claude Code skill
  templates/
    engineering-decision-record.md
  dimensions/
    api.md
    database.md
    events.md
    observability.md
    reliability.md
    security.md
    performance.md
    concurrency.md
    operations.md
  examples/
```

---

## Workflow

```
Feature / Bug Complete
        ↓
Run What-Else-Review Skill
        ↓
Engineering Decision Record (EDR)
        ↓
Developer
    ├── Fix now (critical)
    ├── Create follow-up task (debt)
    └── Accept trade-off (documented)
```

EDRs live in `/edrs/` inside the service repo they describe.

---

## Input

Feed the skill any combination of:
- `git diff main..feature-branch`
- Relevant proto/interface definitions for cross-service boundaries
- A brief description of the feature

The skill detects technologies used and selects relevant review dimensions automatically.

---

## What the skill does NOT do

- Replace static analysis or security scanners
- Replace human code review
- Focus on formatting or style
- Run on every PR — run once per feature/bug when complete

---

## EDR placement

EDRs belong next to the code they describe:

```
your-service/
  src/
  edrs/
    2026-08-01-payment-http-grpc-integration.md
```

---

## Success criteria

1. Engineers spend less than 10 minutes reviewing an EDR.
2. Every feature leaves behind useful engineering context.
3. Future engineers can understand why decisions were made.
4. Recurring gaps surface across EDRs over time.
