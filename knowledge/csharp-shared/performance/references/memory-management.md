# Memory Management

## Core Frame

Memory management in this domain means:
- allocation discipline
- lifetime discipline
- explicit ownership
- bounded reuse

The goal is not "zero allocations everywhere". The goal is predictable cost where it matters.

## Decision Table

| Situation | Preferred Move |
| --- | --- |
| Hot path repeatedly allocates temporary collections | Reusable working buffer |
| Stable short-lived objects are created and retired frequently | Bounded pool with reset rules |
| Ownership is unclear across subsystems | Define owner, borrower, and release rules first |
| Rare cold-path allocations dominate code complexity discussions | Defer optimization until evidence exists |

## Escalation Rules

Escalate to `data-oriented-design` when:
- repeated allocations are a symptom of poor layout or staging rather than missing pooling

Escalate to `scheduler` when:
- allocation spikes come from burst-heavy lifecycle execution or oversized per-tick work

Escalate to `cache-optimization` when:
- the memory issue is not allocation count but poor adjacency or false sharing

## Anti-Pattern Matrix

| Anti-Pattern | Why It Fails |
| --- | --- |
| Pooling everything by default | Complexity and stale-state risk rise fast |
| Reusing buffers without reset discipline | Old state leaks into new work |
| Keeping peak-sized buffers forever | Memory retention becomes the next problem |
| Focusing on allocation count without cost context | High-signal bottlenecks stay unfixed |
| Passing ownership implicitly through convenience helpers | Lifetime bugs become structural and hard to review |
| Using pools to hide burst-heavy execution instead of smoothing the workload | The root runtime shape stays broken |

## Review Playbook

1. Identify hot allocation sites.
2. Classify lifetimes: pass-local, short-lived, long-lived.
3. Choose reuse only where the lifecycle is stable.
4. Define reset and shrink rules.
5. Re-measure memory pressure and total system cost.

## Review Matrix

| Review Question | If "No" | Escalate To |
| --- | --- | --- |
| Is ownership explicit? | Lifetime bugs are still likely | remain in `memory-management` |
| Are hot allocations measured rather than assumed? | The target may be misidentified | `performance-entry` |
| Is reuse bounded by reset and capacity rules? | Pooling may be unsafe | remain in `memory-management` |
| Are spikes caused by lifecycle bursts rather than raw allocation frequency? | Scheduling shape may dominate | `scheduler` |
| Is layout causing object churn indirectly? | Data shape may need redesign | `data-oriented-design` |
