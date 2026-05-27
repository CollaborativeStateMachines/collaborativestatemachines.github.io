---
title: State machine
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: State machine
show_datetime: true
order: 5
---

## `StateMachine`

A `StateMachine` models reactive, event-driven execution behavior through transitions between control states
and the optional composition of nested state machines.

A state machine consists of one or more states connected by transitions. At any given time, exactly one state
within the state machine is active, while all other states remain inactive. State transitions are triggered by
events, guard conditions, or internal execution semantics defined by the model.

In addition to atomic states, a state machine may contain nested state machines. Nested state machines execute
concurrently with their parent and enable hierarchical decomposition of coordination logic. This allows
complex distributed behavior to be expressed as compositions of smaller autonomous reactive components.

A state machine may also declare transient data local to its execution context.

```pkl
new StateMachine {
  states {
    ["name"] = new State { ... }
    ...
  }
  stateMachines {
    ["name"] = new StateMachine { ... }
    ...
  }
  transient = new Context { ... }
}
```

### Properties

| Property        | Type                        | Multiplicity | Description                                                                         |
| --------------- | --------------------------- | ------------ | ----------------------------------------------------------------------------------- |
| `states`        | `Map<String, State>`        | Required     | A non-empty map of atomic states composing the state machine.                       |
| `stateMachines` | `Map<String, StateMachine>` | Optional     | A map of nested state machines executed concurrently with the parent state machine. |
| `transient`     | `Context`                   | Optional     | Declares transient data local to the state machine and its descendants.             |

#### Name

The name of a state machine is defined by its key within the enclosing `stateMachines` declaration.

State machine names may be used for diagnostics, instance identification, event targeting, or runtime
introspection.

<!-- prettier-ignore -->
!!! info "Name Validity"
    Name validity and naming constraints are implementation-specific.

#### `states`

The `states` property defines the collection of atomic states belonging to the state machine.

Atomic states are declared using the [State](state.md) construct and define the executable coordination
behavior of the machine.

<!-- prettier-ignore -->
!!! danger "Constraint: Minimum Cardinality"
    A state machine must declare at least one state.

<!-- prettier-ignore -->
!!! danger "Constraint: Unique State Names"
    State names must be unique within the enclosing state machine.

<!-- prettier-ignore -->
!!! danger "Constraint: Single Initial State"
    Exactly one [initial state](state.md) must be declared.

<!-- prettier-ignore -->
!!! danger "Constraint: Initial State Reachability"
    Initial states must not declare inward transitions.

<!-- prettier-ignore -->
!!! danger "Constraint: Terminal State Finality"
    Terminal states must not declare outward transitions.

<!-- prettier-ignore -->
!!! danger "Constraint: Reachability"
    Every state must be reachable either directly or transitively from the initial state.

#### `stateMachines`

The `stateMachines` property defines nested state machines executed concurrently with the parent state
machine.

Nested state machines are declared using the `StateMachine` construct itself, enabling hierarchical and
concurrent composition of reactive behavior.

Nested state machines begin execution together with their parent and participate in the same hierarchical
execution structure and scoping model.

#### `transient`

The `transient` property declares lexically scoped transient data local to the state machine.

Transient data is accessible from the declaring state machine and all hierarchically nested components beneath
it, according to the CSM scoping rules described in the [Data Model](data-model.md).
