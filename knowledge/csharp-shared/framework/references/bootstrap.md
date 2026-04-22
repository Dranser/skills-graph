# Framework Bootstrap

## Purpose

Use this reference when startup order, entrypoint structure, or composition root ownership is the main framework concern.

## Core Rules

- keep bootstrap thin
- initialize diagnostics before optional systems
- register core services before dependents
- make startup order explicit and reviewable

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A runtime host starts multiple systems | Define explicit startup phases | Hidden ordering becomes brittle. |
| Diagnostics must capture early failures | Initialize diagnostics first | Failure visibility must exist before optional systems. |
| Feature code begins to spread into entrypoint | Push it out of bootstrap | Bootstrap should orchestrate, not own feature behavior. |
| Multiple startup paths appear | Collapse to one authoritative composition root | Competing startup paths weaken lifecycle clarity. |

## Ownership Heuristics

Bootstrap should own:
- startup sequencing
- composition root boundaries
- early infrastructure bring-up

Bootstrap should not own:
- feature behavior
- ad hoc runtime coordination after startup
- hidden service registration side effects

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Fat bootstrap | Entrypoint owns feature behavior and becomes hard to review | Move behavior into runtime services and keep only orchestration in bootstrap. |
| Implicit startup order | Bugs depend on registration or initialization side effects | Declare startup phases explicitly. |
| Diagnostics starts too late | Early failures vanish or become hard to diagnose | Bring diagnostics up before optional systems. |
| Multiple partial bootstraps | Different runtime paths drift out of sync | Re-establish one authoritative composition root. |

## Escalation Rules

Escalate from bootstrap to supporting references when:
- service lifetime and graph composition dominate: use `di.md`
- boundary shape dominates: use `contracts.md`
- startup visibility dominates: use `diagnostics.md`

## Review Playbook

1. Name the host boundary.
2. List startup phases in order.
3. Mark which services are core and which are optional.
4. Verify diagnostics starts before optional systems.
5. Verify feature logic stays outside bootstrap.

## Review Matrix

| Review question | Healthy answer |
| --- | --- |
| Is startup order visible without reading side effects? | Yes, phases and registration order are explicit. |
| Can bootstrap be read quickly? | Yes, it orchestrates and delegates instead of owning feature detail. |
| Can early failures be observed? | Yes, diagnostics starts before optional systems. |
