---
title: State
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: State
show_datetime: true
order: 6
---

## `State`

A `State` represents an atomic execution unit within a `StateMachine`. A state becomes active when entered
through a transition and remains active until another transition causes the state machine to advance to a
different state.

States may be classified as:

- **Initial states**, which define the starting control state of the containing state machine.
- **Terminal states**, which indicate completion of the state machine lifecycle.
- **Intermediate states**, which represent ordinary operational control states.

At any point during execution, exactly one state within a state machine is active.

State behavior is defined through transitions and lifecycle actions:

- **On transitions** react to incoming events.
- **Always transitions** are evaluated independently of events and may trigger autonomously when their guard
  conditions hold.
- **Entry actions** execute when entering the state.
- **Exit actions** execute when leaving the state.
- **During actions** execute while the state remains active.
- **After actions** define time-triggered behavior through scheduled timeout actions.

States may also declare statically scoped data. Unlike transient data, static data persists across repeated
activations of the same state, enabling local state retention between exits and re-entries.

```pkl
new State {
  initial = true
  terminal = false
  entry {
    new ...Action { ... }
    ...
  }
  exit {
    new ...Action { ... }
    ...
  }
  during {
    new ...Action { ... }
    ...
  }
  after {
    new TimeoutAction { ... }
    ...
  }
  on {
    ["event"] = new Transition { ... }
    ...
  }
  always {
    new Transition { ... }
    ...
  }
  static = new Context { ... }
}
```

### Properties

| Property   | Type                      | Multiplicity | Description                                                           |
| ---------- | ------------------------- | ------------ | --------------------------------------------------------------------- |
| `initial`  | `Boolean`                 | Optional     | Marks the state as the initial state of the containing state machine. |
| `terminal` | `Boolean`                 | Optional     | Marks the state as a terminal state.                                  |
| `entry`    | `List<Action>`            | Optional     | Actions executed upon entering the state.                             |
| `exit`     | `List<Action>`            | Optional     | Actions executed upon exiting the state.                              |
| `during`   | `List<Action>`            | Optional     | Actions executed while the state remains active.                      |
| `after`    | `List<TimeoutAction>`     | Optional     | Time-triggered actions executed while the state is active.            |
| `on`       | `Map<String, Transition>` | Optional     | Event-triggered transitions indexed by event name.                    |
| `always`   | `List<Transition>`        | Optional     | Transitions evaluated independently of incoming events.               |
| `static`   | `Context`                 | Optional     | Lexically scoped static data associated with the state.               |

### Name

The name of a state is defined by its key within the enclosing `states` declaration of the containing state
machine.

State names may be used for diagnostics, transition references, runtime inspection, or visualization purposes.

<!-- prettier-ignore -->
!!! info "Name Validity"
    Name validity and naming constraints are implementation-specific.

#### `initial`

The `initial` property specifies whether the state is the initial state of the containing state machine.

An initial state defines the starting control state activated when the state machine instance begins
execution.

<!-- prettier-ignore -->
!!! danger "Constraint: Single Initial State"
    Exactly one initial state must be declared within a state machine.

<!-- prettier-ignore -->
!!! danger "Constraint: Initial State Reachability"
    Initial states must not declare inward transitions.

#### `terminal`

The `terminal` property specifies whether the state is terminal.

Terminal states indicate completion of the containing state machine lifecycle. Multiple terminal states may
sexist within the same state machine.

<!-- prettier-ignore -->
!!! danger "Constraint: Terminal State Finality"
    Terminal states must not declare outward transitions.

#### `entry`, `exit`, `during`, `after`

Lifecycle actions define executable behavior associated with the state.

- `entry` actions execute when the state becomes active.
- `exit` actions execute immediately before leaving the state.
- `during` actions execute while the state remains active.
- `after` actions define timeout-triggered behavior evaluated during state execution.

Actions execute within the lexical scope of the state and have access to all variables visible through the CSM
scoping hierarchy. See [Data Model](data-model.md).

#### `on`

The `on` property defines event-driven transitions originating from the state.

Each entry associates an incoming event with a transition specification. When a matching event is received and
the transition guard evaluates to true, the transition becomes enabled.

Events may originate from:

- The containing state machine
- Other state machines
- External systems or devices
- Runtime-generated timeout events

<!-- prettier-ignore -->
!!! danger "Constraint: Valid Event References"
    Events referenced by `on` transitions must correspond to valid events observable within the collaborative
    state machine execution context.

#### `always`

The `always` property defines spontaneous transitions evaluated independently of incoming events.

Always transitions enable autonomous control progression based on guard evaluation and internal execution
semantics.

These transitions correspond to event-independent execution semantics within the CSM model.

#### `static`

The `static` property declares lexically scoped static data associated with the state.

Static data persists across repeated activations of the same state and retains its values after exiting and
re-entering the state.
