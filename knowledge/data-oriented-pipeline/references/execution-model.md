# Execution Model

## Purpose

Use this reference when choosing how a data-oriented pipeline actually runs once stages are defined.

## Core Rules

- derive execution order from dependencies
- parallelize only where ownership and dataflow permit
- keep dispatch comprehensible

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| Stage outputs depend strictly on previous stage results | Keep sequential order | Dependency dominates. |
| Two regions have independent owned products | Consider parallel execution | Safe boundaries may permit concurrency. |
| Shared mutable ownership remains unclear | Do not parallelize yet | Scheduler-first design would hide structural problems. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Scheduler-first architecture | Dispatch exists before clean stage design | Return to stage boundaries and ownership first. |
| Parallelism across unclear ownership | Contention and correctness bugs appear | Rework boundaries before dispatch. |
| Opaque dispatch graph | Profiling can no longer explain runtime cost | Simplify execution regions and keep them explicit. |

## Review Playbook

1. Draw stage dependencies.
2. Mark sequential dependencies.
3. Mark independent regions.
4. Verify ownership is safe before parallelization.
5. Confirm dispatch still profiles clearly.
