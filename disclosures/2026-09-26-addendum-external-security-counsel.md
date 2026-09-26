# Security-Core Addendum — External Security Counsel

## 1. Security Counsel for Unresolved Security Questions

The Security Core may encounter requests for which the available decision context is insufficient to produce a sufficiently precise security assessment.

This may occur because information is missing, because the security significance of available information is difficult to determine, or because an Objective or execution plan is sufficiently complex that additional security expertise is useful.

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

Rather, it may identify security-relevant interpretations, missing conditions, or ambiguities that may require further clarification or security evaluation.

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

The Security Counsel may identify aspects of a plan that require additional information before the Core can make a sufficiently precise security assessment.

It does not thereby approve the plan.

---

## 4. Secure and Minimized Communication

Communication between the Security Core and the Security Counsel occurs through a Core-controlled secure channel.

Only information necessary for the security question should be transmitted.

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
* identifying missing security conditions,
* distinguishing materially different interpretations,
* identifying information required for further evaluation,
* recommending a more precise clarification,
* recommending interruption or further security review,
* or recommending that the security question remain unresolved.

The Security Counsel does not directly execute an operation.

It does not directly modify the authorization state of the protected platform.

---

## 6. Security Advice and Process Decisions

Security Counsel advice may concern whether continued processing or execution should proceed.

For example, the Security Counsel may provide advice indicating:

* that further information should be obtained,
* that a security concern remains unresolved,
* that further analysis is appropriate,
* or that continuation should be reconsidered.

Such advice may be particularly relevant where an Objective has potentially serious security implications or where a complex execution plan cannot be sufficiently evaluated from the local context.

The Security Core may use such advice according to its own defined security architecture.

---

## 7. Processing of Security Counsel Advice Is Not Defined Here

This architecture does **not** define how an individual Security Core must process Security Counsel advice.

In particular, this document does not prescribe:

* whether particular advice results in `ALLOW`, `DENY`, `CLARIFY`, `HOLD`, `BREAK`, or another state,
* whether specific classes of Security Counsel advice are binding,
* whether advice is advisory only,
* how conflicting advice is resolved,
* whether multiple Counsel instances may be consulted,
* or which security policies determine the effect of Counsel advice.

These semantics belong to the implementation and policy of the Security Core.

The architectural requirement is only that the Security Counsel remains within the defined advisory interface and that the Core remains the authoritative enforcement boundary.

---

## 8. Responsibility for the Authorization Decision

The Security Counsel provides advice.

The Security Core makes and enforces the authoritative security decision.

Consequently, the architectural responsibility for a security decision remains with the Security Core, regardless of whether the Core:

* acts without external advice,
* requests advice from a Security Counsel,
* accepts that advice,
* rejects that advice,
* or receives an incorrect or incomplete recommendation.

In particular:

> **A Security Counsel recommendation does not transfer the Core's decision responsibility to the Counsel.**

A mistaken recommendation by the Security Counsel therefore does not become an authorization decision merely because it was received by the Core.

The Core remains responsible for the state transition that it authoritatively establishes.

This distinction is fundamental:

```text
Security Counsel → provides advice
Security Core    → makes the security decision
Security Core    → enforces the resulting state
```

The Security Counsel is therefore not a fallback decision-maker.

It is an additional source of security intelligence.

---

## 9. Security Counsel Does Not Resolve Authorization

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

How the Core incorporates the advice is deliberately left undefined by this architectural specification.

---

## 10. Information Minimization

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

## 11. No Authority Transitivity

Authority does not flow from the Security Counsel back through the communication channel.

The existence of a secure channel does not itself establish authorization.

The Security Counsel provides additional security intelligence.

The Core determines the significance of that intelligence within its own defined security architecture.

---

## 12. Escalation Without Boundary Expansion

Security Counsel provides a mechanism for the Security Core to obtain additional security expertise when its local decision context is insufficient.

This permits a system to distinguish between:

```text
insufficient local security understanding
        ↓
additional security analysis
        ↓
Security Counsel advice
        ↓
Core-defined processing
```

rather than treating the absence of sufficient local understanding as permission.

The mechanism therefore extends the **decision-support capability** of the Security Core without extending the **authorization boundary**.

---

## 13. Architectural Principle

The Security Counsel introduces a controlled higher-level advisory path:

> **When the local security system cannot sufficiently determine the security meaning or consequences of an Objective or execution plan, the Security Core may obtain additional security expertise through a secure, information-minimized channel.**

The Security Counsel may contribute to:

* understanding the security implications of an Objective,
* analyzing a complex execution plan,
* identifying missing conditions,
* recommending further clarification,
* or recommending interruption or additional security review.

**How the Security Core acts upon that advice is intentionally not defined by this specification.**

The decisive architectural properties remain:

> **The Security Counsel advises. The Core decides.**

and:

> **Additional security intelligence may improve the quality of the authorization process without becoming an alternative authorization mechanism.**
