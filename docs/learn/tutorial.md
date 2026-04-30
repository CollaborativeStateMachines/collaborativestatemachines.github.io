---
title: Tutorial
summary: How to implement a Collaborative State Machines-based application?
new: false
description: How to implement a Collaborative State Machines-based application?
keywords: collaborative,state,machines,tutorial
author: metheredge
sidebar_title: Tutorial
show_datetime: true
order: 1
---

The Collaborative State Machines model is created around the concept of a _collaborative state machine_, the
root of a hierarchical structure consisting of state machines, their nested state machines, and states. At
each level of this hierarchy, data can be declared that is accessible locally as transient data, as well as
data declared at the root which acts as persistent data. This architecture represents a hierarchy designed
specifically to describe distributed systems that span many distributed resources, allowing a developer to
focus on the entire system instead of fragmented components. Each state machine is an autonomous, reactive
entity that interacts with external services and other machines through events defined in a high-level
declarative language called CSML using [Pkl](https://pkl-lang.org/) syntax.

## Collaborative state machine

To begin implementing an application, you first declare the `collaborativeStateMachine` root. This block
serves as the container for the entire system logic. In this specific implementation, we define a global
`message` variable within the `persistent` block. This variable acts as a shared memory space that different
state machine instances will populate during their execution lifecycle.

```pkl
collaborativeStateMachine {
  persistent { ["message"] = "''" }
  stateMachines {
    // State machine definitions will follow here
  }
}
```

## State machine

The next step involves defining the individual state machine logic within the root. The first machine, named
`one`, is designed to initiate the workflow. Upon entry into its initial state `a`, it appends a string to the
global `message` variable and emits an event with the topic `advance` specifically targeting the second state
machine. It then waits for a returning `advance` event to transition to state `b`, where it logs the final
accumulated message and terminates.

```pkl
    ["one"] {
      states {
        ["a"] = new Initial {
          entry {
            new Eval { expression="message += 'Hello, '" }
            new Emit { event { topic="advance" }; target="'two'" }
          }
          on {
            ["advance"] { to = "b" }
          }
        }
        ["b"] = new Terminal {
          entry {
            new Log { message="message" }
          }
        }
      }
    }
```

Following the first machine, you define the reactive behavior of any subsequent machines. Machine `two`
remains in its initial state `a` until it receives the `advance` event from the first machine. Once triggered,
it moves to state `b`, appends its own contribution to the global `message`, and emits an event back to
machine `one` to signal that its phase of the collaborative process is complete.

```pkl
    ["two"] {
      states {
        ["a"] = new Initial {
          on {
            ["advance"] { to = "b" }
          }
        }
        ["b"] = new Terminal {
          entry {
            new Eval { expression="message += 'World!'" }
            new Emit { event { topic="advance" }; target="'one'" }
          }
        }
      }
    }
```

## Instantiation

The final step in the implementation is the instantiation of the defined state machine classes. By adding an
`instances` block, you tell the CSM runtime to create active instances of the logic blocks defined previously.
This bridges the gap between the declarative class definition and the actual execution of the distributed
system.

```pkl
instances {
  ["one"] { stateMachineName = "one" }
  ["two"] { stateMachineName = "two" }
}
```

## More examples

Discover more in the [Examples](../examples) gallery.
