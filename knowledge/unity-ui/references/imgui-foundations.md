# IMGUI Foundations

## Scope

Use IMGUI as a framework foundation for:
- editor tools
- debug overlays
- inspectors
- programmer-facing runtime tooling

Do not treat IMGUI as the default foundation for player-facing production UI. Unity's own manual positions IMGUI mainly as a code-driven system for programmer tools and editor extension work.

Official sources:
- Unity Manual: Immediate Mode GUI (IMGUI)
  https://docs.unity3d.com/es/2018.3/Manual/GUIScriptingGuide.html
- IMGUI module overview
  https://docs.unity3d.com/es/2020.2/Manual/com.unity.modules.imgui.html

## Mental Model

Treat IMGUI as an immediate drawing and event-processing API. The draw code runs through `OnGUI`, but the UI framework must retain its own state outside the draw calls.

Separate:
- domain state
- retained UI state
- draw-time logic

Do not infer persistent truth from the current draw pass.

## Event-Phase Discipline

Anchor design around a deterministic event pipeline:
- host receives the current event
- retained state is available before drawing
- the same structural call graph remains reachable across layout and repaint
- event ownership and consumption stay explicit

The framework must not hide:
- `Layout`
- `Repaint`
- keyboard focus
- hot control
- event consumption

If an abstraction makes those invisible, it is too expensive architecturally.

## Architectural Guidance

Prefer:
- one explicit UI host per window or tool surface
- retained window or panel state outside draw helpers
- contract-based composition for panels and widgets
- stable identities for repeated controls and window instances

Avoid:
- pseudo-retained UI trees that emulate another UI system on top of IMGUI
- reflection-driven widget discovery
- service locator access from draw code
- draw-time creation of broad object graphs

## Design Questions

When reviewing or designing an IMGUI framework, answer these first:
1. Who owns the top-level IMGUI host loop?
2. Where does persistent window and panel state live?
3. How is control identity kept stable across passes?
4. Which layer owns focus and event routing?
5. Which abstractions are allowed in draw code, and which must stay in host orchestration?

## Decision Heuristics

Use IMGUI as the primary owner when:
- the task is about tools, debug UI, inspectors, overlays, or editor windows
- deterministic draw order matters
- explicit state ownership matters more than visual authoring tools

Escalate to supporting skills when:
- retained state modeling becomes the main concern: `imgui-state-model`
- composition and panel structure dominate: `imgui-layout-composition`
- focus and command routing dominate: `imgui-input-routing`
- runtime cost dominates: `imgui-performance`

## Decision Table

| Situation | Preferred owner | Why |
| --- | --- | --- |
| Tool window shell, host lifecycle, event-phase architecture | `imgui-core` | This is the main framework orchestration problem. |
| Retained foldouts, tabs, scroll, selection, per-window UI state | `imgui-state-model` | State ownership dominates over draw mechanics. |
| Split panes, sidebars, content slots, reusable panel regions | `imgui-layout-composition` | Structural composition dominates. |
| Modern Unity input, action maps, host commands, focus routing | `imgui-input-routing` | Input and command ownership dominate. |
| Explicit host geometry, Rect allocation, avoiding `GUILayout` | `imgui-manual-layout` | Geometry ownership dominates. |
| Repaint cost, allocation discipline, caches | `imgui-performance` | Runtime cost dominates. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Pseudo-retained tree over IMGUI | Hidden event ownership, unclear hot paths, framework feels magical | Collapse back to explicit host orchestration and retained state outside draw calls. |
| `GUILayout`-driven host composition | Hard-to-review geometry, unstable panel placement, weak control over structure | Move host layout to explicit `Rect` computation. |
| Legacy input assumptions in modern Unity | Framework breaks when only Input System is active | Move command sourcing to Input System actions or device reads. |
| Feature widgets own global behavior | Shortcut conflicts, focus bugs, event loss | Push command ownership back to host or panel boundaries. |
| Full-tree rebuild on local state change | Small edits still trigger broad recomputation and redraw cost | Introduce binding boundaries and subtree-level dirty propagation. |
| Regenerating presentation resources every pass | Allocations and formatting cost remain high even when data is unchanged | Cache derived resources behind explicit invalidation rules. |

## Review Heuristics

Use this quick review order:
1. Identify the host boundary.
2. Identify where retained state lives.
3. Identify how geometry is owned.
4. Identify how commands enter the system.
5. Identify how bindings and dirty markers localize updates.
6. Identify whether any abstraction hides event, focus, layout, or invalidation ownership.

## Optimization Model Heuristics

For this domain, the framework should assume an optimized retained model from the start:
- generate expensive presentation resources once and reuse them
- bind source data to retained presentation state so unchanged data does not force needless work
- propagate dirty state through explicit subtree boundaries
- allow local updates to stop before the whole UI tree is reconsidered

If the design cannot explain those four points, the optimization model is still incomplete.
