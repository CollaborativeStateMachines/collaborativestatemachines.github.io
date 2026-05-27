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

An expression models an evaluable statement used to compute values, evaluate guard conditions, or perform
state modifications within the execution context.

Expressions are declared as strings and interpreted by the runtime system. Depending on the expression
language supported by the underlying implementation, expressions may evaluate to a single output value (such
as boolean logic or arithmetic calculations) or execute state mutations via assignments.

```pkl
"5 + 5 + v"
```

Expressions also support assignment semantics to modify scoped variables.

```pkl
"v = v + 5"
```

### Properties

| Property | Type     | Multiplicity | Description                                                        |
| -------- | -------- | ------------ | ------------------------------------------------------------------ |
| `value`  | `String` | Required     | The raw string statement to be evaluated by the execution runtime. |

<!-- prettier-ignore -->
!!! info
    The evaluation engine, syntax rules, and supported functions of an expression are implementation-specific.

<!-- prettier-ignore -->
!!! danger "Constraint: Syntactic Correctness"
    Expressions must strictly conform to the syntax rules defined by the target runtime implementation.
