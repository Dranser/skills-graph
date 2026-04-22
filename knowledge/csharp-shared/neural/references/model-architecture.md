# Neural Model Architecture

Model architecture in this domain is designed around tensor contracts first and layer objects second.

## Layer Contract

For each layer, record:

- input tensor roles and 3D runtime shapes;
- output tensor roles and 3D runtime shapes;
- parameter tensor shapes and precision;
- gradient tensor shapes;
- scratch tensor needs;
- forward command or kernel sequence;
- backward command or kernel sequence if trainable;
- batch-size resize behavior;
- prototype/runtime status.

This keeps layers reviewable even when the implementation is custom and does not use a framework graph.

## Runtime-Backed vs Prototype

Classify layer code explicitly:

- runtime-backed: uses arena slices and backend-executable commands/kernels;
- CPU prototype: uses semantic tensors, arrays, or spans for correctness exploration;
- hybrid: has a runtime-backed subset and CPU-side boundary work.

Do not benchmark CPU prototype costs as GPU backend costs. Do not assume a CPU prototype is ready for training throughput until tensor roles, commands, and residency are defined.

## Forward Graph

Forward execution should be deterministic:

- stable layer order;
- stable tensor ownership;
- stable command compilation when shape is unchanged;
- explicit invalidation when batch size, topology, parameter shape, or activation shape changes.

Avoid reflection-driven graph discovery or mutable collection order as execution policy.

## Attention And Sequence Models

Attention and transformer components need clear axis policy:

- batch;
- sequence;
- model dimension;
- head count;
- head dimension;
- vocabulary/classes;
- mask shape.

Before moving attention into runtime execution, define how these axes map into 3D slices and which operations need new backend commands: projections, score matmul, mask application, softmax, context matmul, output projection, normalization, residual add, and feed-forward blocks.
