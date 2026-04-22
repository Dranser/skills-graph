# Scheduler

## Core Frame

Scheduler work is about explicit task boundaries, worker ownership, and synchronization cost.

Good scheduling does not compensate for incoherent dataflow. It makes coherent work execute predictably.

Lifecycle order is part of the scheduler problem, not a separate cleanup concern.

## Work Budget Doctrine

Scheduler design should assign explicit budgets to important phases and queues.

A budget should define:
- maximum work per phase or tick
- what gets deferred when the budget is exceeded
- how backlog is observed and drained

Without that, the scheduler is not really controlling runtime cost.

## Latency vs Throughput Decision Frame

Ask first:
- is this phase responsiveness-sensitive
- or throughput-sensitive

If responsiveness-sensitive:
- keep work slices bounded
- prefer shorter completion delay
- accept some throughput loss

If throughput-sensitive:
- allow larger bounded batches
- accept some extra delay
- keep worst-case spikes inside the phase budget

## Decision Table

| Situation | Preferred Move |
| --- | --- |
| Main-thread runtime methods accumulate too much work | Split into bounded incremental slices |
| A phase has no explicit budget | Define the budget before tuning dispatch mechanics |
| Responsiveness matters more than raw work completion | Use latency-biased bounded slices |
| Throughput matters more than immediate response | Use bounded batch windows with explicit delay tolerance |
| Stable independent partitions exist | Partition-first scheduling |
| Pipeline stages are already explicit | Stage-aligned scheduling |
| Work units are tiny and numerous | Increase granularity before adding scheduler complexity |
| Dynamic imbalance is proven under load | Add controlled dynamic balancing |

## Escalation Rules

Escalate to `data-oriented-design` when:
- worker or stage problems are really caused by incoherent layout and traversal

Escalate to `memory-management` when:
- scheduler spikes are driven by temporary allocation bursts and unclear lifetime ownership

Escalate to `cache-optimization` when:
- scaling collapses mainly because of false sharing or locality loss after worker partitioning

## Anti-Pattern Matrix

| Anti-Pattern | Why It Fails |
| --- | --- |
| Unbounded runtime work on the main thread | Narrow frame budget gets blocked by burst execution |
| Implicit lifecycle ordering between systems | Races and unstable timing appear |
| No explicit per-phase budget | Backlog behavior becomes accidental and opaque |
| Maximizing throughput without tracking latency | User-visible spikes or delay go unnoticed |
| Chasing low latency by slicing work beyond usefulness | Dispatch overhead eats the gain |
| Spawning very small tasks everywhere | Scheduling overhead dominates |
| Shared global mutable state across workers | Contention and correctness risks rise |
| Adding dynamic scheduling before measuring imbalance | Complexity grows without payoff |
| Synchronization scattered inside task logic | Cost becomes opaque and hard to review |
| Solving uneven work by adding more workers instead of smoothing the workload | Main-thread spikes and coherence issues remain |
| Treating deterministic order as optional in stateful runtime systems | Debugging and correctness degrade together |

## Review Playbook

1. Define task size and ownership.
2. Define lifecycle order and execution phases.
3. Define queue, slice, or partition model.
4. Define synchronization boundaries.
5. Measure contention, worker utilization, latency, and main-thread spikes.
6. Adjust granularity or partitioning before adding more machinery.

## Review Matrix

| Review Question | If "No" | Escalate To |
| --- | --- | --- |
| Is lifecycle order explicit and inspectable? | Timing and race issues remain architectural | remain in `scheduler` |
| Is main-thread work bounded per tick or phase? | Spike prevention is still unresolved | remain in `scheduler` |
| Does the phase have a clear work budget and overflow policy? | Runtime cost is still effectively unbounded | remain in `scheduler` |
| Is the latency-versus-throughput priority explicit? | The execution shape may be wrong for the phase | remain in `scheduler` |
| Are task sizes large enough to justify dispatch overhead? | Granularity is still wrong | remain in `scheduler` |
| Is worker ownership clearer than the shared-state alternative? | Concurrency model is still too implicit | remain in `scheduler` |
| Are remaining issues really about layout or locality instead? | Scheduler may not be the root owner | `data-oriented-design` or `cache-optimization` |
