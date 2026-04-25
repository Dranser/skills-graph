# Skills Graph Entry Point

Use the JSON documents under `meta/` as the primary local policy and knowledge system for task routing, skill attachment, and execution shaping in Skills Graph.

## Scope And Precedence

- This `AGENTS.md` applies to this directory and its descendants unless a deeper `AGENTS.md` overrides it.
- If this repository is embedded inside a larger host project, treat this file as authoritative only for this subtree.
- Outside this subtree, follow the host project's own local instructions.

## External Project Use

- This file does not need to live at repository root to be valid.
- If a task spans both host-project code and Skills Graph content, keep these rules local to the Skills Graph subtree unless explicitly promoted to project-wide policy.
- If the user explicitly disables Skills Graph routing for a task, skip the JSON routing flow and work directly.

## Activation Gate

Before using the JSON routing flow, classify the user's intent. Skills Graph is an execution and design-routing system, not a mandatory prelude to every repository interaction.

Use direct repository work, without full Skills Graph routing, when the user asks to:

- gather or summarize project context;
- inspect files, folders, dependencies, build layout, or repository shape;
- answer a narrow question about known files;
- make a small mechanical edit in a known location;
- run validation, formatting, tests, search, or status checks;
- inventory existing skills, manifests, routes, or graph connectivity.

Use the full Skills Graph routing flow when the user asks to:

- design or redesign architecture;
- perform implementation work where domain ownership is not obvious;
- choose stack, domain, primary owner, or attached skills;
- plan or execute refactoring, rewrite, migration, diagnostics, runtime, UI, performance, or pipeline work;
- apply Skills Graph to a downstream project as an active task-shaping system.

If intent is ambiguous, prefer the smallest useful mode first: inspect directly, state the inferred intent, and only escalate to full routing when the task needs execution shaping.

## Stack Isolation Policy

- Do not mix materially different technology stacks inside the same skill set, domain folder, or routing surface.
- Treat stack selection as a first-class routing dimension alongside domain selection.
- Resolve the target stack before or together with primary domain selection when multiple stacks are present in the repository.
- Keep stack-specific guidance, references, constraints, and recipes in stack-specific directories.
- Reuse cross-stack ideas only through explicit shared skills or shared references, never by silently merging stack assumptions into one skill file.
- If two stacks solve the same domain problem differently, model them as separate stack variants rather than one blended skill.
- If the target stack is ambiguous, prefer the repository's declared default stack and state that assumption.

## Required Read Order

When the Activation Gate selects full Skills Graph routing, read:

1. `meta/agents.json`
2. `meta/index.json`
3. `meta/router.json`
4. `meta/execution-plan.json`
5. `meta/depgraph.json`

Then read only the minimum necessary files under `knowledge/` selected by the routing result.

## Operational Contract

- Treat `meta/agents.json` as the top-level execution and engineering policy.
- Treat `meta/index.json` as discovery-only metadata.
- Treat `meta/router.json` as the stack-and-domain routing policy.
- Treat `meta/execution-plan.json` as the attachment and execution-order policy.
- Treat `meta/depgraph.json` as dependency expansion only, never as authority for primary routing.
- Treat `knowledge/<stack>/<domain>/*.json` skill files as the canonical local instruction source for selected domains and skills.
- Never treat manifest files as executable skills.
- Never treat entry files as executable skills.

## Fallback Mode

- If `meta/` is missing, incomplete, or clearly irrelevant to the requested task, do not invent missing routing state.
- Fall back to direct repository inspection and load only the minimum relevant files.
- If the user requests a narrow edit in a known file, inspect that file first and load additional policy only if it changes the implementation.
- If the user asks only to collect context, produce an evidence-based summary from direct inspection and do not select a primary skill unless they ask for task execution or design guidance.

## Routing Rules

- Select the target stack before selecting the primary domain when multiple stacks are present.
- Select one primary domain within the selected stack.
- Do not use dependencies to choose the primary domain.
- Freeze the selected stack after resolution.
- Freeze the primary domain after selection.
- Attach same-domain skills before cross-domain skills.
- Read only the selected skill files and the minimum directly relevant dependencies needed for the current task.

## Global Engineering Constraints

- Do not introduce third-party libraries, frameworks, or packages as the default path.
- Prefer custom from-scratch implementations for core runtime, UI, diagnostics, bootstrap, and composition concerns.
- Prefer CPU-predictable designs over convenience abstractions.
- Minimize heap allocations in hot paths.
- Avoid reflection-heavy, attribute-driven, or hidden runtime orchestration.
- Prefer explicit lifecycle, explicit ownership, deterministic startup, and profiler-friendly control flow.
- If a target stack strongly depends on framework-managed lifecycles or external-library abstractions, treat these skills as material to adapt carefully, not as drop-in default policy.

## File Selection Discipline

- Read policy before breadth. Do not scan large parts of the repository without a concrete need.
- Prefer targeted file reads over broad discovery once the task owner and scope are clear.
- When the task touches a single subsystem, load only the files necessary for that subsystem and its direct dependencies.
- Do not treat open editor tabs, reference markdown files, or manifests as active instructions unless the task directly depends on them.

## Response Shaping

- Keep loaded context minimal and relevant.
- Use the selected primary skill as the main reasoning lens.
- Use attached skills only to support the primary design.
- If the JSON policy and a generic default assistant behavior conflict, follow the JSON policy for work in this directory.
