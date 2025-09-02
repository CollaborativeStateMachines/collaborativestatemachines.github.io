---
title: Data
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Data
show_datetime: true
order: 1
---

Data-related constructs are described below.

## Data

A _Data_ construct is used to lexically declare data within a certain scope (see
[dynamic extent](data-model.md)), data may be _persistent_, _local_, or _static_.

- Persistent data is available persistently throughout and possibly outside the collaborative state machine.
- Local data is restricted to a subset of components of the collaborative state machine.
- Static data is specific to states.

```pkl
new Data {
    variables {
        new Variable {...}...
    }
}
```
/// caption
Listing 1: A Data construct (we use _..._ to denote a placeholder for construct keywords, or multiple
instances of a construct).
///

!!! note ""
    The value of a variable is defined through an expression. In Listing 1, the integer value _1_ is obtained
    from the string expression _"1"_, which evaluates to the integer value.

The following keywords can/must be provided:

| **Keyword** | **Description**                         | **Type**                      | **Optional** |
| ----------- | --------------------------------------- | ----------------------------- | ------------ |
| variables   | Collection of variables declaring data. | list of [Variable](#variable) | No           |

### variables

The _variables_ keyword is used to declare variables. Their scope and lifetime are bound to the associated
scope defined by the _persistentData_, _localData_, or _staticData_ keyword.

## Variable

A variable declares data, it does so by providing an expression that, when evaluated, results in a data value.

Whether a variable is in scope is dependent on how it is declared, see [dynamic extent](data-model.md).

```pkl
new Variable {
    name = "variableName"
    value = "1"
}
```
/// caption
Listing 2: A Variable construct.
///

The following keywords can/must be provided:

| **Keyword** | **Description**       | **Type**                  | **Optional** |
| ----------- | --------------------- | ------------------------- | ------------ |
| name        | Name of the variable. | string                    | No           |
| value       | Value expression.     | [Expression](expression.md) | No           |

### name

The _name_ keyword specifies the name of the variable. It serves to reference the variable.

!!! warning ""
    Within a scope, the name of a variable must be unique.

!!! info ""
    The validity of a variable name is implementation-specific.

### value

The _value_ keyword specifies the value expression of the variable. When evaluated, the value expression
yields the variable's value. 

!!! info ""
    Evaluation of a value expression is implementation-specific.

## VariableReference

A variable may be referenced through a variable reference.

```pkl
new VariableReference {
    reference = "photoPath"
}
```
/// caption
Listing 3: A VariableReference construct.
///

The following keywords can/must be provided:

| **Keyword** | **Description**                  | **Type**                        | **Optional** |
| ----------- | -------------------------------- | ------------------------------- | ------------ |
| reference   | Name of the referenced variable. | [Variable.name](#name) | No           |

### reference

The _reference_ keyword specifies the name of the variable that is referenced.

!!! warning ""
    The variable reference must be an existing variable in scope.

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>