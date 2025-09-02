---
title: Execution Model
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Execution Model
show_datetime: true
order: 400
---

This section defines the CSM execution model. The execution model is defined as the execution of an individual
state machine. A collaborative state machine containing multiple (nested) state machines will concurrently
execute each individual state machine according to the execution model.

A state machine instance is associated with a _status_ $\mu = \langle c^+, \Gamma, \Theta \rangle$ that
captures its current situation, including its dynamic extent $c^+$, input events $\Gamma$, and
_active configuration_ $\Theta$.

The _active configuration_ $\Theta$ of the state machine is the set of states $s \in S$ that are currently
_active_. The _CSM step algorithm_ aims to produce a new status according to changes in the state machine's
environment that the state machine responds to.

The step algorithm reaches a new status by performing a _step_. We say that a step consists of an arbitrary
number of _micro-steps_ and that a step is completed yielding a new status whenever no micro-step exists for
the executed step.

The CSM execution model adheres to the following principles:

1. Changes that occur during a step are effective immediately.
2. An event is available during one step only and becomes unavailable upon completion of the step where it is
   handled.
3. A state machine is considered alive if the active configuration contains no terminal state.
4. The execution of a step and a micro-step may take time.
5. Actions defined are executed sequentially; a successive action is executed when the preceding action is
   completed.
6. One state must be active in the active configuration.

Principle 1 serves to ensure that changes to the state machine's status take effect immediately, avoiding
non-determinism by committing changes without delay, thus maintaining consistency and predictability in the
state machine's behavior.

Principle 2 serves to prevent redundant processing of events within a single step, ensuring that each event is
handled exactly once. By making events unavailable after processing in a step, this principle maintains the
integrity of event-driven behavior and avoids potential inconsistencies.

Principle 3 serves to define the lifetime of a state machine based on the absence of terminal states in its
active configuration. By considering a state machine alive if it has states to transition through, this
principle ensures that execution continues until a terminal state is reached, maintaining the ongoing
functionality of the state machine.

Principle 4 serves to acknowledge the potential time consumption during the execution of steps and
micro-steps. By recognizing that execution may take time, this principle allows for accommodating delays
caused by various factors, ensuring that the execution model remains robust and capable of handling
real-world scenarios where computational or external dependencies may affect execution time.

Principle 5 serves to enforce the sequential execution of actions defined within the state machine. By
ensuring that each action is executed only after the completion of the preceding action, this principle
maintains determinism and prevents race conditions or inconsistencies that could arise from concurrent
execution of actions, thereby preserving the integrity of state transitions and overall system behavior.

Principle 6 serves to avoid non-determinism, i.e., $|\{s \in \theta ∣ s \text{ is active}\}|=1$, by enforcing
that only one state can be active in the active configuration at any given time. By ensuring that the active
configuration always contains exactly one active state, this principle prevents ambiguity in state transitions
and guarantees predictable behavior within the state machine.

## Step algorithm

We define the step algorithm as follows.

**procedure** _ExecuteStep_

- **Input** Given the status $\mu$ of a state machine.
- **Ouput** Provide a new status $\mu$ of the state machine.

    1. $\textbf{while } \Theta \neq \emptyset \textbf{ do}$
        1. $x \leftarrow \text{Pop(}\Theta\text{)}$
        2. $\text{Handle(}x, \mu\text{)}$

Here, _1.1_ ensures that events in the event queue are removed before handling, and _1.2_ ensures that the
status is updated according to the behavior that follows from events in the event queue.

Before executing a single step, $\Theta$ is updated with respect to any input events in the environment or
collaborative state machine. External events, provided by the environment, are added to $\Theta$ before the
execution of a step. Timeout events are treated as external events.

We define the _Handle_ procedure as follows.

**procedure** _Handle_

- **Input** Given an input event $x$ and status $\mu$.
- **Output** Provide an updated status $\mu$ according to the event.

    1. Select the active _on_ transition $t$, given $x$ and $\mu$, or $\emptyset$ in case no active _on_
    transition exists.
    2. $\textbf{while } \delta \neq \emptyset \textbf{ do}$
        1. If $\delta$ is an external transition.
            1. Cancel any _while_ actions of the states exited due to $\delta$.
            2. Execute the _exit_ actions of the states exited due to $\delta$.
            3. Execute the actions of $\delta$.
            4. Execute the _entry_ actions of the states entered due to $\delta$.
            5. Execute the _while_ actions of the states entered due to $\delta$.
            6. Update the activate configuration.
            7. Set $\delta$ the active _always_ transition or $\emptyset$ in case no active _always_ transition 
            exists.
        2. Otherwise.
            1. Execute the actions of $\delta$.

An _on_ transition is selected (active) iff its source state (the state from which the transition starts) is
the active state, its guard conditions evaluates to true, and $x$ is the event that would trigger the
transition.

An _always_ transition is selected iff its source state (the state from which the transition starts) is the
active state and its guard condition evaluates to true.

The selection of a transition must yield exactly one transition. Conflicts arise whenever one event dictates
that the state machine should make two atomic states active. This is invalid and will result in
non-determinism.

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>