# Stage Boundaries

## Purpose

Use this reference when deciding where one stage ends and another begins.

## Core Rules

- split around owned products
- split where recompute scope changes
- avoid boundaries that are only conceptual labels

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A reusable intermediate product appears | Promote it to a stage output | That product becomes a real boundary. |
| Downstream stages can stay valid after a local change elsewhere | Separate the boundary | Recompute can stop earlier. |
| A stage only forwards mutable state around | Reconsider the split | No real ownership exists yet. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Over-splitting | Many tiny stages with no useful products | Merge until ownership becomes meaningful. |
| Under-splitting | One stage owns too many recompute regions | Split by data dependency and output reuse. |
| Mutable product shared everywhere | Boundaries exist but recompute remains broad | Reintroduce explicit ownership and narrower contracts. |

## Review Playbook

1. List transformations.
2. Group them by shared inputs and outputs.
3. Name the stage output for each group.
4. Verify that each split reduces ambiguity or recompute scope.
