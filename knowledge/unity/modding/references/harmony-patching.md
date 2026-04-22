# Harmony Patching

Harmony should be used as a narrow interception layer, not as the architecture of the mod.

## Patch Selection

Prefer targets that represent semantic lifecycle events:

- object loaded;
- context activated or deactivated;
- loading started or finished;
- race started or completed;
- profile applied;
- menu opened or closed.

Prefer postfix for observation. Use prefix when the mod must guard, redirect, or capture state before the game changes it. Treat transpilers as last resort because they are harder to review and more fragile across game updates.

## Patch Class Contract

A patch class should:

- locate the target type and method;
- install or remove the patch;
- call a handler with the minimal payload;
- report whether the hook is active.

It should not contain feature policy, UI work, large state machines, or unrelated mutations.

## Review

Every patch needs a reason, a target signature, a fallback behavior when missing, and a diagnostic path that can prove whether the patch is active at runtime.
