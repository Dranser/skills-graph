# GPU Buffer Contracts

Buffers are the stable ABI between CPU code, compute kernels, and draw shaders.

## Required Contract

For each buffer define:

- semantic role;
- element type;
- count policy;
- stride and memory layout;
- target type such as structured, raw, append, or indirect arguments;
- owner responsible for allocation and release;
- resize policy;
- clear or counter reset policy;
- producer and consumer stages.

## Common Roles

- `SpawnInput`: generated candidate input for later stages.
- `InstanceData`: canonical per-instance state.
- `VisibleIndices`: append buffer containing compact visible instance ids.
- `InputCountBuffer`: raw count copied from an append counter.
- `DispatchArgs`: indirect dispatch dimensions.
- `DrawArgs`: indirect draw arguments.
- `DebugCounters`: optional counters for async diagnostics.

## Rules

Keep CPU and shader struct layouts synchronized. Bind safe dummy buffers when shaders require buffers before real data exists. Prefer buffer reuse over release/recreate once counts stabilize.
