---
title: Transition
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Transition
show_datetime: true
order: 8
---

## `Transition`

A `Transition` defines the control flow between states within a `StateMachine`. When a transition is taken,
the currently active state is exited and the target state becomes active.

Transitions form the reactive execution semantics of CSML and may be triggered by events, guard evaluation,
timeout behavior, or autonomous control progression.

CSML supports two forms of transitions:

- **External transitions**, which move execution to another state using the `to` property.
- **Internal transitions**, which omit a target state and execute transition behavior without leaving the
  currently active state.

An external transition may also target the originating state itself, resulting in a self-transition. In this
case, the state is exited and re-entered according to the normal lifecycle semantics.

Internal transitions differ in that they do not execute state exit or entry actions. The state remains active
while the transition actions are executed atomically within the current execution context.

Transitions may optionally define guard conditions through the `provided` property. A guard is an expression
evaluated over all variables visible within the transition scope. The transition is enabled only when the
guard evaluates to `true`.

Transition-local actions are declared using the `yields` property.

```pkl
new Transition {
  to = "state"
  provided = "..."
  yields {
    new ...Action { ... }
    ...
  }
  or = "alternativeState"
}
```

<!-- prettier-ignore -->
!!! danger "Constraint: Deterministic Transition Semantics"
    Transition guards originating from the same execution context must be mutually exclusive to ensure
    deterministic execution semantics. At most one transition may become enabled at a time.

### Properties

| Property   | Type           | Multiplicity | Description                                                                                |
| ---------- | -------------- | ------------ | ------------------------------------------------------------------------------------------ |
| `to`       | `State.name`   | Optional     | Target state entered when the transition is taken. If omitted, the transition is internal. |
| `provided` | `Expression`   | Optional     | Guard condition evaluated before the transition may execute.                               |
| `yields`   | `List<Action>` | Optional     | Actions executed atomically as part of the transition.                                     |
| `or`       | `State.name`   | Optional     | Alternative target state entered when the guard condition evaluates to `false`.            |

#### `to`

The `to` property specifies the target state of the transition.

When present, the transition is external and causes the current state to be exited before entering the target
state.

If omitted, the transition is treated as an internal transition and does not alter the currently active state.

Self-transitions are permitted by referencing the currently active state as the target.

<!-- prettier-ignore -->
!!! danger "Constraint: Valid Target State"
    The referenced target state must exist within the containing state machine.

#### `provided`

The `provided` property defines the guard expression associated with the transition.

The guard expression is evaluated within the transition scope and must resolve to a boolean value. A transition becomes eligible for execution only when the expression evaluates to `true`.

If no guard is specified, the transition is considered unconditionally enabled whenever its triggering semantics apply.

<!-- prettier-ignore -->
!!! danger "Constraint: Boolean Guard Evaluation"
    Guard expressions must evaluate to either `true` or `false`.

#### `yields`

The `yields` property defines the actions executed as part of the transition.

Transition actions execute atomically within the transition context and may manipulate data, emit events,
invoke services, instantiate state machines, or perform other runtime-supported operations.

Actions executed within a transition have state-machine-level scope according to the CSM scoping rules
described in the [Data Model](data-model.md).

#### `or`

The `or` property specifies an alternative target state entered when the guard condition associated with
`provided` evaluates to `false`.

This enables declarative branching semantics within a single transition definition.

<!-- prettier-ignore -->
!!! danger "Constraint: Valid Alternative Target"
    The referenced alternative target state must exist within the containing state machine.
