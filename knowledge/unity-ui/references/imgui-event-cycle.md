# IMGUI Event Cycle

## Core References

- Unity Scripting API: Event
  https://docs.unity3d.com/es/530/ScriptReference/Event.html
- Unity Scripting API: GUIUtility
  https://docs.unity3d.com/es/530/ScriptReference/GUIUtility.html
- Unity Manual: Input System
  https://docs.unity3d.com/ja/2020.3/Manual/com.unity.inputsystem.html
- Input System Manual: Actions
  https://docs.unity3d.com/ja/Packages/com.unity.inputsystem%401.4/manual/Actions.html

## Working Model

Think about IMGUI input and drawing as one event-driven pipeline, not as isolated widget calls.

Useful categories:
- input events
- layout events
- repaint events
- command-style events

In modern Unity projects, do not assume IMGUI `Event` is the only meaningful signal source. If the project uses `Input System Package (New)` and legacy input is disabled, the framework should derive command intent from Input System first, then feed that intent into host, panel, and widget routing.

For framework design, this means:
- host-level routing happens before leaf widget interpretation
- focus ownership is retained state, not a side effect
- event consumption must be visible at boundaries

## Host Boundary Rules

At the host level:
- inspect the current event
- read or receive Input System actions that represent tool commands
- resolve global commands first
- delegate to the active window or panel next
- consume only when ownership is clear

At the panel level:
- handle panel-local commands
- return unhandled intent upward or leave the event untouched

At the widget level:
- keep handling narrow
- avoid hidden global behavior

## Input System Guidance

Prefer this order in modern Unity:
1. collect Input System action or device signals
2. translate them into framework commands
3. route those commands at host and panel boundaries
4. use IMGUI `Event` details where control-level UI behavior still requires them

This keeps the framework alive when old `UnityEngine.Input` paths are unavailable.

## Focus And Control Identity

`GUIUtility` exposes concepts that matter architecturally:
- `keyboardControl`
- `hotControl`
- `GetControlID`
- state objects associated with control IDs

Use them as evidence of the true ownership model:
- focus exists and must be owned
- active manipulation exists and must be owned
- control identity exists and must remain stable

Do not design a framework that pretends these concerns are irrelevant.

## Review Questions

1. Where does global shortcut handling happen?
2. How does the system know which panel is active?
3. What is allowed to call `Use` on the event?
4. Can focus transitions be traced at the host boundary?
5. Will the same structure be reached during layout and repaint?

## Common Failures

- leaf widgets consuming events that should remain global
- no explicit active-panel ownership
- focus bugs caused by transient routing decisions
- layout and repaint reaching different structure
- framework shortcut logic tied to legacy input APIs
- no explicit bridge from Input System actions into IMGUI command routing
