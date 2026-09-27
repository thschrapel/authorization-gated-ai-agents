# Sandbox Adapter — Protocol Adaptation for Unknown Planner Models

## 1. Purpose

The Sandbox Adapter provides a controlled execution environment for Planner models whose internal behavior, training, implementation, or protocol compliance is unknown.

Its purpose is to allow such a model to participate in an Authorization Agent architecture without requiring the model itself to natively implement the Authorization Agent protocol.

The Sandbox Adapter therefore acts as a mediation and protocol-adaptation layer between the model and the Authorization Agent.

The model may remain unaware of the internal authorization architecture while all relevant model-originated interactions are translated into the defined Authorization Agent interface.

## 2. Unknown Planner Model

A Planner executed through the Sandbox Adapter is not required to be trusted or protocol-native.

The model may be:

* locally executed,
* newly acquired,
* insufficiently evaluated,
* independently developed,
* otherwise unknown,
* or not specifically trained for operation within an Authorization Agent.

The architecture therefore does not assume that the Planner will voluntarily conform to the authorization protocol.

Instead, protocol conformance is established by the surrounding architecture.

This preserves the principle:

> **Authorization does not depend on Planner cooperation.**

## 3. Sandbox as Protocol Adaptation Layer

The Sandbox Adapter is more than an isolation mechanism.

It translates between:

1. the native interaction model of the Planner, and
2. the defined communication protocol of the Authorization Agent.

Conceptually:

```text
              Unknown Planner Model
                       │
                       │ native interaction
                       ▼
              ┌─────────────────────┐
              │   Sandbox Adapter   │
              │                     │
              │  protocol mediation │
              │  access mediation   │
              │  output mediation   │
              │  communication      │
              └──────────┬──────────┘
                         │
                  Authorization
                    Protocol
                         │
                         ▼
                Authorization Agent
                         │
                         ▼
                   Environment
```

The Planner therefore does not need to understand `ALLOW`, `DENY`, `CLARIFY`, or other Authorization Agent protocol semantics in order for its actions to be governed by the architecture.

## 4. Model-Originated Operations

All relevant operations originating from the Planner must pass through the Sandbox Adapter.

This includes, where applicable:

* information access,
* file reads,
* file writes,
* tool requests,
* external service requests,
* execution requests,
* communications,
* context requests,
* generated outputs,
* and other operations capable of affecting the protected environment.

The Sandbox Adapter must not provide an uncontrolled parallel interface to the protected environment.

The intended architectural property is:

```text
Planner
   │
   └── all relevant external interaction
                 │
                 ▼
          Sandbox Adapter
                 │
                 ▼
       Authorization Agent
                 │
                 ▼
        Protected Environment
```

## 5. Information Access Adaptation

A Planner may request information using its native interface.

For example:

```text
Planner:
    "Read /data/customer.db"
```

The Sandbox Adapter converts this interaction into an Authorization Agent request:

```text
INFORMATION_REQUEST
resource = /data/customer.db
```

The request is then evaluated through the normal authorization architecture.

The Planner therefore does not obtain information merely because its native interface permits the request.

The result is returned through the Sandbox Adapter in a representation appropriate to the Planner's native interface.

## 6. Clarification Adaptation

An unknown Planner may not understand the Authorization Agent's `CLARIFY` protocol.

This does not prevent the Authorization Agent from requiring clarification.

For example:

```text
Planner
   │
   │ "Read file X"
   ▼
Sandbox Adapter
   │
   │ INFORMATION_REQUEST
   ▼
Decisioner
   │
   │ CLARIFY
   ▼
Security Core
   │
   ▼
Sandbox Adapter
   │
   │ adapted clarification request
   ▼
Planner
```

The Sandbox Adapter translates the Core-defined clarification into an interaction that the Planner can process through its native interface.

The Planner's response is then translated back into the Authorization Agent protocol.

Thus:

> **Protocol adaptation allows a non-conforming Planner to participate in a conforming authorization process.**

The Sandbox Adapter does not decide whether clarification is sufficient.

The Security Core and Decisioner retain their existing responsibilities.

## 7. Denial and Authorization Boundaries

The same adaptation applies to authorization denial.

If the Decisioner returns:

```text
DENY
```

the Sandbox Adapter may translate this into the native interaction model of the Planner.

The Planner may interpret the denial, revise its plan, or request another operation.

However, the Planner cannot convert the denial into permission.

A rejected operation remains rejected regardless of how the Planner interprets the response.

## 8. Adapter Is Not an Authorization Authority

The Sandbox Adapter is not itself an authorization layer.

Its primary responsibilities are:

* mediation,
* protocol translation,
* interface adaptation,
* controlled exposure of resources,
* observation of model-originated interactions,
* and forwarding of requests and responses.

The authoritative authorization decision remains with the Security Core.

The architectural separation is therefore:

```text
Planner
   │
   ▼
Sandbox Adapter
   │
   ▼
Decisioner
   │
   ▼
Security Core
```

The Adapter does not replace the Decisioner and does not establish independent authorization policy.

## 9. No Direct Model-to-Environment Channel

A fundamental requirement of the Sandbox Adapter is the absence of an uncontrolled direct channel between the Planner and the protected environment.

The model must not be able to bypass the Adapter by using an alternative interface.

This includes attempts to obtain unauthorized information or effects through:

* alternative tools,
* undeclared interfaces,
* direct filesystem access,
* unauthorized network access,
* alternative communication channels,
* hidden subprocesses,
* or other mechanisms capable of circumventing the defined mediation boundary.

The exact technical mechanisms used to enforce this property are implementation-specific.

The architectural requirement is:

> **Relevant Planner-originated interaction with the protected environment must be mediated by the Sandbox Adapter and the Authorization Agent.**

## 10. Observation and Evaluation

The Sandbox Adapter also enables systematic evaluation of unknown Planner models.

Because model-originated interactions pass through the Adapter, the system can record and analyze behavior such as:

* information requests,
* denied requests,
* repeated requests,
* clarification responses,
* attempted authorization-boundary violations,
* alternative approaches after denial,
* tool usage,
* communication patterns,
* resource usage,
* and behavior across multiple iterations.

This allows evaluation of the Planner as an active system component rather than merely evaluating isolated model outputs.

The resulting observations may be used for model evaluation, security testing, red-teaming, protocol analysis, and controlled deployment decisions.

## 11. Behavioral Transparency

The Sandbox Adapter should minimize unnecessary behavioral distortion of the Planner.

Where possible, the Planner should interact with a stable and realistic interface corresponding to the environment in which it is intended to operate.

The purpose is not to make the model behave correctly by artificially constraining every possible interaction.

The purpose is to determine how the model behaves when its interactions are subject to the same authorization architecture that governs the intended deployment.

This allows the system to distinguish between:

* model capability,
* model behavior,
* protocol compliance,
* authorization compliance,
* and architectural enforcement.

## 12. Evaluation and Operational Modes

The same Adapter architecture may support different operating modes.

### Evaluation Mode

The Planner operates in a controlled environment in which interactions are observed and evaluated.

The environment may be simulated or isolated while preserving the relevant interfaces and authorization protocol.

### Operational Mode

The Planner operates against the intended production environment through the same architectural mediation pattern.

Conceptually:

```text
                    Planner Model
                         │
                         ▼
                 Sandbox Adapter
                         │
                ┌────────┴────────┐
                │                 │
                ▼                 ▼
        Evaluation Mode    Operational Mode
                │                 │
                ▼                 ▼
        Test Environment   Production Environment
                │                 │
                └────────┬────────┘
                         ▼
                 Authorization Core
```

Where practical, maintaining a common Adapter interface between evaluation and operational modes improves comparability between observed and deployed behavior.

## 13. Unknown Model Does Not Mean Unknown Authority

The Sandbox Adapter allows a distinction between model uncertainty and authorization uncertainty.

A model may be completely unknown while its available authority remains precisely defined.

For example:

```text
Model confidence:        UNKNOWN
Model capability:        UNKNOWN
Model intentions:        UNKNOWN

Authorization scope:     DEFINED
Security boundary:       DEFINED
Enforcement mechanism:   DEFINED
```

The architecture therefore does not require knowledge of the model's internal reasoning before enforcing the authorization boundary.

## 14. Sandbox and Safe Agent

The Sandbox Adapter is compatible with both Authorization Agents and Safe Agents.

In a Safe Agent, the Adapter remains subject to the additional normative safety architecture.

A Planner that attempts an action prohibited by a fundamental normative invariant cannot circumvent the Safe Agent merely because the Planner itself is unaware of that invariant.

The resulting architecture is:

```text
             Unknown Planner
                    │
                    ▼
            Sandbox Adapter
                    │
                    ▼
             Authorization
                 Agent
                    │
             ┌──────┴──────┐
             │             │
       normal security   Safe Agent
             │          normative safety
             │             │
             └──────┬──────┘
                    ▼
              Security Core
                    │
                    ▼
               Environment
```

A fundamental normative violation detected by the Safe Agent may therefore produce the existing non-maskable `HALT` path independently of whether the Planner understands the reason for the halt.

## 15. Architectural Principle

The Sandbox Adapter establishes the following principle:

> **An intelligent component does not need to be trusted, protocol-native, or fully understood in order to operate within a defined authorization architecture, provided that all relevant interactions are mediated through the architectural boundary.**

This allows the Authorization Agent to function as a safety and control envelope around models whose internal behavior is not yet sufficiently characterized.

## 16. Prototype Relevance

The Sandbox Adapter is a primary component for a prototype implementation of the Authorization Agent.

A prototype can therefore begin with an arbitrary or locally executed Planner model and expose only the Adapter interface to that model.

The prototype can then demonstrate:

```text
Unknown Model
      │
      ▼
Sandbox Adapter
      │
      ▼
Authorization Request
      │
      ▼
Decisioner
      │
      ▼
Deterministic Security Core
      │
      ├── ALLOW
      ├── DENY
      └── CLARIFY
             │
             ▼
       Sandbox Adapter
             │
             ▼
        Unknown Model
```

The prototype thereby tests the central architectural proposition without requiring the Planner itself to be modified or retrained.

The model remains an intelligent processing component.

The surrounding architecture remains responsible for authorization, enforcement, and security state transitions.
