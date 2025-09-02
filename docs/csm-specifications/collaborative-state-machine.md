---
title: Collaborative State Machine
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Collaborative state machine
show_datetime: true
order: 4
---

Collaborative state machine-related constructs are described below.

## CollaborativeStateMachine

Within CSM, state machines are inspired by David Harel's statecharts. Their primary role is to exhibit 
autonomous, reactive behavior through state transitions. Actions can occur during transitions, upon entering 
or exiting a state, or while residing in a state. These actions may include invoking computational services, 
manipulating data, or raising events.

A collaborative state machine extends beyond single state machines by modeling an application as a collection
of one or more state machines distributed across the computing continuum. State machines collaborate by
raising and handling events and by sharing data. CSM defines a data model that includes persistent, local, and
static data. Local and persistent data are declared through variables. Data declared within a component
description is _lexically_ declared, meaning it is created as part of the description itself, rather than
_dynamically_ generated through actions (see [data model](data-model.md)). Variable values are determined by
evaluating expressions.

The memory mode defines how state machines may be distributed across the computing continuum. A collaborative
state machine operates in one of two memory modes: _shared_ or _distributed_. In shared memory mode, all state
machines reside on the same computing resource and share memory. In distributed memory mode, state machines
can execute on separate resources, each with private memory. These components run on a distributed computing
infrastructure.

Single state machines always operate in shared memory mode.

Choosing between distributed and shared memory modes has significant implications. Distributed memory enables
greater parallelism, scalability, replication, and decentralization, but may increase data transfer times when
state machines on different resources need to exchange data.

For more details, see the [data model](data-model.md) description.


```pkl
new CollaborativeStateMachine {
    name = "CSM"
    version = "2.0"
    memoryMode = "distributed"
    stateMachines {
        new StateMachine {...}...
    }
    localData = new Data {...}
    persistentData = new Data {...}
}
```
/// caption
Listing 6: A CollaborativeStateMachine construct.
///

The following keywords can/must be provided:

| **Keyword**    | **Description**                                 | **Type**                                                | **Optional** |
| -------------- | ----------------------------------------------- | ------------------------------------------------------- | ------------ |
| name           | Name of the collaborative state machine.        | string                                                  | No           |
| version        | CSML version.                                   | [version](#version)         | No           |
| memoryMode        | Memory mode.                                   | [memoryMode](#memorymode)         | No           |
| stateMachines  | Collection of state machines.                   | list of [StateMachine](state-machine.md)                 | No           |
| localData      | Local data.                                     | [data](data.md)                                           | Yes          |
| persistentData | Persistent data.                                | [data](data.md)                                           | Yes          |

### name

The _name_ keyword specifies the name of the collaborative state machine.

The name of the collaborative state machine is not referenced within the collaborative state machine and only
serves to identify the collaborative state machine among other collaborative state machines.

!!! info ""
    The validity of a collaborative state machine name is implementation-specific.

### version

The _version_ keyword specifies the CSML version used within a description and is used to ensure
backward-compatibility.

The CSML version is bound to a version of these specifications. 

The version is a string enumeration and must be one of the following:

- 1.0
- 2.0
- 3.0

!!! info ""
    A runtime system implementation's support for different CSML versions is implementation-specific.

### memoryMode

The _memoryMode_ keyword specifies the memory mode of the complete collaborative state machine.

The memory mode applies to the entire collaborative state machine.

The memory mode is a string enumeration and must be one of the following:

- distributed
- shared

### stateMachines

The _stateMachines_ keyword is used to specify the collection of state machines included in the collaborative
state machine.

!!! warning ""
    At least one state machine must be declared.

### localData / persistentData

The _localData_ and _persistentData_ keywords allow for lexically declaring respectively the local and
persistent data of the collaborative state machine.

Depending on the memory mode of the collaborative state machine, either or both types of data will be
accessible within the collaborative state machine, see [dynamic extent](data-model.md).

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>