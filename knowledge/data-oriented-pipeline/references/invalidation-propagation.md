# Invalidation Propagation

## Purpose

Use this reference when local changes should trigger only local recompute.

## Core Rules

- mark only affected products dirty
- stop at the first clean boundary
- keep propagation logic explicit

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| One source field affects one downstream product | Invalidate only that product path | Whole-pipeline rerun is wasteful. |
| Global state changes structure or semantics broadly | Use broad invalidation deliberately | The scope is legitimately wide. |
| Dirty logic costs more than rerun | Simplify or broaden carefully | Over-engineered invalidation defeats its purpose. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Everything dirty by default | Pipeline gains vanish | Narrow invalidation to owned outputs. |
| Hidden invalidation through shared mutation | Dirty reasoning becomes opaque | Replace with explicit propagation. |
| Excessively expensive dirty checks | CPU moves from recompute into bookkeeping | Use cheaper signals or broader but simpler boundaries. |

## Review Playbook

1. Pick one local source change.
2. Trace which products actually become invalid.
3. Stop at the first product that remains valid.
4. Verify siblings stay clean.
