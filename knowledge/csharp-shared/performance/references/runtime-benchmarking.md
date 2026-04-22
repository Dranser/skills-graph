# Runtime Benchmarking And Workloads

Runtime benchmarking needs both controlled stress and realistic workload probes.

## Baselines

Keep these comparison points:

- single-thread reference;
- balanced worker policy;
- throughput-oriented worker policy;
- long stable run;
- burst command-pressure run;
- realistic module workload;
- synthetic stress workload.

Single-thread mode is not scaffolding. It is the correctness and profiling baseline.

## Metrics

Capture:

- phase timings;
- task timings;
- worker utilization;
- planning cost;
- reduction commit cost;
- dispatch count;
- chunk count;
- worker imbalance;
- queue depth or backlog;
- storage growth and compaction;
- validation failures;
- published payload preparation and submit cost;
- worst-case and tail latency, not only averages.

## Synthetic Workload

Synthetic load is useful for shaping scheduler behavior because it can control:

- element count;
- minimum chunk size;
- iterations per element;
- reduction cost;
- serial tail cost;
- burst mode;
- fixed versus variable phase placement.

Do not confuse synthetic scaling with product readiness.

## Realistic Workload Probe

A realistic module should stress:

- snapshot capture;
- owned range writes;
- reductions;
- deterministic publication;
- host output consumption;
- diagnostics;
- domain enough behavior to reveal real access patterns.

For a simulation runtime, a gameplay-facing engine testbed or similar subsystem is more useful than abstract math loops alone.

## Interpretation

When results are poor, classify the bottleneck before changing architecture:

- too much main-thread or submit work;
- poor data layout;
- too-small task granularity;
- range imbalance;
- reduction or barrier cost;
- contention;
- storage churn;
- diagnostics overhead;
- host boundary leakage.

Optimization should follow the classification.
