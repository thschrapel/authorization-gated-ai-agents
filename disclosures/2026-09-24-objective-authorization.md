# Objective Authorization

## Architectural Extension to the Authorization Agent

**Type:** Technical Architecture Extension / Prior-Art Publication
**Publication date:** 2026-09-24
**Extends:** Authorization Agent v1.0, initially disclosed 2026-09-23
**Status:** Public technical disclosure

---

## 1. Purpose

This document extends the **Authorization Agent** architecture disclosed in v1.0.

The extension introduces explicit authorization of the **Objective** itself.

The fundamental principle is:

> **An objective is not automatically authorized merely because it was provided to the Agent.**

Before an objective is passed into the planning process, the Decisioner evaluates whether the objective may be pursued.

The objective therefore becomes the first authorization-controlled element in the Agent's operational chain.

---

# 2. Objective as an Authorization-Relevant Request

The Objective is treated as an explicit request originating from an identifiable component or entity referred to as the:

> **Objective-Creator**

The Objective-Creator may be a:

* human,
* software system,
* external application,
* another Agent,
* automated process,
* or other authorized source.

The Objective-Creator is distinct from the Planner.

The Planner does not create the original objective merely by interpreting it.

The Planner receives an objective only after the applicable authorization process has been satisfied.

---

# 3. Objective Authorization

The Decisioner evaluates the Objective before the Planner is permitted to act upon it.

Conceptually:

```text
Objective-Creator
        │
        │ OBJECTIVE REQUEST
        ▼
 ┌─────────────┐
 │  Decisioner │
 └──────┬──────┘
        │
        ▼
Authorization Core
        │
   ┌────┼─────┐
   ▼    ▼     ▼
 ALLOW DENY CLARIFY
   │    │       │
   │    │       └────────► Objective-Creator
   │    │
   │    └───────────────► no planning
   │
   ▼
 Planner
```

The Planner therefore cannot transform an unauthorized objective into an authorized task by choosing a particular implementation.

---

# 4. Decision Outcomes

The Decisioner may return at least three principal states:

```text
ALLOW
DENY
CLARIFY
```

The Deterministic Authorization Core enforces the corresponding state transition.

### 4.1 ALLOW

If the Objective is authorized, the Core permits the Objective to proceed to the next architectural stage.

The Planner may then receive the authorized Objective, subject to any separate information-access restrictions.

### 4.2 DENY

If the Objective is not authorized, the Core prevents the Objective from entering the planning process.

The formal result is:

```text
DENY
```

The Decisioner may additionally provide an explanatory rationale.

The rationale is informational and does not itself constitute the authorization decision.

Conceptually:

```text
DECISIONER
    DECISION: DENY
    RATIONALE: optional explanatory information

CORE
    EFFECT: DENY
```

The distinction is important:

> **The Decisioner determines the decision. The Core enforces the decision.**

### 4.3 CLARIFY

If the Decisioner cannot establish sufficient authorization for the Objective, it may return:

```text
CLARIFY
```

The Core then generates or forwards a structured clarification request to the Objective-Creator.

The Planner does not receive the Objective as an executable planning task merely because clarification has been requested.

---

# 5. Clarification of Objectives

Clarification follows the same architectural principles as clarification of Planner requests.

A clarification request may contain:

```text
CLARIFY_REQUEST
request_id
original_request_id
request_sender
request_type
unresolved_issue
required_information
required_evidence
clarification_scope
iteration_number
timestamp
```

For an Objective clarification:

```text
REQUEST-SENDER: Objective-Creator
REQUEST-TYPE: OBJECTIVE
DECISION: CLARIFY
CLARIFICATION: <required clarification>
```

The Objective-Creator may respond with a structured clarification:

```text
REQUEST-SENDER: Objective-Creator
REQUEST-TYPE: OBJECTIVE-CLARIFICATION
RESPONSE-TO: <request_id>
RESPONSE: <clarification>
```

The response is then evaluated again by the Decisioner.

---

# 6. Controlled Clarification Loop

Clarification does not create an unrestricted retry mechanism.

A clarification cycle may be bounded by:

* maximum iterations,
* time limits,
* authorization policy,
* escalation rules,
* or other deterministic constraints.

Conceptually:

```text
Objective-Creator
        │
        ▼
    Decisioner
        │
    ┌───┼────┐
    │   │    │
 ALLOW DENY CLARIFY
    │   │    │
    │   │    ▼
    │   │  Core
    │   │    │
    │   │    ▼
    │   │ Objective-Creator
    │   │    │
    │   │ clarification
    │   │    │
    │   │    ▼
    │   │ Decisioner
    │   │
    │   └────► HALT
    │
    ▼
 Planner
```

Repeated clarification must not be interpreted as an increasing probability of authorization.

Each iteration represents a new evaluation of the supplied information.

---

# 7. Symmetry of Authorization Requests

The Objective authorization process follows the same general architecture as later authorization requests.

The system may therefore treat:

* Objective requests,
* information-access requests,
* action requests,
* tool requests,
* and other authorization-relevant operations

as instances of a common request/decision/enforcement pattern.

Conceptually:

```text
REQUEST
   │
   ▼
Decisioner
   │
   ├── ALLOW ───► Core ───► EFFECT
   │
   ├── DENY ────► Core ───► NO EFFECT
   │
   └── CLARIFY ─► Core ───► REQUEST-SENDER
                              │
                              ▼
                           RESPONSE
                              │
                              ▼
                          Decisioner
```

The architecture therefore does not privilege the Objective merely because it occurs at the beginning of the process.

---

# 8. Request-Sender as an Explicit Protocol Element

Every authorization-relevant request should identify its sender.

Examples include:

```text
REQUEST-SENDER: Objective-Creator
REQUEST-SENDER: Planner
REQUEST-SENDER: System Component
```

The sender identity is part of the auditable authorization context.

This allows the system to distinguish between:

```text
Objective-Creator → Objective
```

and:

```text
Planner → Information Request
```

or:

```text
Planner → Action Request
```

The authorization decision may depend on the identity and role of the sender.

---

# 9. Auditability

Objective authorization and clarification should be fully logged.

A corresponding protocol record may contain:

```text
timestamp
request_id
request_sender
request_type
objective
decisioner_decision
decisioner_rationale
authorization_evidence
core_result
clarification_request
clarification_response
iteration_number
final_status
```

This permits reconstruction of the complete authorization history.

For example:

```text
OBJECTIVE REQUEST
        │
        ▼
Decisioner: CLARIFY
        │
        ▼
Clarify Request #1
        │
        ▼
Objective-Creator Response
        │
        ▼
Decisioner: CLARIFY
        │
        ▼
Clarify Request #2
        │
        ▼
Objective-Creator Response
        │
        ▼
Decisioner: ALLOW
        │
        ▼
Core: ALLOW
        │
        ▼
Planner receives Objective
```

Alternatively:

```text
OBJECTIVE REQUEST
        │
        ▼
Decisioner: DENY
        │
        ▼
Core: DENY
        │
        ▼
Planning never begins
```

---

# 10. Information Boundary

Objective authorization does not automatically authorize unrestricted disclosure of the Objective or its associated context to every component.

The Decisioner may have access to the full relevant Objective and supporting information.

The Planner receives only the information that is authorized for its role.

Therefore:

> **Authorization of an Objective and authorization to access information about that Objective are separate decisions.**

This preserves the information boundary established in Authorization Agent v1.0.

---

# 11. Architectural Principle

This extension establishes the following principle:

> **Authorization begins before planning.**

The system must not assume that every received objective is a legitimate task.

The correct sequence is:

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

Each stage remains subject to the same fundamental rule:

> **Planning is not authorization.**

And:

> **Unknown authorization is not permission.**

---

# 12. Relationship to Authorization Agent v1.0

This document does not replace or modify the original v1.0 disclosure.

It extends it by introducing explicit authorization of the Objective and by defining the Objective-Creator as an identifiable request sender.

The original architectural separation remains unchanged:

```text
Program Space
    Planner

Kernel / Security Architecture
    Decisioner
    Deterministic Authorization Core
```

The Objective Authorization extension adds an authorization-controlled entry point into this architecture.

---

## Initial Disclosure of This Extension

**Date:** 2026-09-24

**Extension:** Objective Authorization

**Base Architecture:** Authorization Agent v1.0, 2026-09-23

**Purpose:** Public technical disclosure / prior-art publication
