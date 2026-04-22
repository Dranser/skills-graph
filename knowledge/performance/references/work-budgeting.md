# Work Budgeting

## Purpose

Use this reference when the main performance question is how much work a runtime phase should be allowed to perform and what should happen when that limit is exceeded.

## Core Rules

- define a budget for every critical phase
- choose overflow behavior explicitly
- bias toward bounded work on latency-sensitive paths
- measure spikes and backlog growth, not averages alone

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A frame or tick can absorb arbitrarily large work | Add an explicit per-phase budget | Unbounded work becomes visible as spikes. |
| Responsiveness matters more than raw throughput | Use smaller bounded slices with carry-over state | Latency-sensitive phases need stable worst-case cost. |
| Throughput matters more and bounded delay is acceptable | Use larger batch windows inside a latency guardrail | Batching should still respect worst-case delay. |
| Work exceeds the phase budget regularly | Define queue, defer, merge, or drop policy explicitly | Budgeting is incomplete without overflow behavior. |

## Overflow Policy Heuristics

Queue when:
- work must eventually complete
- order matters
- bounded backlog can be observed and controlled

Defer when:
- work is optional this pass
- a later pass can absorb it safely

Merge when:
- multiple pending updates can collapse into one newer representation

Drop when:
- stale work has no user-visible value once newer work exists

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Budget only in comments | Runtime still behaves as if work were unbounded | Enforce the budget in control flow and metrics. |
| Undefined overflow | Backlog growth appears accidental and hard to debug | Choose a queue, defer, merge, or drop policy explicitly. |
| Average-only thinking | Benchmarks look fine while real sessions spike | Track worst-case latency and backlog depth. |
| Over-fragmented slices | Queue overhead dominates useful work | Increase chunk size or redesign the phase boundary. |

## Review Playbook

1. Name the critical phase.
2. Define the budget in concrete units.
3. Define overflow behavior.
4. Measure worst-case latency and backlog depth.
5. Verify the budget matches the runtime's actual priority.
