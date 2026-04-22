# Runtime Synchronization Contract

Parallel runtime performance is valid only when data visibility is disciplined.

## Tick Lifecycle

Use this sequence for worker-capable runtime execution:

1. Capture snapshot.
2. Compute.
3. Barrier.
4. Commit and publish.
5. Submit.

Do not merge these steps implicitly.

## Capture

Capture converts host-facing input into runtime-owned state.

Rules:

- workers read runtime-owned snapshots only;
- snapshot data is immutable for the tick;
- Unity or host objects are not read inside worker tasks;
- input capture cost is measured separately from compute.

## Compute

Allowed worker patterns:

- disjoint range writes;
- read-only shared input;
- thread-local scratch;
- worker-local partials;
- explicit reductions.

Forbidden patterns:

- structural mutation;
- direct host writes;
- shared mutable output without a merge contract;
- reading partially published state.

## Barrier

A barrier must exist before downstream phases observe aggregate results, published snapshots, render payloads, diagnostics, or host-facing state.

The barrier is part of the architecture, not a scheduler detail.

## Commit And Publish

Commit is where write-state becomes visible. Publish creates stable output for host consumers.

Good designs separate:

- read snapshot;
- worker write state;
- reduction partials;
- committed simulation state;
- published host-facing payloads.

## Validation

Before calling worker execution ready, validate:

- single-thread and worker output equivalence;
- disjoint write ranges;
- deterministic reduction merge behavior;
- no cross-tick transient state leaks;
- no host reads before publish;
- no worker access to Unity state.
