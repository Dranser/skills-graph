# Bug Investigation

Bug investigation starts from a failure contract.

## Failure Contract

Record:

- expected behavior;
- actual behavior;
- trigger or input;
- environment or mode;
- first visible symptom;
- closest owning subsystem;
- available validation path.

Do not patch before the mismatch is clear unless the failure blocks all investigation.

## Root Cause Workflow

1. Reproduce or approximate the failure.
2. Identify where valid state first becomes invalid.
3. Trace call flow, dataflow, lifecycle, and ownership around that boundary.
4. Form one concrete hypothesis at a time.
5. Prove or disprove it with inspection, test, logging, or a narrow experiment.
6. Patch the cause.
7. Validate the original failure path and one adjacent regression risk.

## Useful Search Targets

When the cause is unclear, search for:

- call sites of the failing method;
- writers of the corrupted state;
- lifecycle hooks that may run in unexpected order;
- cached or published state;
- feature flags and configuration;
- event subscriptions;
- serialization or editor-created references;
- recently deleted or renamed code paths;
- tests that encode old behavior.

## Fix Discipline

A good bug fix should answer:

- what contract was violated;
- why the code allowed it;
- why the patch restores the contract;
- what validation proves the path is fixed;
- what risk remains.

Avoid broad cleanup in the same patch unless stale code is the root cause and removal is required for correctness.
