# Stage Contracts

## Purpose

Use this reference when stages exist, but the products they consume and produce are still too vague, too broad, or too implicit.

## Core Rules

- define contracts around products, not generic methods
- name stage inputs and outputs explicitly
- keep internal scratch state local
- narrow dependency surfaces where possible

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Downstream stages consume vague broad state | Define named product contracts | Narrow contracts make coupling and recompute clearer. |
| Internal scratch data leaks across boundaries | Pull it back inside the stage | Working state should not become ambient contract surface. |
| One object is passed through many stages | Split it into explicit products | Broad mutable pass-through hides real ownership. |
| Contract changes keep cascading widely | Revisit product width and structural impact | Over-broad contracts destabilize the pipeline. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Method-shaped stage API | The code hides what data actually moves | Define product contracts explicitly. |
| Broad mutable pass-through | Many stages depend on too much state | Split products by owned output. |
| Scratch leakage | Internal work buffers become de facto contracts | Keep working state stage-local. |
| Contract sprawl | Downstream changes ripple too broadly | Narrow the contract or split the product. |

## Review Playbook

1. Name each stage input and output.
2. Separate products from scratch state.
3. Limit downstream dependence to named products.
4. Check contract width against recompute scope.
5. Escalate structural impact when contract changes are broad.
