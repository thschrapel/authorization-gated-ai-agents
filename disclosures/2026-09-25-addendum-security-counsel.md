# Security-Core Addendum — External Security Counsel

## 1. Security Counsel for Unresolved Authorization

The Security Core may encounter authorization requests for which the available decision context is insufficient to produce a sufficiently precise authorization decision.

This may occur not only because information is missing, but because the security significance of the available information is difficult to determine locally.

In such cases, the Security Core may obtain advice from a separate **Security Counsel** operating at a higher-level security or policy instance.

The Security Counsel is an advisory component.

It does not replace the Decisioner and does not obtain authority to directly authorize an effect.

---

## 2. Security Interpretation of Objectives

An Objective may be syntactically clear while remaining security-semantically ambiguous.

For example, an Objective may describe a desired outcome without making sufficiently clear:

* what resources would have to be affected,
* what classes of actions may be required,
* what indirect effects may arise,
* what security boundaries may be crossed,
* or which interpretations of the Objective are materially different from a security perspective.

The Security Counsel may assist the Security Core in analyzing such Objectives.

Its task is not to redefine the Objective.

Rather, it may identify security-relevant interpretations, missing conditions, or ambiguities that require clarification before authorization can be determined.

Thus:

```text
Objective
    ↓
local security analysis
    ↓
if insufficient:
    Security Counsel
    ↓
security interpretation
    ↓
precise Clarify request
    ↓
new authorization evaluation
```

---

## 3. Analysis of Complex Execution Plans

The same principle applies to complex execution plans.

A plan may be technically coherent while its security implications are difficult for the local security system to determine.

The Security Counsel may therefore analyze a proposed plan with respect to:

* security-relevant dependencies,
* indirect effects,
* resource interactions,
* escalation paths,
* boundary crossings,
* cumulative effects,
* exceptional execution paths,
* and other Core-defined security properties.

The Security Counsel may identify aspects of a plan that require additional information before the Core can make a sufficiently precise authorization decision.

It does not thereby approve the plan.

---

## 4. Secure and Minimized Communication

Communication between the Security Core and the Security Counsel occurs through a Core-controlled secure channel.

Only information necessary for resolving the security question should be transmitted.

Where possible, identifying or otherwise unnecessary information is removed, abstracted, or anonymized before transmission.

Conceptually:

```text
Protected Platform
       │
       │ unresolved security question
       ▼
Security Core
       │
       │ minimized / anonymized context
       │ secure channel
       ▼
Security Counsel
       │
       │ security assessment
       ▼
Security Core
```

The Security Counsel therefore receives a **decision-relevant representation**, rather than unrestricted access to the platform's internal context.

---

## 5. Advisory Function

The Security Counsel may assist the Security Core by:

* identifying security-relevant interpretations of an Objective,
* identifying security-relevant properties of a complex execution plan,
* identifying missing authorization conditions,
* distinguishing materially different interpretations,
* identifying information required for a decision,
* recommending a more precise clarification,
* recommending a security-oriented process break,
* identifying whether further security escalation is appropriate,
* or recommending that the request remain unresolved.

The Security Counsel does not directly execute an operation.

It does not directly modify the authorization state of the protected platform.

---

## 6. Security Recommendation to Break

The Security Counsel may identify circumstances in which further execution should not proceed until a security question has been resolved.

It may therefore recommend a security break or suspension.

Such a recommendation is advisory.

The Core remains responsible for establishing the actual process state.

This creates a distinction between the two forms of Counsel:

```text
Counselor
    → evaluates whether continued process effort is worthwhile

Security Counsel
    → evaluates whether continued execution is sufficiently
      security-understood to proceed
```

Both may recommend termination or interruption, but for different reasons.

The Counselor primarily optimizes the process.

The Security Counsel primarily protects the security decision boundary.

---

## 7. More Precise Clarification

The Security Counsel may contribute to a subsequent `CLARIFY` decision.

Instead of issuing a broad clarification request such as:

```text
CLARIFY:
"Please provide more information."
```

the Security Core may formulate a more specific request based on Security Counsel input:

```text
CLARIFY:
"Specify whether authorization includes operation X
under condition Y for resource Z."
```

This can reduce unnecessary clarification cycles while preserving the principle that unresolved authorization is not permission.

The exact protocol and lifecycle of such a `CLARIFY` cycle remain subject to further definition.

---

## 8. Security Counsel Does Not Resolve Authorization

The Security Counsel may improve the information available for an authorization decision.

It does not become the authorization decision-maker merely because it possesses greater contextual or analytical capability.

The distinction remains:

```text
Security Counsel → security analysis
Decisioner       → authorization evaluation
Core             → authoritative state transition
```

In particular:

```text
Security Counsel advice
        ≠
Authorization
```

---

## 9. Information Minimization

The Security Counsel interface should follow the principle of minimum necessary disclosure.

The Core should disclose only information required for the security analysis.

Possible transformations include:

* removal of identity information,
* removal of unrelated objective information,
* abstraction of resource identifiers,
* aggregation of quantitative information,
* replacement of sensitive values with categories,
* and other Core-defined anonymization mechanisms.

The purpose is to allow higher-level security expertise to be consulted without unnecessarily expanding the trusted information boundary.

---

## 10. No Authority Transitivity

Authority does not flow from the Security Counsel back through the communication channel.

In particular:

```text
Security Counsel advice
        ↓
Core evaluation
        ↓
explicit authorization state
```

and never:

```text
Security Counsel advice
        ↓
implicit authorization
```

Likewise, the existence of a secure channel does not itself establish authorization.

---

## 11. Escalation Without Boundary Expansion

Security Counsel provides a mechanism for the Security Core to obtain additional security expertise when its local decision context is insufficient.

This permits a system to distinguish between:

```text
UNKNOWN
    ↓
additional security analysis
    ↓
precise clarification / security break / escalation
    ↓
new authorization evaluation
```

rather than treating uncertainty as permission.

The mechanism therefore extends the **decision-support capability** of the Security Core without extending the **authorization boundary**.

---

## 12. Architectural Principle

The Security Counsel introduces a controlled higher-level advisory path:

> **When the local security system cannot sufficiently determine the security meaning or consequences of an Objective or execution plan, the Security Core may obtain additional security expertise through a secure, information-minimized channel.**

The Security Counsel may contribute to:

* understanding the security implications of an Objective,
* analyzing a complex execution plan,
* identifying missing conditions,
* recommending a security break,
* or formulating a more precise Clarify request.

The decisive property remains:

> **Additional intelligence may improve the quality of the authorization process without becoming an alternative authorization mechanism.**

