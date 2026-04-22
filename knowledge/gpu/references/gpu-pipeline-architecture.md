# GPU Pipeline Architecture

GPU pipeline architecture defines the contract between CPU orchestration and GPU execution.

## Shape

A robust GPU pipeline names each stage, each buffer, and each ownership boundary:

- CPU orchestration owns resource lifetime, dispatch scheduling, feature toggles, and diagnostics policy;
- GPU kernels own bulk parallel transforms over explicitly shaped buffers;
- each stage consumes and produces named buffers;
- dirty flags or stage state decide which stages rerun;
- readback is optional, bounded, and asynchronous unless correctness requires otherwise.

## Stage Contract

For every stage, record:

- input buffers and parameters;
- output buffers and counters;
- dispatch dimensions or indirect args source;
- state or dirty bit consumed and cleared;
- downstream stages invalidated by the output;
- diagnostics counters or validation checks.

## Review

Reject GPU designs that hide synchronization, rebuild all buffers for small changes, read back counts in hot paths, or leave draw/dispatch args as CPU guesses when the GPU already owns the count.
