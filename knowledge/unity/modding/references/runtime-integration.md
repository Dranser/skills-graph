# Runtime Integration

Modding a Unity game should turn unstable game internals into explicit, diagnosable contracts.

## Shape

Use a layered integration model:

1. Loader entry attaches the runtime and diagnostics.
2. Patch layer observes small game lifecycle anchors.
3. Hook handlers translate game calls into internal events.
4. Reflection probes read unstable state behind narrow adapters.
5. Runtime modules consume events and probes.
6. Public facades expose stable snapshots and operations to feature code.

## Rules

- Keep Harmony patches thin.
- Move feature logic out of patch classes.
- Prefer postfix observation over mutation when observation is enough.
- Centralize reflection and log degradation once per missing member.
- Keep startup, update, fixed update, GUI, and shutdown explicit.
- Expose game state as snapshots or facades instead of leaking raw internals everywhere.

## Diagnostics

A production modding toolkit should report hook availability, reflection adapter fallback, module state, queue pressure, sync state, and lifecycle phase. Missing hooks should degrade features instead of crashing unrelated systems.
