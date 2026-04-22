# Neural Tensor Runtime Architecture

The neural domain is centered on data architecture: tensors are explicit runtime memory contracts, not passive model metadata.

## Two Tensor Layers

Use two tensor concepts deliberately:

- semantic tensors: CPU-side shape, rank, stride, view, transpose, and validation objects;
- runtime tensors: arena-owned 3D slices with offset, length, dimensions, precision, and lifetime.

Semantic tensors are useful at ingestion, dataset, tokenizer, transformer prototype, and validation boundaries. Runtime tensors are the hot-path contract for backend execution, training state, and checkpoint ownership.

Do not let these layers drift. Every conversion between them must name the synchronization point, materialization cost, contiguity requirement, and owner after conversion.

## 3D Runtime Projection

A 3D runtime tensor should preserve execution meaning:

- `SizeX`: batch, row, or X dimension;
- `SizeY`: feature, class, sequence, column, or Y dimension;
- `SizeZ`: channel, sequence, shared dimension, depth, or Z dimension.

Common projections:

- dense activations: `batch x features x 1`;
- token sequences: `batch x sequence x features` or `batch x sequence x 1` for ids;
- logits: `batch x classes x sequence` when token-wise class scores are needed;
- volumes: `x x y x z`;
- parameters: shape them by operation, not by source declaration, for example `input x output x 1` for dense weights;
- gradients and optimizer state: mirror the parameter shape.

Flatten only after recording the original semantic axes and the rule used to recover dispatch dimensions.

## Arena Ownership

Runtime tensor slices should expose:

- flat offset;
- element length;
- 3D dimensions;
- chunk count or allocation footprint;
- precision;
- role: input, target, parameter, activation, gradient, accumulator, optimizer state, scratch, metric, or diagnostic;
- lifetime: per model, per batch, per epoch, temporary, checkpoint-only, or diagnostic-only.

Prefer arena allocation for all hot-path tensors. Hidden arrays inside layers are acceptable only for prototype CPU code or immutable ingestion artifacts, and should not be confused with runtime state.

## Command Compatibility

Backend commands should be executable from offsets and dimensions:

- commands reference tensor slices by offset, not object graph traversal;
- `SizeX`, `SizeY`, and `SizeZ` must carry dispatch or math shape, not vague metadata;
- flags must describe transpose, broadcast, or layout modifiers;
- command arrays should be compiled once per stable shape and reused until shape or topology changes.

If an operation cannot be described this way, classify it as prototype-only, CPU-side, or requiring a new command/kernel contract.

## Review Questions

- Can every hot tensor be identified by role, owner, shape, precision, and lifetime?
- Does every CPU/GPU synchronization point have a reason?
- Can backend execution run from arena slices and commands without reconstructing layers?
- Are batch-size changes handled by explicit resize/reallocation policy?
- Are semantic shape transforms separated from runtime execution layout?
