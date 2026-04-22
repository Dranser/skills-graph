# Framework Diagnostics

## Purpose

Use this reference when the framework needs early startup visibility, compact runtime traces, or durable failure reporting.

## Core Rules

- initialize diagnostics early
- prefer file-backed logging over console-only output
- keep messages compact and structured
- keep the diagnostics interface minimal

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Startup failures matter | Initialize diagnostics before optional systems | Early visibility is required. |
| Console output is unreliable or secondary | Use a durable file-backed sink | Diagnostics should survive outside console presence. |
| Logging API starts to expand too broadly | Keep only essential capabilities | Early infrastructure must remain lightweight. |
| Failure path depends on optional runtime systems | Remove that dependency | Diagnostics must remain available under failure. |

## Sink Heuristics

Prefer the primary sink to be:
- durable
- available early
- compact to inspect

Treat console output as secondary when:
- the runtime host may not expose a useful console
- startup failures must survive after process exit
- repeated sessions need durable trace history

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Console-only logging | Important failures disappear in some runtime contexts | Add a durable primary sink. |
| Late diagnostics initialization | Startup issues remain invisible | Move diagnostics earlier in bootstrap. |
| Heavy diagnostics stack in early startup | Bootstrap becomes fragile and overloaded | Keep diagnostics minimal and compact. |
| Overly verbose logging | Important failures drown in noise | Keep messages compact and structured. |

## Escalation Rules

Escalate from diagnostics to supporting references when:
- startup ordering is the real issue: use `bootstrap.md`
- the logging boundary or capability surface needs refinement: use `contracts.md`
- diagnostics services must be composed with the wider service graph: use `di.md`

## Review Playbook

1. Identify the earliest moment diagnostics can start.
2. Verify the primary sink is durable.
3. Verify messages stay compact and structured.
4. Verify failures can be recorded without optional systems.
5. Verify bootstrap initializes diagnostics before broader startup.

## Review Matrix

| Review question | Healthy answer |
| --- | --- |
| Is diagnostics available before optional startup work? | Yes, early failures can be recorded. |
| Is the primary sink durable? | Yes, trace output survives outside ephemeral console state. |
| Are messages compact enough to inspect quickly? | Yes, failure signal is not buried in noise. |
