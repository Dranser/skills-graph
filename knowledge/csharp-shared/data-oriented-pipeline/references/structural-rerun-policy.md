# Structural Rerun Policy

## Purpose

Use this reference when a pipeline already supports local reruns and the next question is when broad recompute should happen deliberately.

## Core Rules

- define what counts as structural change
- keep broad rerun as an explicit fallback path
- separate topology changes from ordinary dirty products
- escalate through named triggers or thresholds

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| A change affects graph topology or schema broadly | Trigger structural rerun | Local repair rules become brittle quickly. |
| Many local changes accumulate beyond a clear threshold | Escalate to broad rerun explicitly | Complexity should not grow silently. |
| Ordinary local dirty handling is still correct and cheap | Stay incremental | Broad rerun should not dominate by habit. |
| Broad rerun cost is too high to tolerate | Revisit architecture or batching | Hiding the cost in invalidation logic is not a real fix. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Hidden full recompute | The pipeline sometimes rebuilds broadly for unclear reasons | Name the structural triggers explicitly. |
| Structural change disguised as local dirty | Repair logic becomes brittle and opaque | Separate topology-aware fallback from ordinary dirtiness. |
| No escalation threshold | Incremental logic grows without bound | Add explicit broad-rerun triggers or thresholds. |
| Broad rerun embarrassment | Designers avoid the fallback even when it is clearly better | Treat structural rerun as a first-class architectural path. |

## Review Playbook

1. Name structural change kinds.
2. Define broad-rerun triggers.
3. Separate them from ordinary local invalidation.
4. Define any escalation thresholds.
5. Verify the fallback stays explicit and correct.
