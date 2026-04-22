# IMGUI Debugging

## Purpose

Use this reference to diagnose framework-level IMGUI issues without immediately rewriting the architecture.

## Where To Instrument

Start at boundaries:
- host
- window
- panel

Only then go deeper into leaf widgets.

## What To Trace

Trace compactly:
- current event type
- active window or panel
- focus owner
- hot control owner
- whether the event was consumed
- repaint count
- layout count

## Debug Overlay Guidance

If adding a visual overlay:
- keep it passive
- do not let it become a hidden input owner
- use it to show retained state and ownership, not decorative noise

## Common Problem Classes

Focus bugs:
- trace focus owner transitions
- compare host decision and panel-local decision

Layout mismatch:
- compare structural branches between layout and repaint
- inspect optional slots and repeated sections first

Event loss:
- identify the first boundary that consumes the event unexpectedly

## Review Questions

1. Can ownership transitions be seen at host boundaries?
2. Can consumed events be traced back to one clear owner?
3. Are repaint or layout spikes visible?
4. Does diagnostics stay passive relative to event flow?

## Triage Table

| Symptom | First place to inspect | Likely cause |
| --- | --- | --- |
| Keyboard focus jumps unpredictably | Host focus ownership trace | Competing owners or hidden focus transitions. |
| Shortcut stops working | Host command routing then active panel | Event consumed too early or command ownership is unclear. |
| Layout mismatch error | Optional slots and repeated sections | Structure differs between layout and repaint. |
| Repaint spike | Host repaint counters then always-visible panels | Repeated expensive draw work or unstable structure. |
| Click disappears | Boundary where event becomes consumed | Hidden event ownership or leaf widget overreach. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Logging only leaf widgets | Noise without ownership clarity | Move diagnostics upward to host and panel boundaries. |
| Debug overlay consumes input | New bugs appear during debugging | Keep overlays passive and separate from command ownership. |
| Tracing everything | No signal in the output | Trace only owners, transitions, and counters that explain control flow. |
| Rewriting architecture before tracing | Expensive guesswork | Add boundary-level instrumentation first. |

## Review Playbook

When debugging an IMGUI framework issue:
1. Name the symptom in one sentence.
2. Identify the highest boundary that should own it.
3. Instrument that boundary first.
4. Move inward only if the boundary behaves correctly.
5. Fix ownership before adding more abstraction.
