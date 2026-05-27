---
title: Collaborative state machine
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Collaborative state machine
show_datetime: true
order: 4
---

## `CollaborativeStateMachine`

Within the Collaborative State Machines model, applications are modeled as networks of collaborating,
distributable state machines that exhibit autonomous, reactive behavior. A state machine transitions between
control states in response to events, executing actions during transitions, on state entry or exit, or while
active within a state. These actions may invoke computational services, manipulate data, or emit events.

CSM extends the traditional single state machine paradigm by representing an application as a coordinated
collection of one or more (distributed) state machines. These state machines collaborate through asynchronous
event propagation and shared data semantics while preserving a unified
application model.

### Data and Memory Model

The CSM data model defines three primary data scopes:

- **Persistent data**: Shared application-wide state accessible across the collaborative state machine.
- **Transient data**: State-machine-local data scoped to a specific distributed execution context.
- **Static data**: Control-state-local data preserved across repeated activations of the same control state.

Data is declared lexically within the application specification rather than dynamically allocated at runtime.
Variable values are resolved through expression evaluation. See [Data Model](data-model.md) for details.

The execution and deployment topology of a CSM is governed by its **Memory Mode**, which defines how state
machines are distributed and how memory is shared:

- **Shared Memory Mode**  
  All constituent state machines execute within a single computing resource and share the same memory space.
  A standalone `StateMachine` is restricted to this mode.

- **Distributed Memory Mode**  
  State machines execute across multiple decoupled computing resources, each maintaining its own local memory
  space while coordinating through events and shared persistent state.

A `CollaborativeStateMachine` defines the root scope of a distributed state machine application.

```pkl
new CollaborativeStateMachine {
  stateMachines {
    ["name"] = new StateMachine { ... }
    ...
  }
  persistent = new Context { ... }
}
```

### Properties

| Property        | Type                        | Multiplicity | Description                                                                                             |
| --------------- | --------------------------- | ------------ | ------------------------------------------------------------------------------------------------------- |
| `stateMachines` | `Map<String, StateMachine>` | Required     | A non-empty map of uniquely named `StateMachine` definitions composing the collaborative state machine. |
| `persistent`    | `Context`                   | Optional     | Declares the persistent data scope shared across the collaborative state machine.                       |

#### `stateMachines`

Defines the collection of collaborating state machines that compose the collaborative state machine.

<!-- prettier-ignore -->
!!! danger "Constraint: Minimum Cardinality"
    The `stateMachines` map must contain at least one valid `StateMachine` definition.

Each key within the map must be unique and identify a single constituent state machine.

#### `persistent`

Defines the lexically scoped persistent data shared across the collaborative state machine. Variables declared
within this scope are accessible according to the CSM scoping rules and may be synchronized across distributed
memory domains depending on the selected memory mode.
