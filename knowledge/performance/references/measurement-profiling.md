# Measurement And Profiling

## Purpose

Use this reference when the performance problem is still diagnostic: you need to know what really dominates cost before redesigning the system.

## Core Rules

- instrument explicit architectural boundaries
- measure worst-case behavior as well as averages
- classify the bottleneck before prescribing a fix
- keep profiling tied to representative workloads

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| The runtime is slow but the dominant cost is unclear | Add phase-scoped timers and counters | Architectural attribution comes before redesign. |
| Spikes are visible but average timing looks acceptable | Capture tail latency and backlog metrics | Average-only data hides the real failure mode. |
| Queueing or staged work is involved | Track queue depth and overflow behavior | Work buildup often explains instability better than raw timings alone. |
| The bottleneck class is already obvious | Escalate to the owning skill | Diagnosis should not linger once cost ownership is clear. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| One giant performance number | You know the runtime is slow but not why | Instrument phase and ownership boundaries. |
| Average-only analysis | Metrics look fine while users still hit spikes | Add worst-case and backlog tracking. |
| Benchmark theater | Synthetic numbers disagree with real runtime pain | Re-profile under representative workloads. |
| Blind redesign | Many changes land before cost attribution is clear | Classify the bottleneck first. |

## Review Playbook

1. Name the candidate hot phases.
2. Add timers and counters at owned work boundaries.
3. Capture representative load.
4. Compare averages, worst-case timings, and backlog behavior.
5. Hand off to the owning optimization skill.
