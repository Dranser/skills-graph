# Skills Graph

Structured skill graph for LLM agents: routing, execution guidance, references, and reusable domain knowledge in JSON.

Current practical emphasis:
- the current skill set is strongest for `.NET`, `C#`, and Unity-oriented engineering work
- the format itself is model-agnostic and can be adapted to other stacks
- stacks and domains are both explicit routing dimensions
- this is not a strong default fit for framework-first or third-party-library-first projects

## What You Will Find Here

This repository contains three practical layers:

- `meta/`: routing and execution policy for the graph itself
- `knowledge/`: stack-aware domain skills with ownership rules, execution guidance, and references
- `docs/`: public-facing metadata for naming and repository positioning

## Why Someone Would Use It

Use Skills Graph when you want agent behavior to be:

- reusable instead of task-by-task prompt rewriting
- inspectable instead of hidden in a long system prompt
- domain-aware instead of flat keyword matching
- execution-oriented instead of "just context"

Core layers:
- `meta/index.json`: discovery
- `meta/router.json`: stack selection and primary-domain ownership
- `meta/execution-plan.json`: attachment order
- `meta/depgraph.json`: capability expansion
- `knowledge/<stack>/<domain>/*.json`: actual skill logic

## What Makes It Different

This repository is not just a prompt library.

Each mature skill can contain:
- routing signals
- ownership rules
- execution-layer guidance
- recipes
- pitfalls
- review checklists
- prioritized references

That means a skill can answer both:
- "Should this domain own the task?"
- "How should the agent actually perform the work?"

## Repository Structure

```text
skills-graph/
|- AGENTS.md
|- README.md
|- CONTRIBUTING.md
|- LICENSE
|- NOTICE
|- docs/
|  `- METADATA.md
|- meta/
|  |- agents.json
|  |- index.json
|  |- router.json
|  |- execution-plan.json
|  |- depgraph.json
|  |- skill-schema.json
|  `- skill-template.json
`- knowledge/
   |- README-SKILL-FORMAT.md
   |- csharp-shared/
   |  |- assembly-analysis/
   |  |- code-maintenance/
   |  |- data-oriented-pipeline/
   |  |- framework/
   |  |- neural/
   |  `- performance/
   `- unity/
      |- gpu/
      |- modding/
      `- unity-ui/
```

## Core Model

Skills Graph uses five layers:

1. Discovery  
   `meta/index.json` lists stacks, domains, and skill files.
2. Routing  
   `meta/router.json` selects a stack first, then a single primary domain.
3. Execution Planning  
   `meta/execution-plan.json` defines attachment and execution order.
4. Dependency Expansion  
   `meta/depgraph.json` expands capability without overriding primary ownership.
5. Skill Knowledge  
   `knowledge/<stack>/<domain>/*.json` contains routing logic plus execution guidance.

## Current Stack Layout

- `csharp-shared`: `assembly-analysis`, `code-maintenance`, `data-oriented-pipeline`, `framework`, `neural`, `performance`
- `unity`: `gpu`, `modding`, `unity-ui`

## Example Use Cases

- Route a Unity IMGUI framework design task to `unity-ui`, then attach `framework` and `data-oriented-pipeline` as supporting lenses.
- Treat `data-oriented-pipeline` as a first-class architectural path when optimization should be baked into the system structure.
- Use `framework` for composition root, contracts, DI, and diagnostics without letting it override the primary owner of a feature domain.

## Compatibility

It is suitable for:
- Codex-style coding agents
- cloud LLM agents
- local agent runtimes
- custom orchestration systems that can read JSON and Markdown

Important scope note:
- the repository format is general-purpose
- the current knowledge base is optimized first for `.NET`, `C#`, and Unity coding and architecture tasks
- when multiple stacks coexist, routing should consider stack as well as domain
- this repository is not aimed at projects where third-party frameworks or libraries define most of the architecture

## Public Positioning

Recommended repository name:
- `skills-graph`

Recommended short description:
- `Structured skill graph for LLM agents: routing, execution guidance, references, and reusable domain knowledge in JSON.`

Recommended compatibility statement:
- `Compatible with Codex-style agents, cloud LLM workflows, and local agent runtimes that can read structured instructions.`

## Getting Started

First decide whether the task needs Skills Graph routing.

Use direct inspection without full routing for read-only context gathering, repository inventory, narrow known-file questions, small mechanical edits, and validation/status checks. Use full routing when the task needs architecture ownership, implementation shaping, refactoring, rewrite, migration, diagnostics, runtime, UI, performance, pipeline, or modding guidance.

If you want an agent to use this repository correctly, the minimum read order is:

1. `meta/agents.json`
2. `meta/index.json`
3. `meta/router.json`
4. `meta/execution-plan.json`
5. `meta/depgraph.json`

Then read only the selected skill files and directly relevant references.

If you are evaluating current skill coverage, assume the strongest guidance today targets:
- `.NET` application architecture
- `C#` implementation work
- Unity runtime, tooling, UI, and performance-related design

## Authoring Standard

New or heavily reworked skills should follow:
- [meta/skill-schema.json](./meta/skill-schema.json)
- [meta/skill-template.json](./meta/skill-template.json)
- [knowledge/README-SKILL-FORMAT.md](./knowledge/README-SKILL-FORMAT.md)

## Repository Packaging

Public release material is split across:
- [docs/METADATA.md](./docs/METADATA.md) for naming, positioning, GitHub fields, and launch copy
- [CONTRIBUTING.md](./CONTRIBUTING.md) for contributor expectations

## License

This repository is licensed under [CC BY-NC-SA 4.0](./LICENSE).

That means the material may be used, modified, and forked for non-commercial purposes, with attribution and the same license preserved for derivatives.

## Roadmap

- Continue upgrading legacy domains to the richer execution-layer format
- Add stronger quality gates for mature domains
- Add more consumer examples for different agent runtimes
- Stabilize public repository packaging for open-source release
