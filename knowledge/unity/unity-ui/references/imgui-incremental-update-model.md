# IMGUI Incremental Update Model

## Purpose

Use this reference when the framework must be optimized from the start instead of relying on whole-UI recomputation.

This model assumes:
- retained state above IMGUI
- reusable generated resources
- binding-aware updates
- dirty propagation through subtree boundaries

## Core Rules

1. Generate expensive presentation resources once.
2. Reuse them until explicit invalidation.
3. Bind source data to retained presentation state.
4. Mark only the affected host, panel, or component subtree dirty.
5. Stop recomputation at the first clean boundary.

## Resource Categories

Common reusable resources:
- `GUIContent` objects
- derived display strings
- icon lookups
- semantic style lookups
- filtered or flattened presentation lists
- layout metadata that only changes when structure changes

If a resource is expensive to derive and changes infrequently, it should usually be cached behind explicit invalidation.

## Binding Model

Binding should answer one question cheaply:

Has the source data changed enough to justify recomputing derived presentation?

Typical strategies:
- version stamps
- last-seen IDs plus per-record version
- lightweight hashes
- explicit dirty notifications from the source model

Prefer the cheapest comparison that is still correct enough for the tool.

## Dirty Propagation Model

Use explicit boundaries:
- host
- window
- panel
- component subtree

State changes should flow like this:
1. source state changes
2. the nearest owning subtree is marked dirty
3. parent nodes know which child subtree is dirty
4. clean siblings avoid recomputation
5. draw uses already-valid derived resources for clean subtrees

## Decision Table

| Situation | Preferred response | Why |
| --- | --- | --- |
| Label text changes for one row | Invalidate that row's derived content only | Whole-list rebuild is unnecessary. |
| Selection changes and only one panel depends on it | Mark that panel subtree dirty | Other panels can stay clean. |
| Theme changes globally | Invalidate style-dependent resources at host scope | The change is legitimately broad. |
| Data set reorder affects visible list structure | Invalidate the owning list subtree | Structural derived data must refresh together. |
| Tool closes and reopens | Rebuild resources according to host lifetime policy | Lifetime boundary was crossed. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Everything is always dirty | Incremental model gives no win | Introduce narrower ownership and subtree boundaries. |
| Binding compares deep object graphs every pass | CPU cost remains high | Use version stamps or cheaper source signals. |
| Cached presentation becomes stale | UI no longer matches source state | Fix invalidation ownership and lifetime rules. |
| Resources are cached per widget ad hoc | Ownership and cleanup become chaotic | Consolidate caches at host, panel, or subtree boundaries. |

## Review Playbook

When designing or reviewing the incremental model:
1. List the expensive derived resources.
2. Mark who owns each one.
3. Mark what invalidates each one.
4. Mark the smallest subtree that depends on each source change.
5. Verify that clean siblings can remain untouched.

## Design Heuristics

Prefer:
- host or panel-owned caches
- source versions instead of deep equality checks
- subtree-level dirty boundaries
- explicit invalidation over implicit recomputation

Avoid:
- hidden binding magic
- framework-wide invalidation by default
- cache ownership split across many unrelated widgets
- optimization layers that obscure event and state ownership
