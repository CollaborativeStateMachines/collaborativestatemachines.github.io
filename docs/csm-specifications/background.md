---
title: Background
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Background
show_datetime: true
order: 0
---

Collaborative State Machines is a distributed programming model for the cloud-edge-IoT. It represents
distributed applications as state machines that can be distributed. The model is designed for reactive,
stateful, and highly dynamic applications that must operate in heterogeneous environments.

While CSM defines the abstract programming model, its applications are expressed in the associated language,
CSML. Like the model itself, CSML is abstract and can be realized in different ways, for example through
serialization formats or domain-specific languages. In these specifications, we describe CSML and the CSM
concepts using the [Pkl](https://pkl-lang.org/)-based implementation of CSML employed by the official runtime
system, [Cirrina](../develop/runtime-system.md).

Within CSML a description consists of _constructs_, such as those describing a collaborative state machine,
state machine and state. A construct consists of one or multiple _keywords_.
