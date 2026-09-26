# Authorization Agent

## Architectural Extensions for Autonomous AI Systems

**Type:** Defensive Technical Disclosure / Prior-Art Publication
**Initial publication date:** 2026-09-26
**Status:** Public technical disclosure
**Revision:** v1.1
**Extends:** Authorization Agent v1.0

---

# 1. Purpose of This Revision

This disclosure extends the architecture defined in v1.0.

The v1.0 architecture established the separation between:

```text
Planning
    ↓
Authorization Evaluation
    ↓
Deterministic Authorization
    ↓
Effect
```

This revision extends that architecture with additional roles and process structures for autonomous, iterative systems.

The principal extensions are:

* explicit authorization of an Objective before planning,
* explicit authority of the Objective-Creator,
* separation of Planner and Counselor as peer optimization modules,
* iterative process control through the Counselor,
* process-cost information,
* explicit iteration results,
* separation of Context Log and Audit Log,
* Security Core deployment across local, remote, software, and hardware boundaries,
* and an optional higher-level Security Counsel.

These extensions do not replace the authorization boundary defined in v1.0.

---

# 2. Objective as an Authorization Object

An Objective is not merely an instruction to the Planner.

The Objective itself may require authorization before planning begins.

The resulting sequence is:

```text
Objective
    ↓
Objective Authorization
    ↓
Planning
    ↓
Information Authorization
    ↓
Action Authorization
    ↓
Execution
```

The Planner therefore does not receive authority merely because an Objective exists.

An Objective may be:

```text
ALLOW
DENY
CLARIFY
UNKNOWN / UNRESOLVED
```

The exact policy semantics remain Core-defined.

The fundamental rule remains:

> **An Objective is not authorized merely because it has been submitted.**

---

# 3. Objective-Creator Authority

The source submitting an Objective is explicitly distinguished from the Objective itself.

The Objective-Creator may be:

* a human,
* an authenticated software component,
* another Agent,
* an external system,
* or an automated process.

Two independent questions therefore exist:

```text
1. Is the Objective-Creator authorized to submit this request?

2. Is the submitted Objective authorized to be pursued?
```

Authority to submit an Objective does not imply authorization of the Objective.

Likewise:

> **Authority is not automatically inherited by the Planner.**

The Objective-Creator may request.

The authorization architecture evaluates.

The Core enforces.

---

# 4. Planner and Counselor as Peer Modules

The Planner and Counselor are not inherently hierarchical components.

Both may be highly capable AI modules operating at comparable levels of competence.

Their distinction is their optimization objective.

```text
Planner
    → optimizes the next iteration

Counselor
    → optimizes the process trajectory
```

The Planner's central question is:

> **What should be done next?**

The Counselor's central question is:

> **Should the process continue, and if so, under what process conditions?**

Neither module derives authorization authority from its competence.

Therefore:

```text
Competence ≠ Authority
Optimization ≠ Authorization
```

---

# 5. Planner Responsibility

The Planner defines the instruction for an opened iteration.

It may:

* decompose the current problem,
* select strategies,
* propose actions,
* request authorized information,
* identify dependencies,
* interpret previous results,
* and construct an `ITERATION_INSTRUCTION`.

The Planner does not determine whether another iteration should occur.

The Planner does not determine whether its instruction is authorized.

The Planner remains subject to the existing authorization architecture.

---

# 6. Counselor Responsibility

The Counselor is responsible for process-level optimization.

Its objective may be expressed as:

> **Complete the authorized Objective successfully in as few iterations and with as little unnecessary resource expenditure as reasonably possible.**

The Counselor may consider:

* progress toward the Objective,
* results of previous iterations,
* actual process costs,
* estimated next-step costs,
* expected information gain,
* expected value of further work,
* diminishing returns,
* available resources,
* and the quality of partial results.

The Counselor may request:

```text
NEXT_ITERATION_REQUEST
BREAK_REQUEST
GOAL_STATUS_REQUEST
```

The Counselor does not itself authorize execution.

---

# 7. Process Cost Information

The Counselor requires information about the resources consumed by the process.

At minimum, process-cost information distinguishes between:

```text
ACTUAL_COST
    = resources already consumed

ESTIMATED_NEXT_COST
    = resources expected to be consumed by the next process step
```

Possible cost dimensions include:

* computation,
* time,
* energy,
* financial cost,
* external service usage,
* quota consumption,
* and other Core-defined resources.

The actual cost is historical process information.

The estimated cost is a prediction and may include uncertainty.

Where possible, costs should be attributable to:

```text
process_id
iteration_id
instruction_id
effect_id
```

This allows estimated and actual costs to be compared.

---

# 8. Process Optimization and Waste Prevention

A process may remain authorized while additional work becomes economically or computationally unjustified.

An iteration can therefore be:

* authorized,
* technically valid,
* well-intentioned,

while nevertheless having insufficient expected value to justify another iteration.

The Counselor may therefore distinguish between:

```text
DIFFICULT BUT PRODUCTIVE
```

and:

```text
CONTINUED EFFORT WITH INSUFFICIENT EXPECTED VALUE
```

Counselor quality includes the ability to prevent unnecessary resource expenditure.

The objective is not:

```text
MINIMIZE ITERATIONS
```

and not:

```text
MAXIMIZE ITERATIONS
```

but rather:

```text
MAXIMIZE EXPECTED VALUE
SUBJECT TO
AUTHORIZATION AND AVAILABLE RESOURCES
```

---

# 9. Partial Results

A process does not need to completely achieve its Objective to produce a useful result.

The system may therefore preserve and present partial results.

A partial result may contain:

* completed sub-results,
* verified findings,
* identified constraints,
* unresolved questions,
* failed approaches,
* remaining work,
* or bounded conclusions.

A partial result must not be represented as complete achievement of the Objective.

Therefore:

```text
RESULT ≠ OBJECTIVE ACHIEVEMENT
```

and:

```text
PARTIAL RESULT ≠ FAILED PROCESS
```

A process may terminate with a useful partial result.

---

# 10. Break Requests

A `BREAK_REQUEST` is a process-control request.

It may contain a structured rationale describing why continued effort is not expected to provide sufficient additional value.

Possible rationale categories include:

* diminishing returns,
* insufficient expected information gain,
* repeated unsuccessful approaches,
* excessive expected cost,
* resource exhaustion,
* time constraints,
* unavailable dependencies,
* unresolved questions,
* or degraded expected result quality.

The Counselor may recommend or request a break.

The Core remains responsible for establishing the resulting process state.

---

# 11. Iterative Process Protocol

The Counselor communicates process requests to the Core.

Defined request types include:

```text
NEXT_ITERATION_REQUEST
BREAK_REQUEST
GOAL_STATUS_REQUEST
```

Corresponding Core responses may include:

```text
ITERATION_OPEN
CONTINUATION_DENIED
ITERATION_LIMIT_REACHED
BREAK_ACCEPTED
BREAK_REJECTED
PROCESS_HALTED
GOAL_STATUS_RESPONSE
```

These process messages are not themselves authorization decisions.

The Core alone establishes the authoritative process state.

---

# 12. Iteration Cycle

A complete iteration may be represented conceptually as:

```text
Counselor
    │
    │ NEXT_ITERATION_REQUEST
    ▼
Core
    │
    │ ITERATION_OPEN
    ▼
Planner
    │
    │ ITERATION_INSTRUCTION
    ▼
Decisioner
    │
    │ authorization result
    ▼
Core
    │
    │ authorized effect
    ▼
Effect
    │
    │ result
    ▼
Core
    │
    │ ITERATION_RESULT
    ▼
Counselor
```

The result of the completed iteration is processed by the Core before becoming process information available to the Counselor.

The result is not routed directly from the Effect to the Planner.

This preserves the Core as the authoritative boundary between effect execution and process state.

---

# 13. Iteration Result

The Core may establish an authoritative `ITERATION_RESULT` containing, at minimum:

```text
ITERATION_RESULT {
    message_type
    process_id
    objective_id
    iteration_id
    instruction_id
    effect_id
    result_status
    result
    timestamp
}
```

`result_status` describes the technical result of the iteration, for example:

```text
SUCCESS
FAILURE
PARTIAL
NO_RESULT
```

It does not establish that the Objective has been achieved.

Objective achievement remains a separate process question.

The result of the preceding iteration may become explicit input to a subsequent:

```text
NEXT_ITERATION_REQUEST
```

---

# 14. Context Log and Audit Log

The architecture distinguishes between the **Context Log** and the **Audit Log**.

The Context Log contains the authoritative relevant authorization context available to the Decisioner.

The Audit Log contains the authoritative security and process history controlled by the Core.

Conceptually:

```text
Context Log
    → What must the Decisioner know to evaluate?

Audit Log
    → What must the Core exclusively control as authoritative history?
```

The Decisioner may access the relevant Context Log through the defined Core interface.

The Decisioner does not thereby obtain unrestricted access to the Audit Log.

Program-space components maintain their own working contexts.

No program-space component may rewrite authoritative Core-controlled security history.

---

# 15. Protocol Language and Communication Boundary

Communication across protected architectural interfaces occurs using Core-defined protocol languages and representations.

The Core may define a fixed set of permitted languages or protocol encodings.

Components may not create undeclared communication channels through:

* alternative languages,
* undocumented symbolic encodings,
* hidden shorthand,
* uncontrolled encryption,
* or other obfuscation mechanisms.

Cryptographic mechanisms may be used where explicitly defined and controlled by the Core.

The purpose is to prevent the communication protocol itself from becoming an unmonitored authority or information channel.

---

# 16. Security Core as Platform Architecture

The Program Space / Kernel Space distinction is an architectural model and is not restricted to conventional PCs.

The Decisioner and Deterministic Authorization Core may be implemented as:

* privileged software,
* kernel components,
* firmware,
* secure processors,
* dedicated security controllers,
* hardware authorization mechanisms,
* or combinations of software and hardware.

The essential requirement is functional rather than physical:

> **The AI components must not be able to bypass the authorization and enforcement boundary.**

---

# 17. Platform Independence

The architecture may be implemented on:

* PCs,
* servers,
* smartphones,
* tablets,
* embedded computers,
* robotics systems,
* vehicles,
* industrial controllers,
* regulation and control hardware,
* appliances,
* network equipment,
* autonomous systems,
* or other programmable hardware.

Planner and Counselor components may execute locally or remotely.

They may operate in Program Space, in a sandbox, on another computer, or through web-based services.

Their physical location does not determine their authority.

---

# 18. Manufacturer-Integrated Security Core

A hardware manufacturer may integrate the Security Core into a platform.

The manufacturer may define which capabilities are exposed through the Security-Core interface.

The platform may then permit AI agents to use hardware capabilities through that interface without granting the AI unrestricted control of the underlying hardware.

Conceptually:

```text
AI Agent
    │
    │ request
    ▼
Security Core
    │
    │ authorized effect
    ▼
Protected Hardware
```

This permits AI systems to operate powerful hardware while retaining an independent authorization and enforcement boundary.

The architecture therefore distinguishes:

> **what the AI may request**

from:

> **what the platform will actually permit.**

---

# 19. External Security Counsel

A Security Core may require additional security expertise when its local context is insufficient to determine the security meaning or consequences of an Objective or execution plan.

The Core may therefore obtain advice from a separate **Security Counsel**.

The Security Counsel may operate:

* locally,
* on a separate trusted processor,
* on another device,
* within manufacturer infrastructure,
* or as a remote high-level security service.

The communication occurs through a Core-controlled secure channel.

Only the information necessary for the security question should be disclosed.

Where possible, information may be:

* minimized,
* abstracted,
* anonymized,
* aggregated,
* or otherwise transformed.

---

# 20. Security Counsel Function

The Security Counsel may assist with:

* security interpretation of Objectives,
* analysis of complex execution plans,
* identification of security-relevant dependencies,
* identification of missing conditions,
* identification of materially different interpretations,
* recommendation of additional clarification,
* recommendation of interruption or additional security review,
* and identification of unresolved security concerns.

The Security Counsel may therefore provide information that helps the Core formulate a more precise clarification request.

For example:

```text
CLARIFY:
"Specify whether authorization includes operation X
under condition Y for resource Z."
```

The exact `CLARIFY` lifecycle is intentionally not fully defined by this revision.

---

# 21. Security Counsel Is Distinct from Counselor

The Security Counsel and Counselor are separate roles.

```text
Counselor
    → process optimization

Security Counsel
    → security analysis
```

The Counselor may determine that further effort has insufficient expected value.

The Security Counsel may determine that the security implications of continued execution are insufficiently understood.

Both may provide recommendations concerning interruption, but their optimization objectives are different.

---

# 22. Security Counsel Does Not Become the Decision Authority

Security Counsel advice does not constitute authorization.

The Security Counsel does not directly execute operations.

It does not directly establish the authorization state of the protected platform.

The architectural relationship is:

```text
Security Counsel
        │
        │ advice
        ▼
Security Core
        │
        │ authoritative processing
        ▼
Authorization / Process State
```

The Core remains the authoritative security boundary.

---

# 23. Responsibility for Security Decisions

The Security Counsel provides advice.

The Security Core remains responsible for the authoritative security decision and the resulting state transition.

This remains true whether the Core:

* does not request external advice,
* requests advice,
* accepts advice,
* rejects advice,
* receives incomplete advice,
* or receives an incorrect recommendation.

Therefore:

> **A Security Counsel recommendation does not transfer the Core's decision responsibility to the Counsel.**

A failure of Security Counsel advice does not itself become the authorization decision.

The Core remains the component responsible for the state transition it authoritatively establishes.

This is an architectural responsibility statement and does not define legal liability.

---

# 24. Security Counsel Identity

The Core must be able to authenticate a Security Counsel according to its defined security protocol.

A Security Counsel identity may represent:

* a trusted security service,
* a manufacturer-controlled security instance,
* a certified security component,
* or another explicitly trusted security entity.

Trust may be scoped according to:

* security domain,
* platform,
* information class,
* protocol,
* or other Core-defined properties.

Identity of the Security Counsel does not itself authorize the requested operation.

```text
Counsel Identity ≠ Action Authorization
```

---

# 25. Processing of Security Counsel Advice Is Intentionally Undefined

This specification does not define how a particular Security Core must process Security Counsel advice.

In particular, this specification does not prescribe:

* whether advice produces `ALLOW`,
* whether advice produces `DENY`,
* whether advice produces `CLARIFY`,
* whether advice produces `BREAK`,
* whether advice is binding,
* how conflicting advice is resolved,
* or how specific security policies map advice to Core state transitions.

Those semantics belong to the implementation and policy of the Security Core.

The architectural requirement is the separation:

```text
Security Counsel → advice
Core             → authoritative processing
```

---

# 26. No Authority Transitivity

Authority must not be inferred merely from participation in the Security Counsel relationship.

In particular:

```text
Security Counsel advice
        ≠
Authorization
```

and:

```text
Secure communication
        ≠
Authorization
```

The Security Counsel provides additional security intelligence.

The Core determines the significance of that intelligence within its own defined security architecture.

---

# 27. Responsibility and External Services

A Security Core may use external AI or security services without transferring its architectural responsibility for authorization.

An external service may provide:

* analysis,
* classification,
* interpretation,
* risk information,
* or recommendations.

The Core remains the enforcement boundary.

Thus:

```text
External Intelligence
        ↓
Security Core
        ↓
Authoritative Decision
        ↓
Enforcement
```

The use of external intelligence does not transform the external service into the authorization authority.

---
# 28. Architectural Composition

The Security Architecture and the Processing Architecture are composed of independent and strictly separated components.

The resulting architecture therefore contains multiple modules that separate capability and authorization along orthogonal dimensions.

The authorization of an Objective is performed by the Security Core before the Processing Architecture is activated.

```text
                         ┌───────────────────┐
                         │  Objective-Creator│
                         └─────────┬─────────┘
                                   │
                              Objective
                                   │
                                   ▼
                    ┌───────────────────────────┐
                    │       SECURITY CORE       │
                    │                           │
                    │  Objective Authorization  │
                    │  Decisioner               │
                    │  Deterministic Core       │
                    │                           │
                    │  Context Log              │
                    │  Audit Log                │
                    └─────────────┬─────────────┘
                                  │
                          authorized Objective
                                  │
                                  ▼
                    ┌───────────────────────────┐
                    │      PROGRAM SPACE        │
                    │                           │
                    │   Planner      Counselor  │
                    │      │             │      │
                    └──────┼─────────────┼──────┘
                           │             │
                      instruction   process control
                           │             │
                           └──────┬──────┘
                                  ▼
                    ┌───────────────────────────┐
                    │       SECURITY CORE       │
                    │                           │
                    │  Decisioner               │
                    │  Deterministic            │
                    │  Authorization Core       │
                    │                           │
                    │  Context Log              │
                    │  Audit Log                │
                    └─────────────┬─────────────┘
                                  │
                                  ▼
                           Protected Effect
```

### Objective Clarification

```text
        Objective-Creator
               ▲
               │
               │  Objective Clarification
               │
               ▼
          Security Core
```

The **Objective Authorization** phase is part of the Security Architecture and occurs before the Processing Architecture is activated.

If the Objective cannot be sufficiently evaluated, the Security Core may initiate a `CLARIFY` cycle with the Objective-Creator.

The Objective-Creator may provide additional information or clarification, which is returned to the Security Core for renewed evaluation.

Only after the Objective has reached the Core-defined authorized state may the Processing Architecture begin.

### Security Counsel

```text
                    Security Core
                         │
                         │ Anonymized
                         │ Security Request
                         ▼
                  Security Counsel
                         │
                         │ Security Counsel Advice
                         ▼
                    Security Core
```

The Security Counsel may optionally support the Security Core during such security evaluations through its separate advisory channel.

The Security Counsel is **not part of the normal execution path**.

It is an optional advisory path that may be invoked by the Security Core when additional security analysis is required.

The Security Counsel receives only the information provided through the Core-defined security interface.

The Security Core remains the authoritative security and enforcement boundary throughout both Objective Authorization and subsequent execution.

# 29. Fundamental Role Separation

The architecture can be summarized as:

```text
Planner
    → WHAT NEXT?

Counselor
    → CONTINUE OR BREAK?

Security Counsel
    → IS THE SECURITY MEANING SUFFICIENTLY UNDERSTOOD?

Decisioner
    → IS IT AUTHORIZED?

Core
    → WHAT STATE TRANSITION IS PERMITTED AND ENFORCED?
```

These questions are related but not interchangeable.

A component's competence in one domain does not grant it authority in another.

---

# 30. Extended Fundamental Principles

The v1.0 principles remain in force.

The following principles are added:

1. **An Objective may itself be an authorization object.**
2. **Objective-Creator authority is distinct from Objective authorization.**
3. **Planner and Counselor may be peer modules with different optimization objectives.**
4. **The Planner optimizes the next instruction.**
5. **The Counselor optimizes the process trajectory.**
6. **Process continuation should consider both actual and estimated resource costs.**
7. **A useful partial result does not require complete Objective achievement.**
8. **A Break may be a process-optimization decision rather than an authorization denial.**
9. **Security Counsel is distinct from Counselor.**
10. **Security Counsel provides security intelligence, not implicit authorization.**
11. **Security Counsel advice does not transfer Core responsibility.**
12. **The Core remains the authoritative security and enforcement boundary.**
13. **Security Counsel may be local or remote and may operate at a higher security level.**
14. **Security Counsel communication should disclose only decision-relevant information.**
15. **The semantics by which a Security Core processes Counsel advice are implementation-defined unless explicitly specified by a particular Core.**
16. **Program Space and Kernel/Security Architecture are functional boundaries and need not correspond to a particular physical device.**
17. **The architecture may be implemented across general-purpose and specialized hardware.**
18. **Additional intelligence must not become an implicit authorization channel.**

---

# 31. Non-Goals of v1.1

This revision intentionally does not define:

* the complete Safe Agent invariant set,
* a complete `CLARIFY` protocol,
* a universal legal-classification system,
* a universal Security Counsel policy,
* mandatory semantics for Security Counsel recommendations,
* a universal process-cost model,
* a universal goal-achievement algorithm,
* or a particular hardware implementation.

These may be addressed by future architectural extensions.

---

# 32. Architectural Summary

The architecture described in v1.1 extends the Authorization Agent from a primarily action-gated system into a composable autonomous process architecture.

The system separates:

```text
Objective
Objective Authorization
Planning
Process Optimization
Security Analysis
Authorization Evaluation
Authorization Enforcement
Execution
Result Processing
```

while preserving the central boundary established in v1.0:

> **Planning is not authorization.**

and:

> **Unknown authorization is not permission.**

The extended architecture further establishes:

> **The Planner optimizes the next instruction.**

> **The Counselor optimizes the process trajectory.**

> **The Security Counsel provides additional security intelligence.**

> **The Decisioner evaluates authorization.**

> **The Core establishes and enforces the authoritative state transition.**

The architecture therefore permits increasingly capable AI components without requiring any individual AI component to possess unrestricted authority over the system on which it operates.

The central security property remains:

> **Intelligence may be distributed. Authorization and enforcement remain explicitly bounded.**

---

## Revision

**v1.1 — 2026-09-26**

This revision extends the initial v1.0 disclosure with:

* Objective authorization,
* Objective-Creator authority,
* Counselor process control,
* process-cost optimization,
* iteration/result protocol,
* Context Log / Audit Log separation,
* platform and hardware deployment,
* Security Counsel,
* Security Counsel identity and placement,
* and explicit responsibility boundaries between advisory intelligence and Core enforcement.
