# Core Authorization Transition

The **Deterministic Authorization Core** converts an evaluated authorization state into an enforceable system state.

The Core does not independently determine whether a request is authorized. It enforces the authorization result produced by the applicable authorization process.

The fundamental transition is:

```text
REQUEST
   │
   ▼
AUTHORIZATION STATE
   │
   ▼
DETERMINISTIC AUTHORIZATION CORE
   │
   ├── ALLOW ──► AUTHORIZED STATE ──► EFFECT PERMITTED
   │
   ├── DENY ───► DENIED STATE ──────► NO EFFECT
   │
   ├── CLARIFY ► PENDING STATE ──────► NO EFFECT
   │
   └── UNKNOWN ► UNRESOLVED STATE ───► NO EFFECT
```

## 1. Core Transition Function

The Core may be modeled as a deterministic transition function:

```text
CORE(request_state, authorization_state)
        → next_state
```

with the fundamental mapping:

```text
ALLOW
    → AUTHORIZED

DENY
    → DENIED

CLARIFY
    → PENDING

UNKNOWN
    → UNRESOLVED
```

The resulting state determines whether the requested effect is permitted.

In particular:

```text
AUTHORIZED
    → effect may proceed

DENIED
    → effect must not proceed

PENDING
    → effect must not proceed

UNRESOLVED
    → effect must not proceed
```

Therefore:

> **Only an explicit authorization state can produce an authorized execution state.**

---

## 2. Default-Deny Property

The Core is default-deny.

Any state that does not explicitly establish authorization must not transition to an executable state.

Formally:

```text
authorization_state ≠ ALLOW
        ⇒
execution_state ≠ AUTHORIZED
```

Therefore:

```text
UNKNOWN ≠ ALLOW
CLARIFY ≠ ALLOW
DENY ≠ ALLOW
```

No absence of a denial may be interpreted as authorization.

---

## 3. Authorization-to-Effect Transition

The Core separates the **authorization decision** from the **effect**.

The transition is therefore:

```text
REQUEST
   ↓
DECISION
   ↓
CORE ENFORCEMENT
   ↓
EFFECT
```

not:

```text
REQUEST
   ↓
DECISION
   ↓
ASSUMED EFFECT
```

For example:

```text
Decisioner:
    DECISION = ALLOW

Core:
    STATE ← AUTHORIZED

Core:
    EFFECT ← PERMITTED
```

Whereas:

```text
Decisioner:
    DECISION = DENY

Core:
    STATE ← DENIED

Core:
    EFFECT ← BLOCKED
```

The Decisioner cannot directly cause the effect.

---

## 4. Core as the Security Boundary

The Core constitutes the final deterministic enforcement boundary between an authorization request and its possible effect.

Consequently:

```text
Planner
   │
   │ request
   ▼
Decisioner
   │
   │ authorization result
   ▼
Core
   │
   │ enforced transition
   ▼
Effect
```

The Planner cannot bypass the Core.

The Decisioner cannot bypass the Core.

The Objective-Creator cannot bypass the Core.

An authorization result has no operational effect until the Core has accepted and enforced the corresponding transition.

---

## 5. No Implicit Transitions

The Core must not infer authorization from:

* requester identity alone,
* previous authorization,
* previous successful execution,
* Planner intent,
* Decisioner rationale,
* absence of a denial,
* repeated requests,
* repeated clarification,
* or an asserted authority.

Each authorization-relevant transition must be explicit.

For example:

```text
Previous:
    ACTION A = ALLOW

Current:
    ACTION B = UNKNOWN
```

does not permit:

```text
ACTION B
```

Likewise:

```text
Objective = ALLOW
```

does not imply:

```text
Information = ALLOW
Action = ALLOW
```

Each authorization boundary requires its own transition.

---

## 6. Authorization State Is Bound to the Request

An authorization result should be bound to the specific request for which it was evaluated.

Conceptually:

```text
REQUEST_ID
    +
REQUEST_CONTENT
    +
REQUEST_CONTEXT
    +
AUTHORIZATION_RESULT
    =
AUTHORIZATION_RECORD
```

The Core must not treat an authorization result for one request as authorization for a materially different request.

A material change creates a new authorization-relevant state and may require re-evaluation.

---

## 7. Clarification Transition

`CLARIFY` is not an authorization state.

It is a controlled transition into a pending state:

```text
CLARIFY
    ↓
PENDING
    ↓
NO EFFECT
```

The Core may then return the clarification request to the appropriate request sender.

After a response:

```text
PENDING
    ↓
NEW REQUEST STATE
    ↓
DECISIONER
    ↓
ALLOW / DENY / CLARIFY / UNKNOWN
```

Thus clarification creates a new evaluation cycle rather than weakening the authorization boundary.

---

## 8. Unknown Transition

`UNKNOWN` must be explicitly represented.

The transition is:

```text
UNKNOWN
    ↓
UNRESOLVED
    ↓
NO EFFECT
```

It may subsequently lead to:

```text
UNRESOLVED
    ├──► CLARIFY
    ├──► ESCALATE
    └──► HALT
```

but never directly to:

```text
UNRESOLVED
    └──► AUTHORIZED
```

without an explicit authorization transition.

This establishes the invariant:

> **Unknown authorization is never permission.**

---

## 9. Monotonic Enforcement

Once the Core has entered a blocking state for a request, the blocked state cannot be bypassed by the requesting component.

For example:

```text
DENIED
```

cannot be transformed into:

```text
AUTHORIZED
```

by the Planner, Objective-Creator, or another downstream component.

A new authorization evaluation may produce a new authorization result for a new or materially modified request, but that constitutes a new transition rather than an override of the previous one.

---

## 10. Core Invariant

The central invariant of the Core is:

```text
NO EXPLICIT AUTHORIZATION
        ⇒
NO EFFECT
```

or formally:

```text
¬ExplicitAllow(request)
        ⇒
¬Effect(request)
```

The complementary rule is:

```text
ExplicitAllow(request)
        ⇒
Effect MAY proceed
```

not:

```text
ExplicitAllow(request)
        ⇒
Effect MUST occur
```

Authorization permits an effect; it does not itself constitute the effect.

Therefore:

> **Authorization is not execution.**

---

## 11. Architectural Principle

The Core transition can therefore be summarized as:

```text
REQUEST
   ↓
EVALUATION
   ↓
EXPLICIT AUTHORIZATION STATE
   ↓
DETERMINISTIC CORE TRANSITION
   ↓
AUTHORIZED / BLOCKED SYSTEM STATE
   ↓
EFFECT OR NO EFFECT
```

The essential separation is:

> **The Decisioner decides. The Core transitions. The Core alone permits the effect.**
