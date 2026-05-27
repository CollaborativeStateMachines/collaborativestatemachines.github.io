---
title: Data model
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Data model
show_datetime: true
order: 400
---

## Data Scoping and Visibility

In Collaborative State Machines, data visibility and lifetime are determined strictly by the
application's hierarchical containment structure. Data scoping relies on an **upward-only resolution**
mechanism.

A component can access its locally declared variables as well as any variables declared by its direct
ancestors up to the root. Conversely, a component can never access variables declared by its siblings or its
descendants. This guarantees structural isolation and enforces strict information hiding. For example, a
control state can read its parent state machine's data, but a state machine cannot peek into the data of its
nested states.

When a variable is referenced, the runtime locates it by searching the accessible scope: it checks the local
component first, then traverses upward through the ancestor hierarchy, and finally checks the global
persistent data.

#### Data Categories

To map data lifetime to this structural hierarchy, CSM categorizes data into five distinct classes:

1. **Persistent Data**: Declared at the root of the collaborative state machine. This data is globally
   accessible to all nested components, serving as the application-wide coordination state (though it may
   incur distributed synchronization overhead).
2. **Transient Data**: Declared at the state machine level. This data remains strictly local to that specific
   state machine instance, bypassing global synchronization and isolating it from sibling state machines.
3. **Static Data**: Declared within individual control states. This data is scoped exclusively to the state
   and persists across repeated entries/activations of that state, maintaining local operational history.
4. **Event Data**: Ephemeral payload dictionaries attached to emitted events. These facilitate asynchronous
   data propagation between state machines.
5. **Service Data**: Structured input and output parameters exchanged during external `Invoke` actions. This
   forms the explicit boundary between the state machine's internal coordination logic and external computation.

#### Data Manipulation via Expressions

Data within the accessible scope is manipulated declaratively using expressions. Expressions are utilized to
evaluate and update variables (via `Eval` actions), define conditional progression rules (via `Match` guards),
map dynamic inputs to external services, or populate event payloads.
