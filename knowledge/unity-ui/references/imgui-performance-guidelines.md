# IMGUI Performance Guidelines

## Purpose

Use this reference when the framework is functionally correct but risks paying unnecessary cost on every IMGUI pass.

## Hot-Path Mindset

Treat `OnGUI` code as hot-path code by default, especially for:
- debug overlays
- always-visible tool panes
- inspector-like windows that repaint often

## Main Cost Sources

Watch for:
- repeated string formatting
- repeated `GUIContent` creation
- repeated `GUIStyle` creation or cloning
- unstable layout structure
- unnecessary traversal of large view models every pass

## Preferred Mitigations

Prefer:
- host-owned caches with explicit invalidation
- precomputed labels for repeated content
- stable panel structure
- small draw helpers with explicit data access
- subtree-level dirty boundaries for local refresh
- binding checks that are cheaper than full recomputation

Avoid:
- caching with unclear ownership
- widget-local caches everywhere
- hiding performance problems behind more abstraction

## Style-Specific Note

Styling is a performance concern too:
- semantic styles are easier to cache and review
- ad hoc style construction in leaf widgets multiplies hot-path cost

## Review Questions

1. What allocates every pass?
2. Which labels or content repeat unchanged?
3. Which panels repaint most often?
4. Is the cache invalidation rule explicit?
5. Did optimization preserve readable event flow?

## Decision Table

| Cost source | First response | Escalation |
| --- | --- | --- |
| Repeated string formatting | Precompute or cache labels | Revisit data flow if formatting is scattered across features. |
| Repeated `GUIContent` creation | Host or panel-owned cache | Add invalidation keyed to source data changes. |
| Repeated `GUIStyle` setup | Centralize semantic styles | Review styling architecture if style variants are too dynamic. |
| Expensive repaint on always-visible panel | Simplify draw flow first | Add targeted caches only after the flow is stable. |
| Unstable layout structure | Stabilize composition | Revisit manual layout and panel slot ownership. |
| Small state change in one panel still refreshes everything | Add subtree dirty markers | Revisit binding boundaries and invalidation scope. |
| Expensive derived resource generation | Generate once and reuse | Add explicit invalidation keyed to source versions. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Widget-local caches everywhere | Cache invalidation is impossible to reason about | Consolidate caches at host or panel boundaries. |
| Performance fix adds hidden control flow | Profiling gets harder and bugs become opaque | Prefer simpler draw logic and explicit invalidation. |
| Caching before stabilizing structure | Fast but brittle code, stale visuals, unclear ownership | Freeze structure and state model first. |
| Blaming IMGUI globally | Optimization work is unfocused | Isolate the concrete hot panel, loop, or content path. |
| Whole-tree invalidation as the default | Most updates cost far more than needed | Push invalidation down to panel or component subtree boundaries. |
| Binding checks are heavier than redraw | CPU cost shifts but does not improve | Simplify the binding model or compare cheaper versions instead of deep values. |

## Review Playbook

Use this order when performance work starts:
1. Identify the hottest panel or host.
2. List allocations and repeated formatting.
3. Remove obviously repeated work.
4. Check whether the update can stop at a subtree boundary.
5. Add only the smallest cache or binding check with explicit ownership.
6. Re-check that event flow, layout ownership, and invalidation flow remain readable.

## Optimized Model Checklist

The framework should be able to answer yes to all of these:
1. Are expensive resources generated once and reused?
2. Can unchanged data skip binding work?
3. Can a local state change mark only the affected subtree dirty?
4. Can clean subtrees avoid presentation recomputation?
