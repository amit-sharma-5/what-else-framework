# What-Else Framework

A contextual questioning framework that improves **engineering decision quality** at every stage of the development lifecycle.

> **The job of this framework is not to answer questions.**
> Its job is to make sure you don't forget to ask them.

---

## The Problem

Engineering decisions are made continuously — during design, implementation, incident reviews, and architectural choices.

Most of them go undocumented. The context, assumptions, trade-offs, and alternatives are lost.

Months later, teams ask:
- Why wasn't this logged?
- Why wasn't idempotency implemented?
- What happens if the downstream service is unavailable?
- Why was Kafka chosen over RabbitMQ?

The answers exist. They just were never written down.

---

## The Framework

```
                    What-Else Framework

                ┌─────────────────────────┐
                │          Idea           │
                └──────────┬──────────────┘
                           │
        ┌──────────────────┼─────────────────────┐
        │                  │                     │
      RFC                 ADR                Feature
        │                  │                     │
        └──────────────────┼─────────────────────┘
                           │
                     Implementation
                           │
                      Code Review
                           │
                           ▼
                Engineering Decision Record
```

The same questioning applies at every stage. The artifact changes. The thinking does not.

---

## How It Works

You write the RFC, ADR, feature, or incident report.

The framework — or an AI skill — asks: **What else?**

You answer the questions that matter. The document gets better.

The framework does not generate documentation. It challenges it.

---

## Structure

```
what-else-framework/

  frameworks/       ← (roadmap) per-stage questioning guides
    rfc.md          ← What Else before building
    adr.md          ← What Else before deciding
    feature.md      ← What Else after implementing
    bugfix.md       ← What Else after fixing
    incident.md     ← What Else after an outage

  dimensions/       ← Review heuristics per technology
    api.md
    database.md
    events.md
    observability.md
    reliability.md
    security.md
    performance.md
    concurrency.md
    operations.md
    data-integrity.md
    testing-strategy.md

  skills/           ← AI skills that apply the framework
    what-else-review.md

  templates/        ← Output artifacts
    engineering-decision-record.md

  examples/         ← Real EDRs showing the framework in use

  architectural-review-reports/  ← Framework's own self-review history

  CONTRIBUTING.md   ← How to add or extend dimensions
  SECURITY.md       ← EDR sensitivity and URL trust guidance
```

---

## Skills

### `/what-else-review` — first implemented skill

Runs after a feature or bug fix is complete. Reads a diff, detects technologies, selects relevant dimensions, and produces an **Engineering Decision Record (EDR)**.

**Install globally (one command):**
```bash
curl -o ~/.claude/commands/what-else-review.md \
  https://raw.githubusercontent.com/amit-sharma-5/what-else-framework/main/skills/what-else-review.md
```

**Usage:**
```
/what-else-review --pr https://github.com/org/service/pull/42
/what-else-review --tickets https://jira.../PAM-5163 --services service-a service-b
/what-else-review --branch feat/payment-api --dry-run
```

| Argument | Description |
|----------|-------------|
| `--tickets <url>...` | Jira ticket URLs (requires Jira MCP) |
| `--pr <url>` | GitHub PR URL |
| `--branch <name>` | Branch name |
| `--services <name>...` | Restrict to named services only |
| `--context "<text>"` | Extra context the diff can't show |
| `--contract <path\|url>` | Service contract — proto, OpenAPI, or AsyncAPI spec |
| `--trusted-domain <host>` | Add trusted hostname for `--contract` URL validation |
| `--output <path>` | Override EDR output directory |
| `--dry-run` | Print EDR without writing file |

EDRs are saved versioned by timestamp next to the code they describe:
```
your-service/
  edrs/
    2026-08-01-1430-payment-http-grpc.md
    2026-08-01-1600-payment-http-grpc.md   ← re-run after fixes
```

---

## Dimensions

Dimensions are the reusable review heuristics. Each dimension is a set of "What Else?" questions for a specific technology or concern.

The AI skill auto-selects relevant dimensions based on what it detects in the diff. Engineers can also apply them manually during design and review.

---

## Works Without AI

The framework is AI-independent by design.

```
What Else?

  Database      Security      Operations
  Reliability   Performance   Failure modes
  Observability Migration     Alternatives
  Trade-offs    Unknowns
```

AI automates the asking. The framework is the thinking.

---

## Roadmap

- [x] `what-else-review` skill — feature/PR review → EDR
- [ ] `what-else-adr` skill — challenge ADRs
- [ ] `what-else-rfc` skill — challenge RFCs
- [ ] `what-else-incident` skill — post-incident review
- [ ] Framework templates for RFC, ADR, incident

> Skills are added as the framework is validated with real use.
> See `what-else-framework.md` for the full vision.
