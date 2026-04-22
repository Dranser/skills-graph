# Neural Training Runtime

Training is a sequence of explicit tensor state transitions.

## Phase Contract

A custom training loop should separate:

- batch ingestion;
- input upload or binding;
- forward;
- loss and metric computation;
- output-gradient write;
- backward;
- gradient accumulation or normalization;
- gradient diagnostics or clipping;
- optimizer update;
- accumulator clearing;
- telemetry;
- checkpointing.

Each phase should list which tensor roles it reads and writes.

## Tensor Roles

Keep these roles separate:

- inputs;
- targets;
- masks;
- activations;
- predictions;
- loss scratch;
- output gradients;
- parameter gradients;
- gradient accumulators;
- parameters;
- optimizer state;
- checkpoint staging;
- telemetry or diagnostic buffers.

Sharing storage is allowed only when lifetime and overwrite order are explicit.

## Gradient And Optimizer State

Gradient tensors should be runtime-owned. Optimizer state should live beside parameters in the same ownership model.

For each optimizer command, record:

- parameter offset and shape;
- gradient offset and shape;
- state tensor offsets and shapes;
- learning rate and hyperparameters;
- precision and conversion policy;
- whether update is in-place.

Gradient logging and clipping should be optional, bounded, and synchronized deliberately. Downloading all gradients every batch is a diagnostic path, not a default throughput path.

## Checkpointing

Checkpoint code must know the current source of truth:

- CPU arena;
- GPU-resident buffer;
- hybrid with dirty ranges;
- stale mirror that must not be used.

Checkpoint only the required tensors. Include shape, precision, backend, dataset/training context, optimizer metadata, and versioning.

## Telemetry

Useful training telemetry includes:

- per-phase timings;
- batch size and token/sample counts;
- arena active/reserved/free/peak usage;
- backend label;
- execute and dispatch counts;
- upload/download bytes;
- optional gradient norm and clipping state;
- loss, accuracy, perplexity, or task-specific metrics.

Telemetry should not force blocking readback unless the metric explicitly requires it.
