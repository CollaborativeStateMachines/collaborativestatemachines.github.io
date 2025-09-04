Collaborative State Machines (CSM) is a programming model for building **reactive, distributed applications**
that run across the **cloud, edge, and IoT**.  

Traditional programming models often struggle with applications that are **dynamic, stateful, and spread 
across the cloud, edge, and IoT**. CSM addresses this by letting you design applications as **collections of
state machines that collaborate with each other**. Each state machine can run in the cloud, at the edge, or
directly on IoT devices, yet all machines continue to operate together as one cohesive system.

## About Us

CSM is developed by the [Distributed and Parallel Systems Group](https://dps.uibk.ac.at/) at the University of
Innsbruck.

!!! note ""
    +streamline-emojis:woman-scientist-2;height=2em+ Learn more about our [research](research/publications.md)
    and meet the [team](research/team.md).

## State Machines

At the heart of CSM are **state machines**. Each one is small, autonomous, and easy to reason about. Together,
they form powerful distributed applications. State machines can:

- **React to events** from their environment or from other machines.
- **Maintain their own state**, while also working with persistent shared data.
- **Trigger actions or external services** directly inside states and transitions.
- **Collaborate** by exchanging events and data with other machines.

This approach makes complex logic easier to manage without tying your application to specific compute nodes,
services, or execution environments.

!!! note ""
    +streamline-emojis:green-book;height=2em+ Learn more about the fundamental programming model through the CSM
    [specifications](csm-specifications/specifications.md).

## Features

CSM helps developers tackle complexity with:

- **Resilience** &mdash; applications adapt to changing environments and recover from failures.
- **Modularity** &mdash; behavior is encapsulated, making systems easier to understand and extend.
- **True distribution** &mdash; run across cloud, edge, and IoT without extra complexity.
- **Separation of concerns** &mdash; cleanly express application logic, decoupled from infrastructure.
- **Structured data management** &mdash; handle local, static, and persistent data with well-defined scope and
  lifetime.

!!! note ""
    +streamline-emojis:rocket;height=2em+ Learn more about [getting started](learn/getting-started.md).

## Use Cases

CSM is ideal for any system that is **distributed, event-driven, and stateful**. Example use cases include:

- **IoT & Edge Solutions** &mdash; devices reacting to local events while syncing with cloud services.
- **Surveillance & Monitoring** &mdash; distributed data streams processed in real time.
- **Smart Factories** &mdash; workflows coordinating machines, sensors, and analytics.
- **Large-Scale Cloud Systems** &mdash; scalable applications with clear separation of business logic and
  infrastructure.

!!! note ""
    +streamline-emojis:vertical-traffic-light;height=2em+ For more, see our [examples](learn/examples.md).