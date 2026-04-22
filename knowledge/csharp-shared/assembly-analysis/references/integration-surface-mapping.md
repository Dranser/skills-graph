# Integration Surface Mapping

Integration surface mapping is the bridge from reverse-engineered evidence to runtime modding contracts.

## Inputs

- decompiled source or structured dump output;
- exact type and member signatures;
- metadata records for overloads, attributes, and visibility;
- notes from runtime observation when available;
- current feature need, stated as one narrow runtime concern.

## Candidate Classes

Classify every plausible surface before implementation:

- `hook`: a lifecycle or behavior method suitable for Harmony observation or interception;
- `direct-api`: a public or stable method/property that can be called directly;
- `reflection-probe`: private or version-sensitive state that needs an adapter;
- `snapshot-source`: raw state that should be converted into an immutable read model;
- `facade-operation`: a stable public operation exposed to feature modules;
- `rejected`: a tempting surface that is too broad, fragile, ambiguous, or high risk.

## Stability Questions

- Is the target a semantic lifecycle anchor or incidental implementation detail?
- Is the declaring type stable across the relevant game states?
- Are overloads and signatures exact?
- Does the surface expose the needed payload without broad side effects?
- What happens if the member is renamed, absent, or delayed?
- Can dependent features be disabled without taking down the whole mod?

## Handoff

The output should be a runtime contract:

- target evidence;
- chosen integration mechanism;
- minimal payload;
- handler or adapter boundary;
- event, snapshot, or facade output;
- diagnostics;
- fallback behavior;
- rejected alternatives.
