# IMGUI State Patterns

## Purpose

Use this reference when the main difficulty is not drawing controls, but deciding where UI state belongs and how long it lives.

## State Categories

Separate state into at least these buckets:
- domain state
- view state
- transient interaction state

Domain state:
- authoritative application or tool data
- should not depend on GUI layout order

View state:
- foldouts
- selection
- expanded sections
- scroll positions
- active tabs
- per-window visibility or arrangement

Transient interaction state:
- drag in progress
- hover-derived temporary state
- ephemeral command intent before commit

## Ownership Patterns

Prefer:
- host-owned window state
- panel-owned local state when truly private
- stable registries keyed by tool, window, panel, or entity identity

Avoid:
- static global dictionaries with no cleanup policy
- state keyed only by list index
- domain model pollution with purely visual flags

## Keying Rules

Good keys:
- asset GUID
- entity ID
- window ID
- panel ID
- stable composite key from domain identity plus panel identity

Weak keys:
- current draw order
- filtered list index
- current visible position in a tree

## Lifetime Rules

Define what resets state:
- tool close
- target change
- domain reload
- explicit user reset

If the reset rule is unclear, ownership is unclear.

## Review Questions

1. Which state must survive redraw?
2. Which state must survive close and reopen?
3. Which state is only visual convenience?
4. Which keys remain stable under sort, filter, and reorder?

## Decision Table

| State example | Preferred home | Why |
| --- | --- | --- |
| Asset data being edited | Domain state | It is authoritative application data, not UI convenience. |
| Foldout open or closed | View state | It is persistent for the tool session but not domain truth. |
| Scroll position | View state | It is purely presentational and tied to the current surface. |
| Active drag operation | Transient interaction state | It is temporary interaction state that should be cleared predictably. |
| Selected entity in a multi-panel tool | Host-owned view state | More than one panel may depend on it. |

## Ownership Heuristics

Promote state upward when:
- multiple panels depend on it
- window restore behavior depends on it
- focus or command routing reads it

Keep state local when:
- it affects only one panel
- it does not influence host geometry or command routing
- it can be reset safely with panel recreation

## Binding And Dirty-State Categories

Track these separately from plain view state when needed:
- binding snapshot state
- last-seen version or hash
- dirty flags per host, panel, or component subtree
- derived presentation data invalidation markers

Do not merge them blindly into domain state or visual convenience state. They exist to control update scope.

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| State keyed by visible list index | Selection and expansion jump after sort or filter | Key by stable domain identity. |
| Global static UI dictionary | State leaks across tools or survives too long | Move ownership to host or scoped registry with lifetime rules. |
| Domain model stores UI-only flags | Tool concerns pollute business data | Split view state out into dedicated UI records. |
| Reset rules are implicit | Random state loss on target change or reopen | Define explicit lifetime and reset triggers. |
| Every state change marks the whole tree dirty | Local edits still trigger broad work | Add subtree-level dirty boundaries and scoped invalidation. |
| Binding cache is treated as source-of-truth | Stale or inconsistent updates become hard to reason about | Keep domain state authoritative and binding state derivative. |

## Review Playbook

When a state design feels wrong:
1. List every state field.
2. Mark each one as domain, view, or transient.
3. Mark which identity owns it.
4. Mark which event resets it.
5. Mark whether it participates in binding or dirty propagation.
6. If any row is unclear, the model needs refinement.
