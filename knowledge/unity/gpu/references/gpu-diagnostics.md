# GPU Diagnostics

GPU diagnostics should reveal pipeline health without introducing synchronization stalls.

## Signals

- stage dispatch counts;
- spawned, valid, visible, and rejected instance counts;
- buffer allocation and resize events;
- missing kernel or shader binding failures;
- readback in flight state;
- indirect args validity;
- culled or unloaded chunk state.

## Readback Policy

Prefer GPU-side counters plus throttled `AsyncGPUReadback`. Avoid blocking readback in hot paths. Treat readback as diagnostics, not as required frame-to-frame control flow, unless correctness genuinely depends on CPU-visible data.

## Failure Policy

Missing kernels, buffers, or material bindings should fail clearly and early. Optional diagnostics should degrade silently after recording the reason.
