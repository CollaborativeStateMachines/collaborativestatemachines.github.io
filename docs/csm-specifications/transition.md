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

## Transition

A state transition enables the transitioning between the states of a state machine. The result of a state 
transition is exiting the currently active state and entering the newly active state.

CSML supports _external_ transitions, using the _target_ keyword to specify a target state towards which the
transition is directed within the same state machine. A state may externally transition into itself by
declaring a transition with its state name.

Optionally, the target state may be omitted, leading to an _internal_ transition. In this case, the state
machine stays in its current state, bypassing the execution of any actions triggered by entering or exiting a
state while still carrying out the transition actions.

Guard conditions are provided as expressions in the context of transitions. A guard expression
$E : \mathbb{R}^n \to \{\text{true}, \text{false}\}$ takes the form where $\mathbb{R}^n$ represents the
domain, consisting of all variables in scope, see [dynamic extent](data-model.md).

To initiate a transition, the conjunction of all guard expressions must collectively evaluate to true. The
_actions_ keyword is used to specify the actions executed when the transition is taken.

```pkl
new Transition {
  target = "Sa"
  guards {
    new Guard {...}...
  }
  actions {
    new Action {...}...
  }
  else = "Sb"
}
```
_Listing 10: A Transition construct._

!!! warning "Rule"
    Transition guards must be mutually exclusive to ensure determinism, so that at most one transition can be 
    triggered from any given state at a time.

The following keywords can/must be provided:

| **Keyword** | **Description**     | **Type**                    | **Optional** |
| ----------- | ------------------- |-----------------------------| ------------ |
| target      | Target state name.  | [State.name](state.md)   | No           |
| guards      | Guard conditions.   | list of [Guard](guard.md)     | Yes          |
| actions     | Transition actions. | list of [Action](action.md)   | Yes          |
| else        | Else target.        | [State.name](state.md)   | Yes          |

### OnTransition

The on-transition construct is a specialization of the transition construct, adding the _event_ keyword that
specifies the event that triggers the transition.

```pkl
new OnTransition {
  event = "e1"
  target = "Sa"
  guards {
    new Guard {...}...
  }
  actions {
    new Action {...}...
  }
  else = "Sb"
}
```
_Listing 11: An OnTransition construct._

The following keywords can/must be provided:

| **Keyword** | **Description**      | **Type** | **Optional** |
| ----------- | -------------------- | -------- | ------------ |
| event       | Event responding to. | string   | No           |

## target

The _target_ keyword specifies the [State.name](state.md) to transition into.

!!! warning "Rule"
    The target state name must be valid.

## guards

The _guards_ keyword specifies the guard conditions of the transition. All guard expressions must evaluate to
boolean true for the transition to be taken.

!!! warning "Rule"
    The guard expression must evaluate to boolean true or false.

## actions

The _actions_ keyword specifies the actions executed as part of the transition. Actions may be declared
in-line, in which case an action name is not required. Actions may also be referenced by name. Actions
executed within a state have state-scope, see [dynamic extent](data-model.md).

!!! warning "Rule"
    An action reference must be a valid action name.

## else

The _else_ keyword specifies the state to transition into, should the guard condition not evaluate to boolean
true.

!!! warning "Rule"
    The else state name must be valid.

## event

The _event_ keyword specifies the name of an event that triggers the on-transition.

!!! warning "Rule"
    The name of the event must be an event raised within the collaborative state machine.