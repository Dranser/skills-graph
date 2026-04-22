# Diagnostics And Readiness

Runtime integration should explain what is available, what is degraded, and why.

## Required Signals

- loader attached state;
- module states and initialization failures;
- hook registration and active patch state;
- reflection probe availability;
- lifecycle phase and current game context;
- main-thread queue pressure when dispatching is used;
- sync channel state when multiplayer transport is involved.

## Readiness Gates

Feature modules should check readiness before acting on game internals:

- hook active;
- game state or scene ready;
- local object reference valid;
- required probe data available;
- config and diagnostics initialized.

## Failure Policy

Missing optional integration should disable only dependent features. Missing core integration should fail loudly in diagnostics, not through scattered null references or silent no-op behavior.
