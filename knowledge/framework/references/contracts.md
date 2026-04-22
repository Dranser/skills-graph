# Framework Contracts

## Purpose

Use this reference when runtime boundaries, extension points, or capability-oriented interfaces are the main design concern.

## Core Rules

- define contracts before wiring implementations
- keep interfaces minimal
- expose capabilities, not internals
- split read and mutate responsibilities when they change for different reasons

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Consumer needs one narrow capability | Define a small capability-focused contract | Broad interfaces leak implementation detail. |
| Read and mutate paths evolve differently | Split the boundary | Separate reasons to change. |
| Extension point is needed | Publish a dedicated extension contract | Hidden side channels weaken architecture. |
| Interface only mirrors one class | Reconsider whether the abstraction is real | One-to-one mirroring often has no runtime value. |

## Ownership Heuristics

Contracts should own:
- capability boundaries
- extension points
- read/write responsibility splits

Contracts should not own:
- registration order
- startup policy
- container-specific mechanics

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Speculative abstraction | Many interfaces exist with no meaningful consumers | Remove or narrow the abstraction. |
| Implementation-shaped contracts | Consumers still know too much about concrete types | Redesign around actual required capability. |
| Service locator disguised as contract | Boundaries exist but dependencies remain hidden | Restore explicit composition and dependency declaration. |
| Boundary that mixes unrelated concerns | One interface changes for too many reasons | Split by capability or by read/write responsibilities. |

## Escalation Rules

Escalate from contracts to supporting references when:
- service registration and lifetimes dominate: use `di.md`
- startup sequencing dominates: use `bootstrap.md`
- diagnostics capability boundaries are the concrete use case: use `diagnostics.md`

## Review Playbook

1. Name the capability the consumer actually needs.
2. Remove responsibilities the consumer does not need.
3. Check whether read and write behavior should split.
4. Verify the contract does not leak implementation internals.
5. Bind the implementation later in bootstrap or DI.

## Review Matrix

| Review question | Healthy answer |
| --- | --- |
| Does the contract expose only required capability? | Yes, consumers do not need implementation detail. |
| Is the abstraction justified by a real boundary? | Yes, it exists to support composition or extension. |
| Is service locator behavior absent? | Yes, the contract does not hide dependency discovery. |
