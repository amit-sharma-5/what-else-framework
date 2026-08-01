# What-Else Framework

## The Core Idea

"What Else" is not a code review tool.

It is a **contextual questioning framework** that improves engineering decision quality at every stage — not just after implementation.

The job of the framework is **not to answer**:

> "Should we use Kafka?"

Its job is to ensure you don't forget to ask:

- What happens if Kafka is unavailable?
- What ordering guarantees do we need?
- What retry strategy are we assuming?
- How will we observe failures?
- Under what conditions would we replace Kafka in the future?

That distinction keeps the framework lightweight, reusable, and AI-independent.

---

## Where It Applies

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

The same questions surface at every stage — the artifact changes, the thinking does not.

---

## Framework Structure

```
what-else-framework/

  README.md

  frameworks/
    rfc.md          ← What Else before building
    adr.md          ← What Else before deciding
    feature.md      ← What Else after implementing
    bugfix.md       ← What Else after fixing
    incident.md     ← What Else after an outage
    postmortem.md   ← What Else was missed

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
    data-integrity.md
    testing-strategy.md

  skills/
    what-else-review.md    ← applied to feature/PR
    what-else-rfc.md       ← applied to RFC documents
    what-else-adr.md       ← applied to ADRs
    what-else-incident.md  ← applied to incident reports

  templates/
    edr.md          ← Engineering Decision Record
    adr.md          ← Architecture Decision Record
    rfc.md          ← Request for Comments
```

---

## How It Works Per Stage

### RFC — What Else before building?

**Trigger:** Author runs before sharing the RFC for team review.
**Output:** Annotated version of the RFC with gaps and unanswered questions added inline. Not a new document.

Instead of asking: *Does this design work?*
Ask: *What else?*

- What requirements are missing?
- What alternatives were not considered?
- How does this behave at 10× traffic?
- Migration path? Rollback?
- What assumption might become invalid in 6 months?
- What happens if Product changes this requirement?
- Security, PII, encryption?
- Runbooks, alerts, observability?

The AI does not write the RFC. It challenges it. You answer the questions that matter. The RFC gets better.

---

### ADR — What Else before deciding?

**Trigger:** Decision made but not yet implemented. Run before the ADR is merged.
**Output:** Gaps and open questions added to the ADR itself. The ADR author resolves or explicitly accepts each one before merging.

Traditional ADR:
```
Decision: Use Kafka.
```

What-Else ADR:
```
Decision: Use Kafka.

Alternatives: RabbitMQ, SQS

Trade-offs: Higher operational complexity

Assumptions: High throughput needed

Failure Modes: Broker unavailable

Future Review: Revisit if throughput stays under 100 msgs/sec

Observability: Consumer lag metrics required

Open Questions: Exactly-once delivery required?
```

Same decision — much stronger document.

---

### Feature — What Else after implementing?

Exactly what the current `what-else-review` skill does.
Produces an Engineering Decision Record (EDR).

---

### Incident — What Else after an outage?

**Trigger:** After the incident is resolved and root cause identified. Run before the postmortem is written.
**Output:** Extended postmortem with systemic gaps captured, not just the immediate fix.

Instead of stopping at root cause, ask:

- What signal did we miss?
- What documentation was absent?
- What dashboard was missing?
- What assumption failed silently?
- What test would have caught this?
- What runbook would have shortened the MTTR?

---

## The Key Insight

The framework works without AI.

```
What Else?

  Database      Security     Operations
  Reliability   Performance  Failure
  Observability Migration    Alternatives
  Trade-offs    Unknowns
```

AI automates the asking. The framework is the thinking.

---

## What This Is Not

- Not a universal checklist for every project
- Not a replacement for code review, static analysis, or security scanners
- Not a documentation generator — it challenges documentation, not produces it
- Not opinionated about technology choices — it surfaces the right questions, not the answers

---

## Evolution

```
"Can AI review my PR?"
        ↓
"Can AI help catch what we missed after implementing?"
        ↓
"Can we create a systematic way of thinking
 that improves engineering decisions at every stage?"
```

That last question is what this framework answers.
