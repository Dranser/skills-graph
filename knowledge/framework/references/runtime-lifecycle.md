# Runtime Lifecycle

## Purpose

Use this reference when the main framework problem is not startup itself but the host lifecycle that continues after startup.

## Core Rules

- define explicit runtime phases beyond bootstrap
- make steady-state update order reviewable
- keep shutdown order deterministic
- avoid hidden lifecycle callbacks spread across feature code

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A host runs recurring systems after startup | Define named runtime phases with explicit ownership | Recurring work needs inspectable lifecycle boundaries. |
| Systems start cleanly but teardown is fragile | Define explicit shutdown order and dependency release | Teardown bugs usually expose hidden lifecycle coupling. |
| Feature modules register their own update hooks | Pull lifecycle control back to the host | Hidden callbacks weaken determinism and reviewability. |
| Suspend or resume behavior is emerging ad hoc | Add explicit transition phases and contracts | Lifecycle transitions should not be implied by side effects. |

## Ownership Heuristics

Runtime lifecycle should own:
- steady-state phase boundaries
- update ordering policy
- suspend, resume, and shutdown transitions

Runtime lifecycle should not own:
- one feature's local business logic
- startup composition root wiring
- low-level performance tuning detached from lifecycle structure

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Bootstrap owns everything forever | Entrypoint turns into a permanent runtime coordinator | Split ongoing orchestration into explicit lifecycle phases. |
| Hidden update callbacks | Runtime order can only be learned by tracing code | Re-centralize lifecycle ownership in the host. |
| Shutdown by side effect | Disposal order bugs appear late and unpredictably | Define explicit teardown sequence and dependencies. |
| Implicit suspend or resume behavior | State recovery differs between systems | Add explicit transition contracts. |

## Review Playbook

1. Name the runtime host.
2. List startup, steady-state, and shutdown phases separately.
3. Mark which systems may run in each phase.
4. Verify transitions and teardown order are explicit.
5. Check that feature code is not secretly owning lifecycle.
