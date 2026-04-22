# Performance Foundations

## Purpose

Use this reference when performance is not just a polish pass but an architectural owner.

The performance domain should frame work around:
- measurable cost
- strict lifecycle control
- explicit hot paths
- explicit ownership
- bounded recomputation
- predictable CPU and memory behavior
- workload smoothing instead of burst-heavy execution

## Operating Model

Start from:
- what order the system executes in
- where time is spent
- where memory is allocated or retained
- where contention occurs
- what is recomputed too often
- where burst workloads can stall the main thread

Do not start from:
- generic optimization folklore
- low-level tricks without a workload model
- library or abstraction preferences detached from measurement

## Base Doctrine

Performance starts with lifecycle control.

The baseline expectations are:
- execution order is strict and inspectable
- races are prevented through ownership and sequencing, not luck
- work volume is kept even rather than burst-heavy
- main-thread methods do bounded work by default
- large jobs are staged into smaller repeatable units when correctness allows it
- every critical runtime phase has an explicit work budget
- latency and throughput are treated as a tradeoff, not as one metric

The basic bias is:
- smaller work more often
- not larger work all at once

## Task-Conditioned Performance

Performance doctrine is general, but concrete performance decisions are task-conditioned.

That means:
- `performance` should define the laws of cost, ownership, budgeting, lifecycle, and recomputation
- the primary task domain should define what the actual bottleneck means in context
- generic optimization advice must yield to task-specific constraints once the bottleneck shape is known

Examples:
- `gpu + performance`
  performance governs CPU budgets, memory ownership, synchronization discipline, and diagnostics cost
  the `gpu` domain governs instancing strategy, data upload shape, readback policy, and what diagnostics are worth returning
- `unity-ui + performance`
  performance governs work budgets, incremental updates, and main-thread spike prevention
  the `unity-ui` domain governs event flow, UI state shape, and rendering-specific tradeoffs
- `data-oriented-pipeline + performance`
  performance governs cost modeling, batching discipline, and lifetime control
  the pipeline domain governs stage structure, invalidation flow, and execution topology

The practical rule is:
- performance should not pretend to own task-specific bottleneck choices by itself
- task domains should not ignore performance laws when shaping runtime behavior

## Work Budget Doctrine

Every important runtime phase should have a budget.

A budget answers:
- how much work the phase is allowed to do
- what happens when work exceeds that budget
- whether overflow is queued, deferred, dropped, or merged

The doctrine is:
- no unbounded runtime work on critical phases
- no accidental "just this once" burst path becoming the normal path
- no budget defined only in comments or intuition

Preferred pattern:
- fixed or measurable budget
- explicit carry-over state
- overflow strategy defined in advance

## Latency vs Throughput Doctrine

Latency and throughput are related but different.

Use latency-biased execution when:
- frame stability
- responsiveness
- input-to-result delay
- visible main-thread smoothness
matter more than peak batch efficiency

Use throughput-biased execution when:
- total work completed per window matters more than immediate response
- the stage can tolerate bounded delay
- batching materially reduces overhead

The rule is:
- do not optimize throughput by silently violating latency budgets
- do not optimize latency by fragmenting work so aggressively that total system cost collapses

## Decision Table

| Situation | Primary Lens | Secondary Lens | Expected Output |
| --- | --- | --- | --- |
| The optimization depends strongly on task-specific constraints or hardware behavior | task domain | `performance` | Domain-conditioned optimization plan |
| Runtime order, spike prevention, or main-thread smoothing dominate the problem | `scheduler` | `performance-entry` | Lifecycle and bounded-work execution plan |
| Explicit phase budgets or backlog control are missing | `scheduler` | `performance-entry` | Work budget policy and overflow strategy |
| Throughput is high but responsiveness is poor | `scheduler` | `performance-entry` | Latency-biased execution reshape |
| Responsiveness is fine but total work throughput is insufficient | `scheduler` | `data-oriented-design` | Throughput-biased batch and stage reshape |
| Layout and traversal dominate cost | `data-oriented-design` | `cache-optimization` | New data layout and stage plan |
| Allocation churn dominates cost | `memory-management` | `data-oriented-design` | Ownership and reuse model |
| Contention or worker orchestration dominates cost | `scheduler` | `cache-optimization` | Explicit task and synchronization model |
| Cost is broad and unclassified | `performance-entry` | context-specific skill | Measurement-first diagnosis plan |

## Escalation Rules

Escalate from `performance-entry` to:
- `scheduler` when the dominant issue is lifecycle order, spikes, task boundaries, contention, or worker orchestration
- `data-oriented-design` when traversal and layout dominate cost
- `memory-management` when allocation churn, pooling, or lifetime ambiguity dominate
- `cache-optimization` when locality or false sharing dominate after the hot path is known

Escalate out of the performance domain when:
- the optimization target is primarily governed by a task-specific domain such as `gpu`, `unity-ui`, or another concrete execution owner
- the task is actually owned by a feature domain and performance is only a supporting concern
- the design question is about UI architecture, plugin integration, or assembly tooling rather than runtime cost ownership

## Anti-Pattern Matrix

| Anti-Pattern | Why It Fails |
| --- | --- |
| Optimizing without identifying the hot path | Work drifts into low-impact code |
| Letting runtime methods perform unbounded burst work on the main thread | Frame spikes become the real bottleneck |
| Treating lifecycle order as implicit | Hidden races and unstable timing emerge |
| Optimizing throughput with no explicit latency budget | Average performance improves while runtime quality degrades |
| Optimizing latency by over-fragmenting work with no throughput discipline | Scheduling overhead and total cost explode |
| Applying generic performance advice without conditioning it on the real task bottleneck | The "optimization" targets the wrong problem |
| Micro-optimizing before fixing data layout or ownership | Local tweaks fail to move whole-system cost |
| Treating every optimization as a pooling or threading problem | Complexity rises while the real bottleneck remains |
| Recomputing whole structures by default | CPU cost scales with system size instead of actual change |
| Using abstraction layers that hide allocations, contention, or traversal | Costs become hard to reason about and harder to review |
| Chasing average throughput while ignoring worst-case spikes | Runtime remains unstable where users actually feel it |
| Spreading performance responsibility across every subsystem with no clear owner | No one is accountable for the dominant cost |

## Review Heuristics

- Can the dominant cost be named concretely?
- Is lifecycle order explicit and stable?
- Does every critical phase have an explicit work budget?
- Is the optimization conditioned on the actual task domain rather than generic folklore?
- Does the design reduce cost structurally instead of cosmetically?
- Are ownership and lifetime explicit?
- Is main-thread work bounded and smoothed instead of burst-heavy?
- Is the latency versus throughput tradeoff explicit?
- Is recomputation scoped to what changed?
- Is the proposed complexity proportional to the measured win?

## Review Matrix

| Review Question | If "No" | Escalate To |
| --- | --- | --- |
| Is the dominant cost explicitly identified? | The optimization target is still vague | keep diagnosis in `performance-entry` |
| Is lifecycle order explicit and stable? | Timing and race risks remain architectural | `scheduler` |
| Does each critical phase have a defined budget? | Burst paths may still be unbounded | `scheduler` |
| Is the latency-versus-throughput priority explicit? | Execution shape may be chosen by accident | `scheduler` |
| Does the current optimization depend on task-specific execution constraints? | Generic performance advice is no longer enough | task domain + `performance` |
| Is data access shaped around actual hot traversal? | Layout likely dominates | `data-oriented-design` |
| Are ownership and lifetime explicit? | Allocation and retention cost may be hidden | `memory-management` |
| Are locality and write-partition risks explicit? | Cache behavior may be the limiting factor | `cache-optimization` |
