---
title: Action
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Action
show_datetime: true
order: 9
---

## InvokeAction

An invoke action allows a state machine to call a defined _service type_. The term 'type' in 'service type'
refers to the CSM service abstraction. Invoking a service type does not specify a particular implementation.
Instead, a service type may represent multiple variants of a service, which can differ in algorithm, resource
optimization, or approximate computation methods to reduce costs. The runtime system selects the specific
implementation based on available context, such as user or environmental information, to optimize for
performance or other objectives.

Input data for a service invocation can be provided through one or more expressions specified with the _input_
keyword.

To handle actions executed after the service completes, _done_ events can be declared. These events provide
access to the service's output data, which can be stored, passed to subsequent invocations or events, or used
in other actions.

Setting the _local_ keyword to true signals the runtime system to invoke the service locally, on the same
resource as the state machine, if possible. If omitted or set to false, the service may be invoked remotely on
a different resource.

```pkl
new InvokeAction {
  serviceType = "serviceTypeName"
  local = false
  input = new Data {...}
  output = new Data {...}
  done {
    new Event {...}...
  }
}
```
_Listing 11: An InvokeAction construct._

The following keywords can/must be provided:

| **Keyword** | **Description**        | **Type**                                          | **Optional**          |
| ----------- | ---------------------- |---------------------------------------------------| --------------------- |
| type        | Type of action.        | string                                            | No                    |
| serviceType | Service type name.     | string                                            | No                    |
| local       | Local execution flag.  | boolean                                           | Yes                   |
| input       | Input data.            | list of [ContextVariable](data.md)                     | Yes                   |
| output      | Output data reference. | list of [ContextVariableReference](data.md) | Yes                   |
| done        | Done events.           | list of [Event](event.md)                           | Yes                   |

### serviceType

The _serviceType_ keyword specifies the invoked service type.

The service type is provided as a string value.

!!! info
    The validity of the service type is implementation-specific.

### local

The _local_ keyword provides a hint to the runtime system implementation that the service type is intended to
be invoked locally, in case of boolean true. Otherwise, it is not specified where the runtime system
implementation should invoke the service type.

### input

The _input_ keyword specifies the input to the invoked service type.

### output

The _output_ keyword specifies context variables where the output of the invoked service type should be 
stored. This is done by matching context variables received as output from the service invocation with the 
output references defined using this keyword. Variable references can be local, static or persistent context
variables.

### done

The _done_ keyword specifies the events raised after invocation. Raising done events allows for asynchronous
service type invocation, allow transitioning into subsequent states whenever the service type invocation has
been completed.

## CreateAction / AssignAction / DeleteAction

Data manipulation actions enable the dynamic creation of variables as local or persistent data, the assignment
of values to existing variables, and the deletion of variables.

When creating a variable, the _persistent_ keyword is used to specify whether the variable should be created
persistently. In case the keyword is omitted or has the value false, the variable is locally created in the
current scope.

```pkl
new CreateAction {
  variable = new ContextVariable {...}
  persistent = true
}

new AssignAction {
  variable = new ContextVariableReference {...}
  value = new Expression {...}
}

new DeleteAction {
  variable = new ContextVariableReference {...}
}
```
_Listing 12: A CreateAction, AssignAction, and DeleteAction construct._

The following keywords can/must be provided (create action construct):

| **Keyword** | **Description**                      | **Type**              | **Optional**          |
| ----------- | ------------------------------------ |-----------------------| --------------------- |
| variable    | Variable to create.                  | [ContextVariable](data.md) | No                    |
| persistent  | Whether to create data persistently. | boolean               | Yes                   |

The following keywords can/must be provided (assign action construct):

| **Keyword** | **Description**                  | **Type**                                  | **Optional**          |
| ----------- | -------------------------------- | ----------------------------------------- | --------------------- |
| variable    | Variable reference to assign to. | [ContextVariableReference](data.md) | No                    |
| value       | Value expression.                | [Expression](expression.md)                 | No                    |

The following keywords can/must be provided (delete action construct):

| **Keyword** | **Description**               | **Type**                                  | **Optional**          |
| ----------- | ----------------------------- | ----------------------------------------- | --------------------- |
| variable    | Variable reference to delete. | [ContextVariableReference](data.md) | No                    |

### variable

The _variable_ keyword specifies the variable to create or the variable reference to manipulate.

!!! warning "Rule"
    When creating a variable, the variable must not exist.
   
!!! warning "Rule" 
    When manipulating an existing variable, the variable must exist.

### persistent

The _persistent_ keyword specifies, for a create action, whether the variable needs to be created
persistently. In case it must, the variable is created in the persistent data. Otherwise, the variable is
locally created in the current scope.

### value

The _value_ keyword specifies the value expression for an assign action. The expression is evaluated to
acquire the data value assigned to the variable.

## RaiseAction

```pkl
new RaiseAction {
  event = new Event { ... }
}
```
_Listing 13: A raise event action construct._

The raise event action enables a state machine to raise an event that is subsequently handled by another state
machine or the state machine itself.

The following keywords can/must be provided (raise action construct):

| **Keyword** | **Description**     | **Type**        | **Optional**          |
| ----------- | ------------------- | --------------- | --------------------- |
| event       | The event to raise. | [Event](event.md) | No                    |

### event

The _event_ specifies the event to raise within the collaborative state machine.

## MatchAction

Coming soon.

## TimeoutAction / TimeoutResetAction

```pkl
new TimeoutAction {
  name = "timeout"
  delay = new Expression {...}
  action = new RaiseAction {...}
}

new TimeoutResetAction {
  action = "timeout"
}
```
_Listing 14: A timeout and timeout reset action construct._

A special type of action, the timeout action, is used together with the _after_ keyword. The timeout action
specifies a delay, after which the specified action is executed. The provided action must be a raise event 
action so subsequent behavior can be triggered upon a raised event.

Timeouts can be reset based on the reset timeout action.

The following keywords can/must be provided (timeout action construct):

| **Keyword** | **Description**     | **Type**                    | **Optional**          |
| ----------- | ------------------- |-----------------------------| --------------------- |
| name       | The timeout action name. | string   | No                    |
| delay       | The event to raise. | [Expression](expression.md)   | No                    |
| action     | The action to execute upon timeout. | [RaiseAction](#raiseaction)   | No                    |

The following keywords can/must be provided (timeout reset action construct):

| **Keyword** | **Description**              | **Type**                    | **Optional**          |
| ----------- | ---------------------------- | --------------------------- | --------------------- |
| action      | The timeout action to reset. | [TimeoutAction.name](#name) | No                    |

### name

The _name_ keyword specifies the name of the timeout action used to reference the timeout action for 
resetting.

!!! info
    The validity of an action name is implementation-specific.

### delay

The _delay_ keyword specifies the delay expression evaluated to provide the delay value. The delay value is
specified in milliseconds.

!!! warning "Rule"
    The delay expression must be evaluated to a numeric value.

### action

For a timeout action, the _action_ keyword specifies the action to execute upon the timeout. The action 
provided must be a raised event action.

!!! warning "Rule"
    The actions provided as timeout actions must be raise event actions.

For a timeout reset action, the _action_ keyword specifies the timeout action to reset. The action referenced
must be a previously executed timeout action.

!!! warning "Rule"
    The action reference provided must be a previously executed timeout action.
