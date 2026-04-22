# Unity Official Links

Use these as the official external references for the `unity-ui` IMGUI category.

Interpret them by category, not as a flat list.

## Preferred Foundation Sources

Use these first when reasoning about the domain baseline.

- Unity Manual: Immediate Mode GUI (IMGUI)
  https://docs.unity3d.com/es/2018.3/Manual/GUIScriptingGuide.html

- IMGUI module overview
  https://docs.unity3d.com/es/2020.2/Manual/com.unity.modules.imgui.html

- Unity Scripting API: Event
  https://docs.unity3d.com/es/530/ScriptReference/Event.html

- Unity Scripting API: GUIUtility
  https://docs.unity3d.com/es/530/ScriptReference/GUIUtility.html

## Preferred Input Sources

Use these first when the framework must work with modern Unity input and the legacy Input Manager may be disabled.

- Unity Manual: Input System
  https://docs.unity3d.com/ja/2020.3/Manual/com.unity.inputsystem.html

- Input System Manual: Installation
  https://docs.unity3d.com/ja/Packages/com.unity.inputsystem%401.4/manual/Installation.html

- Input System Manual: Actions
  https://docs.unity3d.com/ja/Packages/com.unity.inputsystem%401.4/manual/Actions.html

## Supporting API Sources

Use these when a specific framework slice needs them.

- Unity Scripting API: GUI.Window
  https://docs.unity3d.com/ScriptReference/GUI.Window.html

- Unity Scripting API: GUIStyle
  https://docs.unity3d.com/ScriptReference/GUIStyle.html

## Baseline-Only Sources

Use these to understand what Unity provides, but do not treat them as the default architectural recommendation for this domain.

- Unity Manual: IMGUI Layout Modes
  https://docs.unity3d.com/cn/2018.3/Manual/gui-Layout.html

Why baseline-only:
- the manual describes available IMGUI layout modes, including `GUILayout`
- `unity-ui` explicitly prefers manual `Rect`-based layout for framework-level composition
- this source is useful for capability awareness, compatibility understanding, and edge-case behavior, not for choosing the default framework layout architecture

## Domain Interpretation Rules

For `unity-ui`, apply these source-priority rules:

1. Use local references first for architectural guidance.
2. Use preferred foundation sources to confirm IMGUI scope and behavior.
3. Use preferred input sources when modern input routing is involved.
4. Use baseline-only layout sources to understand Unity behavior, not to reintroduce `GUILayout` as the default layout model.

## Notes

- Some Unity pages surfaced through search in locale-specific variants. The content is the same API or manual topic; prefer the page title and API/manual identity over locale.
- If a later pass standardizes URLs to one Unity version or locale, update all `url` fields consistently.
