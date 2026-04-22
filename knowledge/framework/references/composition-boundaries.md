# Composition Boundaries

## Purpose

Use this reference when the framework problem is not one local object graph but where larger runtime areas should compose.

## Core Rules

- define explicit seams between host and modules
- separate contract ownership from wiring ownership
- keep composition visible at the boundary
- do not let container access replace a real seam

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A host integrates multiple runtime modules | Define a named composition seam per module relationship | Larger systems need explicit integration ownership. |
| Contract ownership and wiring ownership differ | State the split explicitly | Hidden ownership drift weakens architecture. |
| Modules can only integrate through host setup internals | Redesign the seam | Internal reach-through is not a stable boundary. |
| Registration logic is scattered everywhere | Pull it back to explicit composition boundaries | Sprawl hides how the runtime actually composes. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Container-as-boundary | Modules integrate by resolving ambient services | Define a real seam with owned contracts. |
| Registration sprawl | Integration logic is spread across unrelated files | Re-center wiring at explicit boundaries. |
| Host-internal reach-through | Modules know too much about setup internals | Narrow the seam and move internals back behind it. |
| Unnamed ownership split | No one can say who owns contracts versus lifecycle entry | State ownership explicitly at the seam. |

## Review Playbook

1. Name the host and module sides.
2. Define the shared contract surface.
3. Define who owns wiring and lifecycle entry.
4. Pull scattered integration back to the seam.
5. Verify the seam is reviewable.
