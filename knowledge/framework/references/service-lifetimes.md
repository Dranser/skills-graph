# Service Lifetimes

## Purpose

Use this reference when the framework question is not just how to register services, but how long they should live and which boundary owns that duration.

## Core Rules

- treat lifetimes as ownership boundaries
- align lifetime policy with real runtime phases or scopes
- prevent long-lived services from capturing short-lived state
- make scope meaning explicit

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A service lives for the whole host runtime | Consider singleton only if it does not depend on shorter-lived state | Long-lived ownership should stay stable and safe. |
| A service belongs to one module, request, or runtime phase | Define a scoped lifetime tied to that real boundary | Scope should mean something operational. |
| A service is cheap and local with no retained ownership | Consider transient | Transient is for simple local lifetime, not for avoiding clarity. |
| Longer-lived code depends on shorter-lived state | Redesign the boundary or lifetime policy | Quiet capture creates structural lifetime bugs. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Default-singleton thinking | Long-lived services retain state they should not own | Re-map lifetime to the real boundary. |
| Scope with no runtime meaning | "Scoped" exists only in container config | Tie the scope to a real host or request boundary. |
| Hidden lifetime inversion | Bugs appear because shorter-lived state outlives its contract | Review dependency direction and redesign the seam. |
| Transient everywhere | Object churn hides missing lifetime decisions | Introduce explicit ownership where needed. |

## Review Playbook

1. Name the real runtime boundary for each service.
2. Map singleton, scoped, or transient intentionally.
3. Check for lifetime inversions.
4. Define disposal or reset behavior.
5. Verify scope meaning exists outside the container.
