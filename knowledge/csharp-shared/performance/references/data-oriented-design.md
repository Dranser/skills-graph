# Data-Oriented Design

## Core Frame

Data-oriented design is about shaping data around the way it is consumed, not around the way it is described conceptually.

The main questions are:
- what is touched together
- in what order it is touched
- how often it changes
- what can be staged or batched

## Decision Table

| Symptom | Preferred Move |
| --- | --- |
| Tight loop jumps across many objects | Flatten or stage the hot data |
| Large records are only partially used in hot code | Split hot and cold state |
| Many small updates hit the same pass | Batch updates into a staged buffer |
| Sparse source state feeds dense execution work | Build a dense execution representation |

## Escalation Rules

Escalate to `cache-optimization` when:
- the hot traversal is already known and the remaining issue is locality or false sharing

Escalate to `memory-management` when:
- the layout is acceptable but temporary allocations or ownership churn dominate

Escalate to `scheduler` when:
- the problem is no longer layout but execution order, task slicing, or main-thread spikes

## Anti-Pattern Matrix

| Anti-Pattern | Why It Fails |
| --- | --- |
| Calling OO records "data-oriented" without changing access patterns | Traversal cost stays the same |
| Flattening all state blindly | Cold and irregular state pollutes hot memory |
| Rebuilding full execution buffers every frame or pass | Staging cost can dominate the gain |
| Treating DOD as a style preference instead of a workload decision | The design loses falsifiability |
| Forcing direct hot-path layout when staged derived data would be cheaper | The source model becomes harder to own without runtime benefit |
| Mixing hot execution data with rarely changed control metadata | Cache behavior regresses and update scope becomes less clear |

## Review Playbook

1. Name the dominant hot traversal.
2. List the fields actually touched there.
3. Separate hot, cold, and derived execution data.
4. Decide whether the system needs direct layout or staged layout.
5. Re-check whether recomputation can be bounded by dirty scope.

## Review Matrix

| Review Question | If "No" | Escalate To |
| --- | --- | --- |
| Is the hot traversal explicitly named? | Layout work is still under-specified | `performance-entry` |
| Are hot and cold fields separated? | The layout still carries unused state | remain in `data-oriented-design` |
| Is derived execution data justified by repeated use? | Staging may be needless overhead | remain in `data-oriented-design` |
| Is recomputation scoped to dirty subsets? | Stage cost may still be too broad | `scheduler` or `performance-entry` |
| Are remaining wins mainly about adjacency and contention? | The issue is now lower-level locality | `cache-optimization` |
