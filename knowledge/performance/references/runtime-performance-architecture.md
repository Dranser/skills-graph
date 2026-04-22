# Runtime Performance Architecture

Performance for a custom runtime is a contract across lifecycle, phases, storage, scheduling, synchronization, diagnostics, and host integration.

## Runtime Identity

A CPU-first runtime should be able to explain:

- how it starts, updates, fixed-updates, suspends, resumes, and shuts down;
- where mutation intent enters;
- when structural mutation is applied;
- where hot state lives;
- how work is scheduled;
- when computed state becomes visible;
- how diagnostics observe cost and correctness.

Unity or another engine should remain the host. The runtime owns simulation and compute state; host systems submit input and consume published output.

## Phase Discipline

Phases are responsibility and synchronization boundaries. A useful phase model separates:

- command collection;
- lifecycle or structural resolution;
- fixed deterministic compute;
- variable cadence compute;
- output preparation;
- submit or host handoff;
- diagnostics.

Do not add phases to solve local ordering hacks. Add phases only when there is a new category of responsibility.

## Command-Controlled Mutation

Commands protect structural mutation.

Good command discipline means:

- host and modules express mutation intent;
- validation happens before application;
- structural changes apply in controlled windows;
- workers never perform structural mutation;
- direct writes from host systems into runtime storage are rejected.

Performance work must not bypass these boundaries for convenience.

## Storage And Identity

Packed storage separates logical identity from dense iteration location.

Use:

- handles for stable identity;
- slot/generation checks for stale handle rejection;
- dense packed arrays or lists for hot traversal;
- swap-back or controlled compaction for deletion;
- diagnostics for growth, stale access, creation, destruction, and compaction moves.

Dense storage is what makes range tasks, cache locality, and worker partitioning practical.

## Scheduler Shape

The scheduler should serve the runtime contract. It should not become the architecture.

Prefer:

- serial tasks for orchestration and small finalize steps;
- range tasks for dense hot loops;
- tiled or grouped tasks for ownership-aware chunks;
- island tasks for independent interaction groups;
- reduction tasks for deterministic aggregation;
- barriers for explicit observation boundaries.

Keep single-thread mode as a permanent reference path.

## Unity Host Boundary

Unity-facing code may bootstrap, forward lifecycle, submit inputs, consume outputs, and render/debug published state.

Runtime core and worker tasks should not depend on:

- GameObjects;
- Transforms;
- Renderers;
- host object state;
- Unity API calls inside worker execution.

If host data is needed, capture it into runtime-owned snapshots first.

## Review Questions

- Does the runtime still make sense as a pure CPU simulation project?
- Can each phase explain what it owns?
- Are structural mutation and worker compute separated?
- Can storage be iterated densely without exposing unstable packed indices as identity?
- Are worker writes disjoint or merged through explicit reductions?
- Is published state stable before host systems read it?
- Do diagnostics explain cost without becoming execution policy?
