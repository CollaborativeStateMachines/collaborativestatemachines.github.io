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

## State

A state represents a potentially active state, which can be activated by entering through a state transition
and can be _initial_, _terminal_, or neither (intermediate).

There must be exactly one initial state within a state machine. A terminal state, of which multiple can exist,
indicates the end of the lifecycle of the containing state machine.

State transitions are specified as _always_ or _on_ transitions. Always transitions, possibly guarded with
expressions, are taken without events. On-event transitions drive the event-driven nature of state machines,
allowing a state transition based on a received event.

Events may be generated from the state machine itself, other state machines, as well as from the environment
external to CSM (for instance, an external application or device). Actions executed in a state include _entry_
and _exit_ actions upon entering or exiting the state. _While_ actions are executed while the state is active.
Timed actions are declared using the _after_ keyword.

Like other constructs, a state allows for lexically declaring local, static, and persistent data. An
additional type of data, _static_ data, utilizes the capability of the state for re-entry, allowing data to
retain their values between exiting and re-entering the state.

```pkl
new State {
  name = "Sa"
  initial = true
  terminal = false
  entry {
    new Action {...}...
  }
  exit {
    new Action {...}...
  }
  while {
    new Action {...}...
  }
  after {
    new Action {...}...
  }
  on {
    new OnTransition {...}...
  }
  always {
    new Transition{ ... }...
  }
  localData = new Context {...}
  persistentData = new Context {...}
  staticData = new Context {...}
}
```
_Listing 8: A State construct._

The following keywords can/must be provided:

| **Keyword**    | **Description**             | **Type**                                          | **Optional** |
| -------------- | --------------------------- |---------------------------------------------------| ------------ |
| name           | Name of the state.          | string                                            | No           |
| initial        | Initial flag of the state.  | boolean                                           | Yes          |
| terminal       | Terminal flag of the state. | boolean                                           | Yes          |
| entry          | Entry actions.              | list of [Action](action.md)                         | Yes          |
| exit           | Exit actions.               | list of [Action](action.md)                         | Yes          |
| while          | While actions.              | list of [Action](action.md)                         | Yes          |
| after          | After actions.              | list of [TimeoutAction](action.md)   | Yes          |
| on             | On event transitions.       | list of [OnTransition](transition.md)           | Yes          |
| always         | Always transitions.         | list of [Transition](transition.md)                 | Yes          |
| localData      | Local data.                 | [Context](data.md)                                     | Yes          |
| persistentData | Persistent data.            | [Context](data.md)                                     | Yes          |
| staticData     | Static data.                | [Context](data.md)                                     | Yes          |

### name

The _name_ keyword specifies the name of the state and may be used for diagnostic purposes or referencing.

A state's name is referenced throughout the state machine to indicate the transition target.

!!! info
    The validity of a name is implementation-specific.

### initial

The _initial_ keyword specifies whether a state is the initial state of the containing state machine.

!!! warning "Rule"
    Exactly one [initial state](#initial) must be declared.

!!! warning "Rule"
    Initial states must not have inward transitions.

### terminal

The _terminal_ keyword specifies whether a state is a terminal state of the containing state machine. Multiple
states may be declared terminal.

!!! warning "Rule"
    Terminal states must not have outward transitions.

### entry / exit / while / after

The _entry_ keyword specifies the actions executed upon entry of the state.

The _exit_ keyword specifies the actions executed upon exiting the state.

The _while_ keyword specifies the actions executed while in a state.

The _after_ keyword specifies the actions executed upon a timeout.

Actions executed within a state have state scope, see [dynamic extent](data-model.md).

!!! warning "Rule"
    An action reference must be a valid action name.

### on

The _on_ keyword specifies the transitions that can occur based on events.

!!! warning "Rule"
    The referenced event in an on-transition must be raised within the collaborative state machine.

### always

The _always_ keyword specifies the transitions that can occur regardless of raised events.

### localData / persistentData / staticData

The _localData_, _persistentData_, and _staticData_ keywords allow for _lexically_ declaring respectively the
local, persistent, and static data at the state machine level.

Static data is unique to states and is available after re-entry of a state.

Data described at the state level is accessible from the state machine and any component hierarchically below
it, see [dynamic extent](data-model.md).