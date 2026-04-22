# Dataflow State

## Purpose

Use this reference when the system must distinguish source truth from retained derived pipeline products.

## Core Rules

- keep source truth authoritative
- make derived products explicit
- retain only what has a clear reuse benefit
- give every retained product an invalidation rule

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Source data changes rarely but many reads consume a derived view | Retain the derived product | Reuse beats rerun. |
| Derived view is cheap and seldom reused | Recompute it | Retention cost may not pay off. |
| Cached output lacks invalidation policy | Redesign before retaining it | Stale derived state will undermine correctness. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Derived state becomes source truth | Ownership and invalidation become confused | Re-separate source input from derived outputs. |
| Retained products without keys | Reuse and invalidation become broad and fragile | Add stable identities and ownership. |
| Everything is retained | Memory and invalidation costs balloon | Keep only products with real reuse value. |

## Review Playbook

1. Name source truth.
2. Name every derived product.
3. Mark whether it is retained or recomputed.
4. Mark its invalidation trigger.
5. Verify downstream consumers depend on the right product.
