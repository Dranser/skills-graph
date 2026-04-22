# Skills Graph

Structured skill graph for LLM agents: routing, execution guidance, references, and reusable domain knowledge in JSON.

`Skills Graph` is a repository format for building agent-facing skill systems that are more rigorous than prompt collections and more reusable than one-off task notes.

It combines:
- domain routing
- execution guidance
- dependency expansion
- reference prioritization
- reusable architectural knowledge

The format is compatible with Codex-style agents, cloud LLM workflows, and local agent runtimes that can read structured files and apply them as policy plus domain context.

## Why It Exists

Most "AI skills" are one of three things:
- prompt snippets with weak structure
- tool wrappers with little domain knowledge
- documentation dumps that are hard to route and hard to execute from

Skills Graph separates those concerns cleanly:
- `meta/index.json` handles discovery
- `meta/router.json` handles primary-domain ownership
- `meta/execution-plan.json` handles attachment order
- `meta/depgraph.json` handles capability expansion
- `knowledge/*/*.json` holds the actual skill logic

The result is a graph of skills, domains, dependencies, references, and execution rules that an agent can apply consistently.

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
   |- framework/
   |- unity-ui/
   |- data-oriented-pipeline/
   |- performance/
   |- modding/
   |- assembly-analysis/
   `- neural/
```

## Core Model

Skills Graph uses five layers:

1. Discovery  
   `meta/index.json` lists domains and skill files.
2. Routing  
   `meta/router.json` selects a single primary domain.
3. Execution Planning  
   `meta/execution-plan.json` defines attachment and execution order.
4. Dependency Expansion  
   `meta/depgraph.json` expands capability without overriding primary ownership.
5. Skill Knowledge  
   `knowledge/*/*.json` contains routing logic plus execution guidance.

## Current Domains

- `framework`
- `unity-ui`
- `data-oriented-pipeline`
- `performance`
- `modding`
- `assembly-analysis`
- `neural`

## Example Use Cases

- Route a Unity IMGUI framework design task to `unity-ui`, then attach `framework` and `data-oriented-pipeline` as supporting lenses.
- Treat `data-oriented-pipeline` as a first-class architectural path when optimization should be baked into the system structure.
- Use `framework` for composition root, contracts, DI, and diagnostics without letting it override the primary owner of a feature domain.

## Compatibility

Skills Graph is intentionally not vendor-locked.

It is suitable for:
- Codex-style coding agents
- cloud LLM agents
- local agent runtimes
- custom orchestration systems that can read JSON and Markdown

The project should be positioned as a structured skill system for agents, not as a repo for one model family only.

## Public Positioning

Recommended repository name:
- `skills-graph`

Recommended short description:
- `Structured skill graph for LLM agents: routing, execution guidance, references, and reusable domain knowledge in JSON.`

Recommended compatibility statement:
- `Compatible with Codex-style agents, cloud LLM workflows, and local agent runtimes that can read structured instructions.`

## Getting Started

If you want an agent to use this repository correctly, the minimum read order is:

1. `meta/agents.json`
2. `meta/index.json`
3. `meta/router.json`
4. `meta/execution-plan.json`
5. `meta/depgraph.json`

Then read only the selected skill files and directly relevant references.

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
