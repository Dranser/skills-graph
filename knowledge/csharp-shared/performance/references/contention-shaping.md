# Contention Shaping

## Purpose

Use this reference when a runtime scales poorly because too much work converges on one shared queue, lock, or mutable structure.

## Core Rules

- diagnose contention as an ownership problem first
- make serialization points explicit
- prefer partition and merge over broad shared mutation
- keep backpressure visible

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Many workers write to one shared structure | Partition ownership and merge later | Shared mutation is often the true bottleneck. |
| One hot queue serializes work | Reduce coordination scope or add staged handoff | Queue design should not dominate useful work. |
| Backlog grows behind a contested point | Define explicit backpressure behavior | Queue buildup should be architectural, not accidental. |
| Scaling fails but contention is not measured yet | Instrument first | Primitive tuning without evidence is guesswork. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Lock-first thinking | Many lock variants tried, little real improvement | Redesign ownership and serialization shape first. |
| Hidden serialization | Parallel system still behaves almost serially | Expose the bottleneck and stage it explicitly. |
| Unobserved backpressure | Latency rises with no clear control point | Add queue-depth and backlog policy. |
| Shared-write convenience | Fast to code, poor to scale | Partition or stage writes by ownership. |

## Review Playbook

1. Identify the contested structure.
2. Measure waiting, serialization, and backlog.
3. Reshape ownership or staging.
4. Define backpressure behavior.
5. Re-measure scaling and latency.
