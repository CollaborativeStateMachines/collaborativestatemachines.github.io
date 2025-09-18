---
title: Expression
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Expression
show_datetime: true
order: 2
---

## Expression

An expression represents an evaluable one-line expression that serves to produce an output value.

Expressions are used throughout CSML to represent boolean expressions as well as data expressions. Depending 
on the expression language supported by the runtime system implementation, additional functionality may be 
available, such as the execution of functions, boolean logic, or arithmetic.

An expression is a string that is evaluated to acquire a certain value. 

```pkl
"5+5+v"
```
_Listing 4: An Expression construct._

!!! info
    The evaluation of an expression, e.g., the supported syntax, is implementation-specific.