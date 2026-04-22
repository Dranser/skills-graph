# Memory Arenas

## Purpose

Use this reference when many temporary allocations belong to one pass or phase and should die together at one explicit reset boundary.

## Core Rules

- use arenas for grouped temporary lifetime
- reset at explicit phase or pass boundaries
- keep long-lived outputs out of the arena
- observe peak capacity instead of letting burst size become permanent

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| One pass allocates many temporary buffers that die together | Use a scratch arena | Grouped lifetime is clearer and cheaper than many tiny transient allocations. |
| Data must survive beyond the reset boundary | Promote it out explicitly | Arena lifetime should stay strict. |
| Temporary memory crosses phases unpredictably | Do not force one arena | The grouped lifetime assumption is false. |
| Peak capacity spikes once and never shrinks | Add capacity discipline or reset policy | Scratch memory should not become accidental retained state. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Shared global arena | Ownership becomes ambiguous across systems | Scope the arena to one pass or owner. |
| Escape from arena lifetime | Use-after-reset or hidden retained state | Promote persistent outputs explicitly. |
| Arena for everything | Long-lived and short-lived memory blur together | Keep arenas for grouped temporary lifetime only. |
| Permanent peak retention | One burst leaves scratch memory oversized forever | Add observation and capacity discipline. |

## Review Playbook

1. Name the phase or pass.
2. Define the arena owner.
3. Define the reset boundary.
4. Mark what may escape and how.
5. Measure peak capacity and reset behavior.
