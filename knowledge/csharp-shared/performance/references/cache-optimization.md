# Cache Optimization

## Core Frame

Cache optimization is about access order, adjacency, and write ownership.

It is not about low-level instruction trivia in isolation.

## Decision Table

| Symptom | Preferred Move |
| --- | --- |
| Hot reads bounce across far-apart memory | Reorder layout or stage into dense buffers |
| Loop order fights data order | Reorder traversal first |
| Multi-threaded scaling collapses during writes | Inspect false sharing and partition writes |
| Cache talk has no explicit hot loop | Stop and define the hot loop first |

## Escalation Rules

Escalate to `data-oriented-design` when:
- locality problems come from a broader mismatch between workload and data model

Escalate to `scheduler` when:
- the dominant issue is not adjacency but cross-thread write ownership or phase ordering

Escalate to `performance-entry` when:
- no explicit hot loop or access pattern has been identified yet

## Anti-Pattern Matrix

| Anti-Pattern | Why It Fails |
| --- | --- |
| Talking about locality without naming the traversal | The optimization target is undefined |
| Forcing cache tuning onto cold code | Complexity is spent where payoff is minimal |
| Ignoring write ownership between threads | False sharing survives layout tweaks |
| Breaking ownership clarity for speculative locality gains | Review and maintenance costs rise |
| Reordering loops without checking semantic constraints or stage boundaries | Apparent locality wins can break correctness |
| Treating read locality as the only cache problem | Write contention and invalidation traffic stay hidden |

## Review Playbook

1. Name the hot loop.
2. Map read and write order to memory layout.
3. Check adjacency of hot data.
4. Check thread write ownership and false sharing risk.
5. Re-measure after each structural change.

## Review Matrix

| Review Question | If "No" | Escalate To |
| --- | --- | --- |
| Is the hot loop explicit? | Cache work is premature | `performance-entry` |
| Is the loop order aligned with the dominant data order? | Traversal still fights layout | remain in `cache-optimization` |
| Are writes partitioned safely across workers? | False sharing or contention may dominate | `scheduler` |
| Is the root issue actually the data model itself? | Layout mismatch is broader than cache tuning | `data-oriented-design` |
| Were wins measured after each change? | The optimization may be folklore-driven | `performance-entry` |
