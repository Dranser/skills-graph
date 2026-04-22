# IMGUI Input System Integration

## Purpose

Use this reference when the IMGUI framework must operate in modern Unity projects where the legacy Input Manager is disabled and `Input System` is the primary source of user input.

## Official References

- Unity Manual: Input System
  https://docs.unity3d.com/ja/2020.3/Manual/com.unity.inputsystem.html
- Input System Manual: Installation
  https://docs.unity3d.com/ja/Packages/com.unity.inputsystem%401.4/manual/Installation.html
- Input System Manual: Actions
  https://docs.unity3d.com/ja/Packages/com.unity.inputsystem%401.4/manual/Actions.html

## Architectural Rule

Treat Input System as the primary source of high-level input intent in new Unity projects.

That means:
- host commands come from `InputAction` or explicit device reads
- panel-local commands come from scoped action ownership
- IMGUI `Event` remains relevant for control-level interaction details
- old `UnityEngine.Input` assumptions must not be required for the framework to function

## Recommended Pipeline

1. enable the relevant action maps with the lifetime of the tool host
2. collect action callbacks or poll action state
3. translate action results into framework commands
4. route commands to the host, active panel, or focused control
5. let IMGUI drawing react to retained command and focus state

## Ownership Guidance

Prefer:
- host-owned action maps for global tool commands
- panel-scoped command ownership for local interactions
- explicit bridges between action callbacks and retained UI state

Avoid:
- direct feature-widget dependence on raw `UnityEngine.Input`
- command semantics encoded only in `Event.current`
- mixing legacy input and Input System assumptions in one framework contract

## Review Questions

1. Can the framework run with `Active Input Handling` set to `Input System Package (New)`?
2. Which action map owns global commands?
3. Which panel or feature owns local commands?
4. Which commands still need IMGUI `Event` details after action translation?

