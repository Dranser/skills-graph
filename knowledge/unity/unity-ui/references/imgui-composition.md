# IMGUI Composition

## Core Reference

- Unity Manual: IMGUI Layout Modes
  https://docs.unity3d.com/cn/2018.3/Manual/gui-Layout.html

## Goal

Use composition rules to keep large IMGUI tools maintainable while preserving deterministic structure across passes.

## Recommended Shape

Use a hierarchy like:
- window host
- named panel slots
- container sections
- leaf widgets

This is simpler to review than a single giant `OnGUI` body or a pseudo-retained widget graph.

For framework-level composition, prefer manual geometry with explicit `Rect` calculation. Do not make `GUILayout` the primary layout engine for hosts, split panes, overlays, or reusable tool shells.

## Panel Slot Pattern

Model major regions as named slots:
- toolbar
- sidebar
- content
- footer
- overlay

Optional panels should occupy explicit slots rather than appearing through arbitrary branching.

## Layout Guidance

Use layout APIs deliberately:
- fixed or manual Rect-based layout for host-level structure
- explicit geometry for split panes, toolbars, overlays, and dock-like regions
- narrow leaf-level convenience only if automatic layout is unavoidable

The Unity manual allows mixing `GUI` and `GUILayout`, but this framework category treats that as a capability description, not as architectural advice. For reusable framework composition, manual layout is preferred because it preserves geometry ownership, reviewability, and deterministic structure.

## Composition Rules

- separate containers from leaves
- keep open/close of groups readable
- keep structural branching state-backed
- make repeated sections explicit and keyed
- keep major geometry decisions in host-owned layout code

## Common Failures

- giant draw methods mixing state mutation, routing, and structure
- optional panels inserted with no stable anchor
- deep nesting that obscures ownership of layout groups
- helper stacks so abstract that reviewers cannot see the real structure
- GUILayout-driven host composition that hides geometry and weakens control over panel placement

## Review Questions

1. Can the top-level slots be named quickly?
2. Are optional regions anchored explicitly?
3. Is the same structure reachable during layout and repaint?
4. Are repeated sections keyed by stable identity?
5. Is the geometry of major regions owned explicitly rather than delegated to GUILayout?

