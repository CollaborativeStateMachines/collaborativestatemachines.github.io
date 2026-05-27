---
title: Action
summary: Specifications of the Collaborative State Machines model
new: false
description: Specifications of the Collaborative State Machines model
keywords: collaborative,state,machines,specifications
author: metheredge
sidebar_title: Action
show_datetime: true
order: 9
---

## `Invoke`

An `Invoke` action calls an external service type, delegating processing, I/O operations, or hardware
interactions. It specifies a contract regarding inputs, outputs, and expected functionality rather than a
particular implementation.

The runtime system resolves execution placement and can select from multiple implementation variants based on
context. Input parameters are passed declaratively using `input` expressions. Output data is mapped back to
the state machine's accessible context variables. Upon completion, conditional events can be evaluated and
emitted asynchronously to trigger subsequent state transitions. Co-locating a state machine instance with a
local service can reduce communication overhead.

```pkl
new Invoke {
  type = "serviceType"
  mode = "remote"
  input = new Context { ... }
  output {
    "faceCount"
    "positions"
  }
  emits {
    new ConditionalEvent {
      provided = "faceCount > 5"
      event = new Event { topic = "alert" }
    }
  }
}
```

### Properties

| Property | Type                        | Multiplicity | Description                                                                                |
| :------- | :-------------------------- | :----------- | :----------------------------------------------------------------------------------------- |
| `type`   | `InvocationType`            | Required     | Specifies the requested service type name.                                                 |
| `mode`   | `InvocationMode`            | Optional     | Explicitly dictates execution placement (`"local"` or `"remote"`). Defaults to `"remote"`. |
| `input`  | `Context`                   | Optional     | A map of parameter names to value expressions passed as input data.                        |
| `output` | `Listing<VariableName>`     | Optional     | An ordered listing of variable names mapping outputs to accessible context variables.      |
| `emits`  | `Listing<ConditionalEvent>` | Optional     | Conditional event descriptions evaluated and raised upon invocation completion.            |

#### `type`

The `type` property defines the service type name targeted by the invocation.

<!-- prettier-ignore -->
!!! info "Service Binding"
    Service type validation and binding rules are implementation-specific.

#### `mode`

The `mode` property provides deployment routing guidance to the underlying runtime system. Setting
`mode = "local"` forces the runtime system to execute the targeted computation locally on the same physical
host as the state machine instance whenever feasible.

#### `input`

The `input` property maps data parameters into expressions evaluated against the active variable scope at the
moment of invocation.

#### `output`

The `output` property specifies destination variables within the state machine's hierarchy.

<!-- prettier-ignore -->
!!! danger "Constraint: Scope Accessibility"
    Target variable targets resolved via output listings must comply with upward-only containment rules and
    reside inside local, static, or persistent contexts.

#### `emits`

The `emits` property allows event-driven continuation by evaluating predicates against returning service
context data and dispatching subsequent events accordingly.

## `Eval`

An `Eval` action evaluates an explicit mathematical, logical, or functional string expression to update state
variables within the accessible variable scope.

```pkl
new Eval {
  expression = "++var"
}
```

### Properties

| Property     | Type         | Multiplicity | Description                                                           |
| :----------- | :----------- | :----------- | :-------------------------------------------------------------------- |
| `expression` | `Expression` | Required     | The expression string evaluated to manipulate runtime context values. |

## `Emit`

An `Emit` action creates and publishes an asynchronous event to facilitate reactive interaction among
distributed state machines or external systems.

```pkl
new Emit {
  event = new Event {
    topic = "release"
    channel = "external"
    data = new Context { ["id"] = "id" }
  }
  target = "arbitrator"
}
```

### Properties

| Property | Type               | Multiplicity | Description                                                                    |
| :------- | :----------------- | :----------- | :----------------------------------------------------------------------------- |
| `event`  | `EventDescription` | Required     | Explicit structural layout definition detailing topic channels and parameters. |
| `target` | `Expression`       | Optional     | Evaluated expression string indicating a unique recipient instance identifier. |

#### `target`

The optional `target` keyword explicitly designates a destiny for an emitted event.

## `Timeout`

A `Timeout` action schedules a future execution trigger. Configured inside the `after` structural block of
control states, it specifies a delay expression that fires an `Emit` action upon expiration.

```pkl
new Timeout {
  delay = "10"
  triggers = new Emit {
    event = new Internal { topic = "ate" }
  }
}
```

### Properties

| Property   | Type              | Multiplicity | Description                            |
| :--------- | :---------------- | :----------- | :------------------------------------- |
| `delay`    | `Expression`      | Required     | The timeout delay in milliseconds.     |
| `triggers` | `EmitDescription` | Required     | The emit action executed upon timeout. |

#### `delay`

<!-- prettier-ignore -->
!!! danger "Constraint: Numerical Evaluation"
    The delay expression must evaluate down cleanly into valid numeric or time units at runtime.

## `Reset`

A `Reset` action cancels pending timeouts based on a timeout name.

```pkl
new Reset {
  name = "timeout"
}
```

### Properties

| Property | Type          | Multiplicity | Description                                                      |
| :------- | :------------ | :----------- | :--------------------------------------------------------------- |
| `name`   | `TimeoutName` | Required     | Reference text key selecting the targeted timeout to deactivate. |

## `Match`

A `Match` action evaluates sequential logical guard predicates over application data elements to establish
conditional execution. It provides structured branch selection across an array of `Case` blocks alongside an
optional fallback `default`.

```pkl
new Match {
  cases = new Listing {
    new Case {
      of = "expression"
      yields {
        new ...Action { ... }
        ...
      }
    }
    ...
  }
  default = new ...Action { ... }
}
```

### Properties

| Property  | Type                       | Multiplicity | Description                                                              |
| :-------- | :------------------------- | :----------- | :----------------------------------------------------------------------- |
| `cases`   | `Listing<CaseDescription>` | Required     | An ordered array containing conditional branches to assess sequentially. |
| `default` | `ActionDescription`        | Optional     | A fallback action block triggered if all preceding guard tests fail.     |

## `Log`

A `Log` action provides formatting primitives for text monitoring, profiling metrics, and diagnostic tracking.

```pkl
new Log {
  message = "'Message'"
}
```

### Properties

| Property  | Type         | Multiplicity | Description               |
| :-------- | :----------- | :----------- | :------------------------ |
| `message` | `Expression` | Required     | String expression output. |

## `Instantiate`

An `Instantiate` action dynamically spawns new, parameterized instances of state machine classes at runtime.

```pkl
new Instantiate {
  instances {
    ["name"] = new InstanceDescription {
      stateMachineName = "stateMachine"
      data = new Context { ... }
    }
    ...
  }
}
```

### Properties

| Property    | Type                                     | Multiplicity | Description                                                                              |
| :---------- | :--------------------------------------- | :----------- | :--------------------------------------------------------------------------------------- |
| `instances` | `Mapping<DynamicInstanceName, Instance>` | Required     | A mapping block tracking the keys and initialization profiles of new dynamic components. |
