# Incremental Recompute

## Purpose

Use this reference when a pipeline already has retained products and the next architectural question is how local changes should trigger bounded partial reruns instead of broad full recompute.

## Core Rules

- represent change sets explicitly
- map change kinds to invalidated products
- rerun only the necessary stages
- keep a full-rerun fallback for structural changes

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Small local changes dominate runtime behavior | Add a change-set driven rerun path | Broad reruns waste the value of retained products. |
| Change bursts can still spike the runtime | Add bounded batching or phase budgets | Incremental does not automatically mean stable. |
| A change affects broad structure or ownership | Fall back to a full rerun | Structural changes should not be forced through brittle local patch rules. |
| Dirty propagation cannot explain which products changed | Stop and clarify invalidation first | Incremental recompute needs explicit product-level dirtiness. |

## Ownership Heuristics

Incremental recompute should own:
- change-set representation
- minimal rerun paths
- fallback from local rerun to full rerun

Incremental recompute should not own:
- initial stage decomposition
- scheduler mechanics without pipeline products
- source-domain semantics outside the pipeline

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Hidden change propagation | Downstream state mutates without a visible cause | Represent change sets explicitly at pipeline entry. |
| Incremental path with no fallback | Structural changes break correctness or explode complexity | Keep a deliberate full-rerun path. |
| Local rerun logic with no budget | Incremental bursts still produce spikes | Add batching or a phase budget. |
| Patch-everything approach | Source-versus-derived ownership becomes blurry | Recenter on retained products and stage-owned outputs. |

## Review Playbook

1. Define the change-set shape.
2. Map each change kind to invalidated products.
3. Define the minimal rerun path.
4. Add bounded execution if bursts are possible.
5. Verify full rerun remains a clean fallback.
