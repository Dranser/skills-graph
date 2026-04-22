# Extension Points

## Purpose

Use this reference when the framework must expose a host extension surface for modules or plugins without turning the host into a bag of leaked internals.

## Core Rules

- expose capability, not host internals
- keep registration explicit
- let the host own lifecycle
- define ordering and failure behavior

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A module needs to add one bounded capability | Publish a narrow extension contract | Broad plugin surfaces leak internals and become hard to version. |
| Extensions participate in runtime phases | Attach them through explicit lifecycle boundaries | The host must stay authoritative about when work may run. |
| Registration is becoming hidden discovery | Re-center registration at one explicit host boundary | Hidden discovery weakens reviewability and determinism. |
| One extension can fail independently | Define containment and diagnostics policy | Extensibility without failure policy becomes brittle fast. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Service-bag plugin API | Extensions depend on too much host knowledge | Narrow the contract to one real capability. |
| Hidden extension discovery | The active extension set is hard to inspect | Use explicit registration and composition boundaries. |
| Host lifecycle leakage | Extensions decide their own execution timing | Pull lifecycle control back to the host. |
| No failure policy | One broken extension destabilizes the whole host | Define containment, diagnostics, and ordering rules. |

## Review Playbook

1. Name the capability being extended.
2. Define the narrow extension contract.
3. Define explicit registration.
4. Define host-controlled lifecycle participation.
5. Verify failure and ordering behavior.
