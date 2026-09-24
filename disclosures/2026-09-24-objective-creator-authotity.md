# Objective-Creator Authority

## Authority Model for Objective Submission

**Type:** Technical Architecture Extension / Prior-Art Publication
**Publication date:** 2026-09-24
**Extends:** Authorization Agent v1.0 and Objective Authorization Extension
**Status:** Public technical disclosure

---

## 1. Purpose

This document defines the authority of the **Objective-Creator** within the Authorization Agent architecture.

The Objective-Creator is the source of an Objective, but the ability to create or submit an Objective does not by itself authorize the resulting Objective to be pursued.

The fundamental distinction is:

> **Authority to submit an Objective is not authority to authorize the Objective.**

The Objective-Creator therefore has a defined role in the authorization protocol, while the Decisioner and Deterministic Authorization Core retain their respective security functions.

---

# 2. Objective-Creator

The **Objective-Creator** is the entity or component that originates an Objective for the Agent.

It may be:

* a human,
* an authenticated software component,
* another Agent,
* an external system,
* an automated process,
* or another explicitly recognized source.

The identity of the Objective-Creator should be available to the authorization system.

The Objective-Creator is therefore an explicit protocol participant.

---

# 3. Authority to Submit

The Objective-Creator may possess authority to **submit** an Objective to the Agent.

This authority means:

> The Objective-Creator is permitted to ask the Agent to evaluate a particular Objective.

It does **not** mean:

> The Objective-Creator is permitted to cause the Agent to pursue that Objective.

The latter requires a separate authorization decision.

Conceptually:

```text
Objective-Creator
        │
        │ authority to submit
        ▼
 Objective Request
        │
        ▼
    Decisioner
        │
        │ objective authorization
        ▼
Authorization Core
```

---

# 4. Explicit Authority Outcomes

The authority of the Objective-Creator to submit a particular Objective request should be represented explicitly.

At minimum, the authority state should distinguish:

```text
AUTHORIZED
NOT AUTHORIZED
UNKNOWN / UNRESOLVED
```

These states have distinct meanings.

### 4.1 AUTHORIZED

The system has established that the Objective-Creator possesses the required authority to submit the particular Objective request.

This does not authorize the Objective itself.

It only establishes:

> **The request may enter the Objective Authorization process.**

### 4.2 NOT AUTHORIZED

The system has established that the Objective-Creator does not possess the required authority to submit the particular Objective request.

The request must not proceed to Objective Authorization.

The Core therefore enforces:

```text
AUTHORITY = NOT AUTHORIZED
        ↓
DENY SUBMISSION
        ↓
NO OBJECTIVE EVALUATION FOR EXECUTION
```

An explanatory rationale may optionally be provided by the Decisioner.

### 4.3 UNKNOWN / UNRESOLVED

The system cannot establish that the Objective-Creator possesses the required authority.

This state must not be interpreted as authorization.

Therefore:

```text
AUTHORITY = UNKNOWN
        ↓
CLARIFY / ESCALATE / HALT
```

The system must not silently convert an unresolved authority question into permission to submit.

---

# 5. Authority Evaluation Is Separate from Objective Evaluation

Two distinct questions must be evaluated:

```text
Question 1:
"Is this Objective-Creator authorized to submit this request?"

Question 2:
"Is this Objective authorized to be pursued?"
```

These questions may produce different outcomes.

For example:

```text
Objective-Creator Authority
        = AUTHORIZED

Objective Authorization
        = DENY
```

or:

```text
Objective-Creator Authority
        = UNKNOWN

Objective Authorization
        = not reached
```

The second evaluation must not be performed as though the first were automatically positive.

The resulting sequence is:

```text
Objective-Creator Authority
          │
     ┌────┼────┐
     ▼    ▼    ▼
  ALLOW  DENY UNKNOWN
     │     │      │
     │     │      └── CLARIFY / HALT
     │     └───────── DENY
     │
     ▼
Objective Authorization
```

---

# 6. Authority Is Not Transitive

Authority possessed by the Objective-Creator must not automatically propagate to the Planner.

For example:

```text
Objective-Creator:
"I am authorized to request this."

        ≠

Planner:
"I am therefore authorized to access everything
required to accomplish this objective."
```

Likewise:

```text
Objective-Creator authority
        ≠
Objective authorization
        ≠
Planner information authorization
        ≠
Action authorization
```

Each boundary requires its own applicable authorization evaluation.

---

# 7. Objective-Creator Authority Scope

The authority of an Objective-Creator may be constrained by:

* identity,
* role,
* authenticated capability,
* environment,
* organizational policy,
* resource ownership,
* objective type,
* requested scope,
* time,
* target system,
* or other authorization attributes.

The Objective-Creator may therefore be authorized to submit some classes of Objectives but not others.

For example, an Objective-Creator may have authority to submit:

```text
OBJECTIVE TYPE A
```

while lacking authority to submit:

```text
OBJECTIVE TYPE B
```

The Decisioner evaluates the applicable authority and context.

---

# 8. Objective Authorization Remains Independent

Even where the Objective-Creator is authenticated and authorized to submit an Objective, the Objective itself remains subject to Decisioner evaluation.

Thus:

```text
Authenticated Objective-Creator
            │
            ▼
     Authority Evaluation
            │
            ▼
         AUTHORIZED
            │
            ▼
     Objective Request
            │
            ▼
        Decisioner
            │
       ┌────┼────┐
       ▼    ▼    ▼
     ALLOW DENY CLARIFY
```

Authentication of the sender does not replace authorization of the Objective.

This preserves the distinction between:

> **Who is allowed to ask?**

and:

> **What is allowed to be pursued?**

---

# 9. Objective-Creator and Clarification

If the authority evaluation returns `UNKNOWN / UNRESOLVED`, the clarification request is returned to the Objective-Creator that originated the request, unless the applicable protocol explicitly specifies another authorized source.

The clarification request is itself a controlled protocol transition.

Conceptually:

```text
Objective-Creator
       │
       ▼
Objective Request
       │
       ▼
Authority Evaluation
       │
       ▼
    UNKNOWN
       │
       ▼
Authorization Core
       │
       ▼
Objective-Creator
       │
       ▼
Authority Clarification
       │
       ▼
Authority Evaluation
```

If authority is established, the Objective may proceed to its separate authorization evaluation.

If authority remains unresolved, the Core must not permit the request to proceed merely because clarification has been repeated.

---

# 10. Clarification Does Not Expand Authority

A clarification response may provide additional information about the authority of the Objective-Creator.

It does not automatically expand that authority.

For example:

```text
Original authority scope
        +
additional explanation
        ≠
expanded authority
```

If the clarification establishes a materially different authority context, that context must itself be evaluated.

The Objective-Creator cannot obtain additional authority merely by restating the request.

---

# 11. Objective Modification

An Objective-Creator may be permitted to modify an Objective after a `CLARIFY` request.

However, a material modification should be treated as a new authorization-relevant state.

A system should therefore be able to distinguish:

```text
clarification of existing Objective
```

from:

```text
materially changed Objective
```

A material change may require a new authority evaluation as well as a new Objective Authorization evaluation.

This prevents an initially authorized request from being gradually transformed into a substantially different request without re-evaluation.

---

# 12. Separation of Authority Layers

The architecture therefore distinguishes at least four authority questions:

```text
1. May this entity submit an Objective?
          │
          ▼
2. May this Objective be pursued?
          │
          ▼
3. May this component receive the required information?
          │
          ▼
4. May this concrete action be executed?
```

These questions may have different answers.

An affirmative answer at one level does not automatically imply an affirmative answer at another.

Each layer should have an explicit authorization state rather than relying on implicit inheritance.

---

# 13. Kernel Enforcement

The Objective-Creator's authority is represented as authorization-relevant information available to the Decisioner.

The Decisioner evaluates that information.

The Deterministic Authorization Core enforces the resulting state transition.

The Objective-Creator therefore cannot directly:

* authorize its own Objective,
* bypass the Decisioner,
* bypass the Core,
* grant the Planner additional authority,
* authorize information release,
* or authorize execution merely by asserting authority.

The fundamental boundary remains:

> **The requester may request. The Decisioner evaluates. The Core enforces.**

---

# 14. Auditability

The authority of the Objective-Creator should be recorded with the Objective request.

A protocol record may contain:

```text
request_id
request_sender
sender_identity
sender_role
sender_authority_scope
authority_decision
authority_evidence
objective
objective_scope
objective_decision
decisioner_rationale
authorization_evidence
core_result
clarification_history
final_status
timestamp
```

This allows an auditor to distinguish:

* who submitted the Objective,
* under what authority it was submitted,
* whether that authority was established,
* what Objective was actually evaluated,
* what the Decisioner determined,
* what the Core enforced,
* and whether the Objective changed during clarification.

---

# 15. Fundamental Principle

The Objective-Creator is the **source of an Objective**, not the ultimate authority over the Agent.

The architectural principle is:

> **Objective-Creator authority establishes the right to submit a request; it does not establish the right to have the request fulfilled.**

This preserves the separation between:

```text
REQUEST AUTHORITY
        ↓
OBJECTIVE AUTHORIZATION
        ↓
INFORMATION AUTHORIZATION
        ↓
ACTION AUTHORIZATION
        ↓
EXECUTION
```

Each transition remains subject to the applicable security architecture.

---

## Initial Disclosure of This Extension

**Date:** 2026-09-24

**Extension:** Objective-Creator Authority

**Base Architecture:** Authorization Agent v1.0, 2026-09-23

**Related Extension:** Objective Authorization, 2026-09-24

**Purpose:** Public technical disclosure / prior-art publication
