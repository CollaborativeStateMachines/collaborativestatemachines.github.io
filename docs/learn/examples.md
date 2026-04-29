---
title: Examples
summary: Implementation use cases and benchmarks for Collaborative State Machines.
new: false
description: Technical examples of CSM implementations ranging from concurrency benchmarks to distributed logic.
keywords: collaborative, state machines, examples, distributed systems, benchmarks, IoT
author: metheredge
sidebar_title: Examples
show_datetime: false
order: 2
---

# Implementation Examples

The CSM project includes a suite of reference implementations designed to demonstrate the modeling of
coordination logic using the **Cirrina** runtime and **CSML**. These examples illustrate how the framework
decouples high-level application logic from underlying infrastructure, applicable to both high-throughput
routing and safety-critical controllers.

For a detailed technical guide on building these applications, refer to the [Tutorial](tutorial.md).

### Examples

The following implementations are provided to evaluate various aspects of distributed coordination and
performance.

| Example                  | Focus Area          | Technical Objective                                                                                         |
| :----------------------- | :------------------ | :---------------------------------------------------------------------------------------------------------- |
| **Big**                  | Routing             | Evaluates many-to-many message routing and ingress congestion under high pressure.                          |
| **Chameneos**            | Communication       | Analyzes symmetrical communication and mediator bottlenecks during stateful pairings.                       |
| **Cigarette Smokers**    | Coordination        | Models multi-resource management with interdependent requirements via a central arbitrator.                 |
| **Dining Philosophers**  | Resource Allocation | Demonstrates deadlock avoidance and neighbor-dependency management in a distributed context.                |
| **Dynamic Philosophers** | Runtime Scaling     | A decentralized Chandy-Misra variant testing system growth through the addition of participants at runtime. |
| **Sleeping Barber**      | Contention          | Examines state-based synchronization and stability within bounded waiting environments.                     |
| **Ping Pong**            | Latency             | Establishes a baseline for sequential message-passing and round-trip overhead.                              |
| **Count**                | Performance         | A fundamental benchmark used to measure core message-processing throughput.                                 |

### Source Access

All source code, configuration instructions, and deployment artifacts are maintained in the:
**[Examples Repository](https://github.com/CollaborativeStateMachines/Cirrina-Examples)**
