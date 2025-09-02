---
title: State Machine
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: State machine
show_datetime: true
order: 5
---

State machine-related constructs are described below.

## StateMachine

The purpose of a state machine is to model event-driven execution behavior across the computing continuum by 
enabling transitions between states or by accommodating nested state machines that are executed concurrently.

Nested state machines allow a state machine to express concurrent behavior while providing mechanisms to
observe and influence the execution of the parent state machine. They begin execution simultaneously with the
parent state machine.

A state machine consists of one or more states, which are reached through state transitions. At any given
moment, only one state within a state machine is active, while all other states remain inactive.

As with collaborative state machines, local and persistent data can be declared within a state machine.

```pkl
new StateMachine {
    name = "SM2"
    states {
        new State {...}...
    }
    stateMachines {
        new StateMachine {...}...
    }
    localData = new Data {...}
    persistentData = new Data {...}
}
```
/// caption
Listing 7: A StateMachine construct.
///

The following keywords can/must be provided:

| **Keyword**    | **Description**                         | **Type**                                | **Optional** |
|----------------|-----------------------------------------|-----------------------------------------| ------------ |
| name           | Name of the state machine.              | string                                  | No           |
| states         | Collection of states.                   | list of [State](state.md)                 | No           |
| stateMachines  | Collection of state machines.           | list of [StateMachine](state-machine.md) | No           |
| localData      | Local data.                             | [Data](data.md)                           | Yes          |
| persistentData | Persistent data.                        | [Data](data.md)                           | Yes          |

### name

The _name_ keyword specifies the name of the state machine and may be used for diagnostic purposes or
referencing.

The name of a state machine is not referenced inside a description.

!!! info ""
    The validity of a name is implementation-specific.

### states

The _states_ keyword is used to specify the collection of atomic states included in the state machine.

Atomic states are described using the [State](state.md) construct.

!!! warning ""
    At least one state must be declared.

!!! warning ""
    [State.name](state.md)s must be unique.

!!! warning ""
    Exactly one [initial state](state.md) must be declared.

!!! warning ""
    Initial states must not have inward transitions.

!!! warning ""
    Terminal states must not have outward transitions.

!!! warning ""
    Every state must be reachable (it must have a transition leading into it, or it must be the initial 
    state).

### stateMachines

The _stateMachines_ keyword is used to specify the collection of nested state machines.

Nested state machines are described using the [StateMachine](state-machine.md) construct.

### localData / persistentData

The _localData_ and _persistentData_ keywords allow for _lexically_ declaring respectively the local and
persistent data at the state machine level.

Data described at the state machine level is accessible from the state machine and any component
hierarchically below it, see [dynamic extent](data-model.md).
