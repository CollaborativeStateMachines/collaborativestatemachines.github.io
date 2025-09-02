---
title: Event
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Event
show_datetime: true
order: 7
---

Event-related constructs are described below.

## Event

Events are raised and handled by state machines in the collaborative state machine and are matched based on
their name.

Events fall into three categories: _internal_, _external_, and _global_.

Internal events are exclusively handled by the state machine that raised them and not seen by other state
machines. External events are received by state machines that have subscribed to the events raised by other 
state machines. This linkage introduces decentralization, allowing state machines to dynamically subscribe to
events of interest raised by other state machines. Global events are not tied to any specific source state 
machine and are universally seen by every state machine. This universal handling establishes a global 
communication channel within the application. 

A raised event may contain data transmitted to the receiver; an expression is used to gather the data to 
include with the event.

When a state machine processes the received event and takes a transition, the event data is assigned to the 
current state's local data using the name and value of each provided context variable.

This overrides existing variables or creates new variables if they don't exist within the dynamic extent.

```pkl
new Event {
    name = "e1"
    channel = "global"
    data = new Data{...}
}
```
/// caption
Listing 9: An Event construct.
///

The following keywords can/must be provided:

| **Keyword** | **Description** | **Type**                        | **Optional** |
| ----------- | --------------- | ------------------------------- | ------------ |
| name        | Event name.     | string                          | No           |
| channel     | Event channel.  | [channel](#channel) | No           |
| data        | Event data.     | list of [Variable](data.md)   | Yes          |

### name

The _name_ keyword specifies the event name. Events are matched according to their name. Event names may be
re-used.

!!! info ""
    The validity of the event name is implementation-specific.

### channel

The _channel_ keyword specifies the event channel of the raised event.

The event channel is a string enumeration and must be one of the following:

- internal
- external
- global

### data

The _data_ keyword specifies the event data contained within the event.

Event data will be assigned to the local data of the state machine's current state which processes the event
and takes a transition.

Event data can be accessed inside expressions by their variable names, with event data variables prefixed by a
_$_ to indicate their special status compared to context variables. The lifetime of event data is bound to the
transition following the event and any subsequently executed actions.
