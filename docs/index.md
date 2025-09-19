---
hide:
  - navigation
---

# Collaborative State Machines

Collaborative State Machines (CSM) is a programming model for building **reactive, distributed applications**
🌐 that run seamlessly across the **cloud, edge, and IoT**.

Modern applications are increasingly dynamic, stateful, and distributed across multiple environments, which
traditional programming models often struggle to handle. CSM addresses this challenge by letting you design
applications as **collections of autonomous state machines that collaborate** 🤝. Each machine can run in the
cloud, at the edge, or directly on IoT devices, while the system continues to operate as a single cohesive
application.

---

## About us

CSM is a research project developed by the [Distributed and Parallel Systems Group](https://dps.uibk.ac.at/)
🏛️ at the University of Innsbruck.

As a research initiative, CSM aims to create a **disruptive, reactive programming model** ⚡ for distributed
applications that span the cloud, edge, and IoT. Our goal is to enable developers to build systems that are
**responsive, stateful, and scalable**, while remaining simple to reason about and deploy in real-world
environments.

The project explores how collections of **collaborating state machines** can provide a cohesive, event-driven
view of a distributed application, supporting automatic scheduling, deployment, and optimization across
heterogeneous computing layers.

By combining theory with practical experimentation, CSM seeks to bridge the gap between **academic research**
📚 and **real-world distributed systems** ⚙️, providing a model that is both rigorous and applicable.

!!! success "Research"
    Explore our [Research](research/publications.md) and meet the [Team](research/team.md) to learn more about
    the people behind CSM.

---

## State machines

At the heart of CSM are **small, autonomous state machines** 🔹 that form the building blocks of distributed
applications. Each state machine maintains its own state, responds to events, and can trigger actions or
external services. When combined, these machines create a **cohesive, reactive system** capable of operating
across cloud, edge, and IoT environments without being tied to specific hardware or infrastructure.

State machines in CSM are designed to:

- **React to events** ⚡ from sensors, other machines, or the surrounding environment, enabling timely and
  context-aware responses.

- **Maintain their own state** 🗂️ while optionally interacting with persistent or shared data, providing
  consistency and traceability.

- **Trigger actions or services** 🛠️ directly within states and transitions, allowing state-driven execution
  of business logic or external operations.

- **Collaborate seamlessly** 🤝 by exchanging events and data with other machines, supporting coordinated
  behavior across distributed components.

This approach makes it easier to reason about **complex, distributed, event-driven logic**, ensuring that
applications remain predictable, maintainable, and scalable while leveraging the full power of reactive
programming.

!!! question "Learn"
    Learn more about the fundamental programming model in the CSM
    [Specifications](csm-specifications/version.md).

---

## Features

CSM provides a set of core capabilities that make building distributed, event-driven applications more
structured and manageable:

- **Resilience** 🌱 &mdash; applications adapt dynamically to changing conditions and can recover gracefully from
  partial failures, ensuring robust operation across heterogeneous environments.

- **Modularity** 🧩 &mdash; each state machine encapsulates a distinct piece of behavior, simplifying
  understanding, maintenance, and extension of the overall system.

- **True distribution** 🌍 &mdash; state machines can run seamlessly across cloud, edge, and IoT devices without
  requiring manual handling of communication, coordination, or deployment details.

- **Separation of concerns** ⚖️ &mdash; application logic is expressed independently of infrastructure, allowing
  developers to focus on *what* the system should do rather than *how* it is deployed or executed.

- **Structured data management** 📦 &mdash; local, transient, and persistent data are handled with well-defined
  scopes and lifecycles, supporting consistency and observability throughout the application.

This combination of features ensures that applications built with CSM are **scalable, maintainable, and
predictable**, while taking full advantage of distributed execution environments.

!!! question "Learn"
    Get started quickly by following our [getting started guide](learn/getting-started.md).

---

## Use cases

CSM is particularly well-suited for systems that are **distributed, event-driven, and stateful**. Its reactive
and modular approach enables developers to design applications that respond efficiently to changes in their
environment while maintaining consistent state across heterogeneous components. Common use cases include:

- **IoT & Edge Solutions** 📡 &mdash; devices react to local events with minimal latency while coordinating and
  syncing with cloud services for analytics, logging, or further processing.

- **Surveillance & Monitoring** 🎥 &mdash; distributed data streams are collected, processed, and acted upon in
  real time, supporting automated alerts and operational insights.

- **Smart Factories** 🏭 &mdash; workflows orchestrate machines, sensors, and analytics pipelines to optimize
  production, reduce downtime, and maintain safety.

- **Large-Scale Cloud Systems** ☁️ &mdash; scalable applications with a clear separation of business logic and
  infrastructure, supporting multiple distributed components without sacrificing maintainability or 
  observability.

This flexibility makes CSM a versatile programming model for **any scenario requiring reliable, reactive, and
coordinated distributed behavior**.

!!! question "Learn"
    Discover more in our [Examples](learn/examples.md) or follow the [Tutorial](learn/tutorial.md) to see CSM
    in action.
