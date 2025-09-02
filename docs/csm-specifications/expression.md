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

Expression-related constructs are described below.

## Expression

An expression represents an evaluable one-line expression that serves to produce an output value.

Expressions are used throughout CSML to represent boolean expressions as well as data expressions. Depending 
on the expression language supported by the runtime system implementation, additional functionality may be 
available, such as the execution of functions, boolean logic, or arithmetic.

An expression is a string that is evaluated to acquire a certain value. 

```pkl
"5+5+v"
```
/// caption
Listing 4: An Expression construct.
///

!!! info ""
    The evaluation of an expression, e.g., the supported syntax, is implementation-specific.

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>