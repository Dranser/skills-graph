# Neural GPU Residency

GPU residency is an ownership policy for tensor data.

## Source Of Truth

For each tensor group, identify whether the current source of truth is:

- CPU resident;
- GPU resident;
- hybrid with dirty ranges;
- checkpoint staging only;
- diagnostic copy only.

Parameters, activations, gradients, accumulators, and optimizer state should stay GPU-resident when GPU throughput is the goal. Inputs and checkpoints are natural synchronization boundaries; diagnostics are optional synchronization boundaries.

## Transfer Policy

Classify every transfer:

- required input ingestion;
- required target or mask ingestion;
- checkpoint download;
- validation or consistency check;
- telemetry;
- accidental synchronization;
- stale mirror repair.

Full-arena upload/download after every backend execution is a major architecture cost. Prefer dirty ranges, tensor-level staging, or GPU-resident source of truth before shader micro-optimization.

## Command Grouping

Small commands can be dominated by dispatch and binding cost. Consider:

- grouping linear elementwise operations;
- compiling stable command arrays;
- separating shader groups without rebinding per tiny operation when possible;
- keeping loss, accumulation, and optimizer operations backend-executable;
- measuring dispatch count per batch.

## Readback Rules

Readback is acceptable for:

- checkpointing;
- deterministic CPU/GPU consistency tests;
- bounded diagnostics;
- final inference output;
- user-requested inspection.

Readback is suspicious when it controls every training step, computes normal optimizer state on CPU, or exists only because the runtime lacks dirty/residency tracking.

## Migration Path

When moving from CPU-backed arena to GPU-resident runtime:

1. Add tensor or range dirty state.
2. Define CPU and GPU source-of-truth transitions.
3. Keep explicit staging paths for inputs and checkpoints.
4. Move parameters and optimizer state first.
5. Move activations and gradients next.
6. Remove full-arena synchronization from hot backend execution.
7. Add telemetry for bytes uploaded, bytes downloaded, execute count, dispatch count, and blocking readback time.
