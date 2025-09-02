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

Guard-related constructs are described below.

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

| **Keyword** | **Description**       | **Type** | **Optional**         |
| ----------- | --------------------- | -------- |----------------------|
| expression  | The guard expression. | [Expression](expression.md)   | No                   |

### expression

The _expression_ keyword specifies the guard expression.

!!! warning ""
    The guard expression must evaluate to a boolean value.

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>