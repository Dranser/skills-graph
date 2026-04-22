# Refactoring

Refactoring changes structure while preserving behavior.

## Before Editing

Define:

- what behavior must remain unchanged;
- what structure is being improved;
- why the current structure is costly;
- which tests, smoke paths, logs, or compile checks can validate the change;
- what code must not be touched.

## Overengineering Audit

A layer is suspicious when it:

- only forwards calls;
- has one caller and no independent ownership;
- hides lifecycle or dependency order instead of clarifying it;
- exists for an old feature that no longer exists;
- makes validation harder than direct code would;
- carries generic abstractions without multiple real implementations.

A layer may still be worth keeping when it protects:

- lifecycle;
- ownership;
- serialization or compatibility;
- validation;
- domain boundary;
- test seam;
- host/integration boundary.

## Refactor Sequence

Prefer small steps:

1. Characterize behavior.
2. Remove or isolate dead entry points.
3. Inline pass-through layers.
4. Collapse duplicate state.
5. Rename or move only after behavior is stable.
6. Run validation.

Separate mechanical moves from semantic edits where possible.

## Deletion Rule

Before deleting code, check:

- direct references;
- registration and manifests;
- event subscriptions;
- serialized fields and assets;
- configuration keys;
- reflection strings;
- editor scripts;
- generated files;
- tests and docs;
- public API or external callers.

Code is safe to delete only after reachability and side effects are understood.
