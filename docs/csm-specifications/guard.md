---
title: Guard
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Guard
show_datetime: true
order: 3
---

## Guard

A guard is an expression that evaluates to a boolean value. Guards are used within transitions to determine
whether a transition is taken.

```pkl
new Guard {
  expression = "v==5"
}
```
_Listing 5: A Guard construct._

The following keywords can/must be provided:

| **Keyword** | **Description**       | **Type**                      | **Optional**         |
| ----------- | --------------------- | ----------------------------- |----------------------|
| expression  | The guard expression. | [Expression](expression.md)   | No                   |

### expression

The _expression_ keyword specifies the guard expression.

!!! warning "Rule"
    The guard expression must evaluate to a boolean value.