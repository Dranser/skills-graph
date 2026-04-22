# Change Batching

## Purpose

Use this reference when a pipeline receives many small local updates and the next problem is how to aggregate them before driving invalidation and rerun.

## Core Rules

- batch derived update work, not source truth
- define merge and flush rules explicitly
- keep latency within the owning phase budget
- measure backlog and batch overhead

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Many tiny local updates trigger too many reruns | Add a batched change-set path | Rerun overhead can exceed useful work. |
| Newer changes supersede older ones | Merge them instead of queueing all versions | Coalescing prevents pointless backlog growth. |
| Responsiveness is sensitive | Use smaller batch windows or phase-bound flushes | Batching must not hide unacceptable latency. |
| Structural changes accumulate | Flush to a broader rerun path | Fine-grained batching should not absorb changes it cannot model cleanly. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| No flush discipline | Backlog grows with no predictable latency | Add explicit phase, size, or time triggers. |
| Queue everything forever | Old changes survive after newer ones supersede them | Introduce merge semantics. |
| Batch by intuition | Responsiveness regresses under burst load | Measure latency and backlog under representative load. |
| Batching bypasses invalidation | Derived products update through hidden patching | Route batch output through the normal incremental recompute model. |

## Review Playbook

1. Define change kinds and mergeability.
2. Define flush triggers.
3. Route batched output into incremental recompute.
4. Measure backlog and latency.
5. Keep a broad-rerun fallback for structural cases.
