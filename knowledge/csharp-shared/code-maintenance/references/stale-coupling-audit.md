# Stale Coupling Audit

Stale coupling is old code that still influences the project after its original feature is gone or changed.

## Common Hidden Links

Check:

- registration lists;
- dependency injection or service composition;
- Unity serialized fields and prefab/scene references;
- ScriptableObject assets;
- reflection string names;
- event subscriptions;
- command handlers;
- lifecycle hooks;
- editor menu items and runners;
- generated files;
- tests asserting old behavior;
- documentation that drives future edits;
- configuration defaults;
- migration or compatibility code.

## Audit Flow

1. Pick the obsolete feature or suspicious subsystem.
2. List all known entry points.
3. Search direct identifiers and user-facing names.
4. Trace lifecycle hooks and registration paths.
5. Trace data writes, cached state, and published outputs.
6. Disable the shallowest entry point first when testing removal.
7. Delete deep implementation only after no active entry point remains.
8. Run compile, tests, smoke path, and log inspection where practical.

## Regression Risks

Stale links often cause:

- old systems re-registering themselves;
- deleted features still writing state;
- UI or overlay reading obsolete payloads;
- tests preserving dead behavior;
- compatibility code reviving removed paths;
- hidden event handlers firing after owner disposal;
- config defaults enabling code nobody expects.

Treat stale code as active until proven unreachable.
