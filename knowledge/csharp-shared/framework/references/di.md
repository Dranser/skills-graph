# Framework DI

## Purpose

Use this reference when the task is about service graph composition, explicit registration, or lifetime policy.

## Core Rules

- keep container ownership at composition root
- register contracts before consumers resolve them
- define lifetimes explicitly
- avoid service locator behavior in feature code

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Many runtime systems collaborate through contracts | Use explicit registration at composition root | Composition stays reviewable. |
| A dependency is purely local and simple | Construct it directly | Container indirection may add no value. |
| Lifetime is unclear | Define it before registration | Hidden lifetime policy causes bugs later. |
| Feature code wants direct container access | Reject it | Service locator hides dependencies. |

## Lifetime Heuristics

Prefer longer-lived services only when:
- they genuinely match host lifetime
- shared state must persist across many consumers
- cleanup boundaries are explicit

Prefer shorter-lived or direct construction when:
- the object is local to one feature path
- no cross-boundary composition is needed
- container ownership would obscure the simpler design

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Global container access | Dependencies disappear from constructors and become hidden | Keep container ownership at composition root only. |
| Registration side effects | Startup behavior becomes hard to reason about | Make registration declarative and move behavior elsewhere. |
| Lifetime mismatch | Services persist too long or disappear too early | Align lifetimes with host lifecycle. |
| Container everywhere by default | Simple code becomes over-composed and harder to trace | Use direct construction where no real boundary exists. |

## Escalation Rules

Escalate from DI to supporting references when:
- the real question is boundary shape: use `contracts.md`
- the real question is startup order: use `bootstrap.md`
- the real question is early logging or trace visibility: use `diagnostics.md`

## Review Playbook

1. List the contracts being composed.
2. Assign implementations and lifetimes.
3. Register core services before dependents.
4. Verify no feature code uses service locator access.
5. Verify lifetime policy matches the host lifecycle.

## Review Matrix

| Review question | Healthy answer |
| --- | --- |
| Is container ownership restricted to composition root? | Yes, feature code does not pull from a global container. |
| Are lifetimes aligned to host lifecycle? | Yes, each registration has an explicit reason to live as long as it does. |
| Would direct construction be clearer anywhere? | Yes, simple local graphs are not forced through the container. |
