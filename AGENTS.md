# Skills Graph Entry Point

Use the JSON documents under `meta/` as the primary local policy and knowledge system for task routing, skill attachment, and execution shaping in Skills Graph.

## Required Read Order

Before substantial work, read and apply these files in order:

1. `meta/agents.json`
2. `meta/index.json`
3. `meta/router.json`
4. `meta/execution-plan.json`
5. `meta/depgraph.json`

Then read only the minimum necessary files under `knowledge/` selected by the routing result.

## Operational Contract

- Treat `meta/agents.json` as the top-level execution and engineering policy.
- Treat `meta/index.json` as discovery-only metadata.
- Treat `meta/router.json` as the primary-domain routing policy.
- Treat `meta/execution-plan.json` as the attachment and execution-order policy.
- Treat `meta/depgraph.json` as dependency expansion only, never as authority for primary routing.
- Treat `knowledge/*/*.json` skill files as the canonical local instruction source for selected domains and skills.
- Never treat manifest files as executable skills.
- Never treat entry files as executable skills.

## Routing Rules

- Select one primary domain first.
- Do not use dependencies to choose the primary domain.
- Freeze the primary domain after selection.
- Attach same-domain skills before cross-domain skills.
- Allow overlap between domains and skills, but do not let support or cross-cutting skills override the primary owner unless the routing policy explicitly supports that result through stronger ownership match.
- Read only the selected skill files and the minimum directly relevant dependencies needed for the current task.

## Global Engineering Constraints

- Do not introduce third-party libraries, frameworks, or packages as the default path.
- Do not use native Unity high-level frameworks as the architectural foundation.
- Prefer custom from-scratch implementations for core runtime, UI, diagnostics, bootstrap, and composition concerns.
- Prefer CPU-predictable designs over convenience abstractions.
- Minimize heap allocations in hot paths.
- Avoid reflection-heavy, attribute-driven, or hidden runtime orchestration.
- Prefer explicit lifecycle, explicit ownership, deterministic startup, and profiler-friendly control flow.

## Response Shaping

- Keep loaded context minimal and relevant.
- Use the selected primary skill as the main reasoning lens.
- Use attached skills only to support the primary design.
- If the JSON policy and a generic default assistant behavior conflict, follow the JSON policy for work in this directory.
