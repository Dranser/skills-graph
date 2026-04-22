# Reverse Integration Map

Use assembly analysis to produce stable runtime integration knowledge, not only decompiled text.

## Purpose

The useful output of reading a Unity game assembly is an integration map:

- candidate types and methods to observe or patch;
- lifecycle anchors and their expected order;
- public API surfaces that are stable enough to call directly;
- private fields and methods that require reflection adapters;
- state objects that should be represented through snapshots or facades;
- uncertainty notes for names, signatures, overloads, and version-sensitive behavior.

## Workflow

1. Start with raw type and member inventory.
2. Group findings by runtime concern: loading, car lifecycle, race lifecycle, menu, network, UI, telemetry, persistence.
3. Identify observable anchors before mutation anchors.
4. Prefer methods that already represent lifecycle transitions over broad update loops.
5. Record the exact type name, member name, signature, patch mode, and expected payload.
6. Mark every private or reflected member as unstable until runtime probes confirm it.
7. Convert the map into modding contracts: hook registrations, probe adapters, events, snapshots, and public facades.

## Review

Good analysis keeps raw evidence and interpretation separate. A decompiled method can explain intent, but the integration contract should name what the mod needs to observe, what it can safely mutate, and what must degrade when the game changes.
