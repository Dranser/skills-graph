# IMGUI uGUI-Inspired Composition

Use this reference when the framework is implemented on top of `IMGUI`, but borrows composition ideas from `uGUI`.

## Core Position

The implementation base is `IMGUI`.

Interpretation:

- rendering still happens through `IMGUI`
- event flow still follows `IMGUI` constraints
- geometry still resolves into `Rect`-like draw regions owned by the custom framework

`uGUI` is only a reference source for good ideas.

Interpretation:

- do not depend on `Canvas`
- do not depend on `RectTransform`
- do not depend on `CanvasScaler`
- do not depend on `HorizontalLayoutGroup` or `VerticalLayoutGroup`

Instead, build equivalent framework concepts where they help.

## Borrowed Ideas Worth Keeping

Prefer reusing these ideas:

- a root UI host analogous in responsibility to `Canvas`
- anchor-like attachment semantics
- explicit alignment semantics
- adaptive scaling tied to resolution
- bounded sizing rather than uncontrolled growth
- horizontal and vertical composition primitives
- reusable modular windows rather than one-off hierarchies

## Implementation Translation

Translate the borrowed ideas like this:

- `Canvas` becomes a custom root UI host
- `RectTransform` becomes a custom geometry contract
- `CanvasScaler` becomes a custom scale policy
- `HorizontalLayoutGroup` and `VerticalLayoutGroup` become custom layout primitives

Keep these as explicit retained concepts that later resolve into IMGUI geometry.

## Layout and Adaptation

Adaptation should not mean only scaling everything uniformly.

The framework should separately reason about:

- host attachment
- local alignment
- structural layout flow
- scaling policy
- size bounds

Important implication:

- smaller resolutions should be able to lower effective maximum sizes for important components
- larger resolutions should not force unlimited growth

This keeps windows and widgets usable instead of oversized or cramped.

## Window and Widget Model

Use two primary UI units:

### Window

For:

- menus
- inspectors
- panels with multiple contexts
- layouts that may need sidebar-style navigation

### Widget

For:

- HUD modules
- overlays
- compact embedded UI pieces

Do not assume both units need the same shell weight.

## Surface and Container Model

Window-first composition is not the only path.

Use `Surface` and `Container` as first-class composition units when the UI reads as:

- a dashboard
- a workspace
- a diagnostics surface
- a sectioned full-screen tool
- a coordinated metrics, timeline, and log view

Recommended interpretation:

- `Surface` is the top-level coordinated UI plane
- `Container` is the primary section or region unit
- `Window` is a specialized container with shell semantics
- `Widget` is a compact embedded unit

Do not force surface-first or container-first interfaces into window semantics when they do not naturally behave like windows.

## Base Window Shell

Recommended shell model:

```text
Window
+-- Background
\-- RootLayout
    +-- Header (optional)
    |   +-- Title
    |   \-- HeaderActions (optional)
    \-- Body
        \-- ContentLayout
            +-- Sidebar (optional)
            \-- ContextView
```

Shell contract:

- the shell owns structure
- `Header` owns title and top-level actions
- `ContextView` owns injected feature content
- `Sidebar` switches contexts when needed

This is a conceptual layout contract, not a statement about Unity runtime components.

## Nested Layouts

Nested layout should be normal.

Examples:

- root vertical shell for header plus body
- nested horizontal body for sidebar plus content
- nested vertical content for title block plus details plus actions

Preserve structure and intent before converting the layout tree into final geometry.

## Review Questions

- Is `IMGUI` still the implementation base?
- Is `uGUI` used only as design inspiration?
- Are borrowed concepts translated into custom framework primitives?
- Are layout, scaling, and size bounds separate concerns?
- Can windows and widgets share the framework while keeping different policies?
- Does the base shell support optional header and optional sidebar cleanly?
