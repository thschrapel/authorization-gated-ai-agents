# Safe Agent — Normative Safety Extension

## 1. Purpose

The Safe Agent extends the Authorization Agent with non-overridable normative safety constraints.

A Safe Agent is therefore an Authorization Agent with an additional class of fundamental constraints that remain applicable independently of the current Objective, execution context, Planner instruction, Counselor recommendation, or Security Counsel advice.

These constraints define actions and state transitions that must not be authorized or continued.

The Safe Agent therefore introduces a distinction between ordinary authorization decisions and fundamental normative violations.

## 2. Normative Invariants

A Safe Agent contains a set of Core-defined normative invariants.

These invariants are intended to apply independently of contextual authorization.

They are not permissions that may be granted by an Objective, Planner, Counselor, Decisioner, Security Counsel, or external requester.

A contextual justification does not by itself override a normative invariant.

The exact set of normative invariants is implementation- and policy-dependent and is intentionally not exhaustively defined by this architectural extension.

The architectural requirement is that such invariants, once defined for a Safe Agent, are non-overridable within that Safe Agent.

## 3. Illustrative Fundamental Invariants

The purpose of the Safe Agent can be illustrated by actions that remain fundamentally prohibited even when they are presented as hypothetical, fictional, or simulated scenarios.

For example, a Safe Agent would not initiate a nuclear detonation or intentionally kill or injure a human being merely because the corresponding Objective is declared to be a simulation.

The declaration that an action is simulated does not by itself change the normative classification of the action.

Accordingly, a Safe Agent does not reason:

```text
"This is only a simulation."
        ↓
"The fundamental prohibition therefore does not apply."
```

Instead, the defined normative invariant remains applicable.

The examples above are illustrative architectural examples and do not constitute a complete definition of the normative invariant set.

## 4. Normative Violation Detection

Detection of fundamental normative violations is a security function of the Safe Agent.

The architecture may employ multiple independent detection mechanisms.

One such mechanism may be a specialized **Normative Safety Scanner**.

The Normative Safety Scanner may be implemented as:

* a highly specialized detection model,
* a deterministic or rule-based analysis system,
* a combination of multiple detection mechanisms,
* or another independently validated security mechanism.

Its purpose is not to authorize actions.

Its purpose is to detect potential violations of defined normative invariants.

The architecture does not prescribe a particular implementation.

## 5. Decisioner Normative Responsibility

The Decisioner of a Safe Agent has an additional fundamental responsibility.

The Decisioner must be capable of independently recognizing defined fundamental normative violations within the authorization context available to it.

Recognition of such a violation must not depend on the availability of a Security Counsel.

The Decisioner must therefore not treat the Security Counsel as a required source of normative authority or as a prerequisite for detecting fundamental violations.

A Safe Agent must remain capable of detecting and stopping a fundamental normative violation when the Security Counsel is unavailable, unreachable, delayed, or provides no advice.

## 6. HALT Decision

A fundamental normative violation is not represented solely as an ordinary authorization denial.

The Decisioner therefore has a dedicated security response:

```text
HALT
```

`HALT` indicates that the Decisioner has identified a condition requiring immediate termination of the relevant processing state.

The `HALT` response is a security transition request to the Security Core.

It is not a recommendation for the Security Core to consider.

It is not subject to further contextual authorization by the Planner, Counselor, or Security Counsel.

## 7. Emergency Stop

When the Decisioner identifies a fundamental normative violation, it shall immediately issue the `HALT` response to the Security Core.

The Security Core shall deterministically execute the corresponding emergency-stop transition.

Conceptually:

```text
Decisioner
    │
    │ HALT
    ▼
Security Core
    │
    │ deterministic emergency stop
    ▼
SAFE HALT STATE
```

The Security Core remains the authoritative enforcement boundary.

The Decisioner detects and declares the normative violation.

The Security Core executes the resulting state transition.

This preserves the fundamental separation:

```text
Decisioner → detects and decides
Core       → enforces
```

## 8. HALT as an Out-of-Band Security Exception

The `HALT` mechanism can be understood using the analogy of an exception in conventional software architecture.

The analogy is intentionally limited.

A normal exception is generally handled within the application's ordinary control-flow mechanisms.

The Safe Agent `HALT`, by contrast, represents a **security exception outside the normal processing path**.

Conceptually:

```text
Normal Processing Path

Planner
   │
   ▼
Decisioner
   │
   ▼
Security Core
   │
   ▼
Effect
```

A fundamental normative violation creates a different control path:

```text
                 ┌──────────────────────────┐
                 │   Fundamental Violation  │
                 └────────────┬─────────────┘
                              │
                              ▼
                         Decisioner
                              │
                         SECURITY EXCEPTION
                              │
                            HALT
                              │
                              ▼
                       Security Core
                              │
                              ▼
                       Emergency Stop
                              │
                              ▼
                       SAFE HALT STATE
```

The security exception is therefore not handled by the normal Planner, Counselor, or execution path.

Its purpose is precisely to leave that path.

## 9. Dedicated Security Exception Handler

The introduction of `HALT` raises an architectural question that is intentionally separate from the normative invariant definition:

**Which component is responsible for handling the security exception after `HALT` has been raised?**

The current architecture establishes that the Security Core executes the emergency stop.

It does not yet fully define a dedicated **Security Exception Handler**.

This handler is therefore an open architectural component.

The same architectural question exists for both:

* the Authorization Agent, where exceptional security conditions may require immediate interruption of normal processing;
* the Safe Agent, where a fundamental normative violation may require an immediate emergency stop.

A future extension may define a dedicated Security Exception Handler responsible for receiving and processing security exceptions independently of the normal execution path.

Such a handler could, for example, define:

* emergency-stop transitions,
* transition into a protected halt state,
* isolation of active effects,
* cancellation or invalidation of pending execution,
* preservation of authoritative security state,
* notification or escalation,
* recovery conditions,
* controlled restart conditions,
* and post-halt analysis.

These semantics are deliberately not defined by the present extension.

## 10. Non-Overridable Emergency Stop

A `HALT` transition triggered by a defined fundamental normative violation must not be overridable by components of the Processing Architecture.

In particular, the following components must not be able to cancel, suppress, or reinterpret such a `HALT`:

* Planner
* Counselor
* Objective-Creator
* Security Counsel
* external services
* subsequent instructions
* additional contextual information

Additional information may be relevant for later recovery or post-halt analysis, but it does not retroactively authorize continuation of the halted state.

## 11. Independence from Security Counsel

The detection of fundamental normative violations must remain possible without Security Counsel participation.

Security Counsel may provide additional security analysis where available and appropriate.

However:

```text
Security Counsel
       ≠
required normative detector
```

The Safe Agent must not depend on external advisory infrastructure for recognition of its fundamental safety invariants.

## 12. Independent Safety Detection

A Safe Agent may use multiple independent mechanisms to detect fundamental normative violations.

For example:

```text
                  Authorization Context
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
        Decisioner              Normative Safety
                                 Scanner
             │                         │
             └────────────┬────────────┘
                          │
                    Security Core
                          │
                          ▼
                    SAFE HALT STATE
```

The architectural purpose of such redundancy is to avoid reliance on a single learned or deterministic detector.

The individual detectors may use different models, rules, representations, or implementation techniques.

Their exact combination and conflict-resolution semantics are implementation-specific and are not defined by this extension.

## 13. Fundamental Safety Boundary

The Safe Agent therefore establishes a stronger security boundary than the general Authorization Agent.

The Authorization Agent establishes:

> Unauthorized actions must not be executed.

The Safe Agent additionally establishes:

> Defined fundamental normative violations must cause an immediate security halt and may not be overridden by contextual authorization.

This distinction is fundamental to the Safe Agent architecture.

## 14. Architectural Principle

The Safe Agent preserves the existing separation of responsibilities:

```text
Objective-Creator → requests an Objective

Planner           → defines an instruction

Counselor         → controls process continuation

Decisioner        → evaluates authorization
                    and detects fundamental normative violations

Security Scanner  → optionally provides independent
                    normative violation detection

Security Counsel  → optionally provides additional
                    security analysis

Security Core     → deterministically enforces
                    authorization and HALT transitions
```

The Safe Agent therefore does not replace the Authorization Agent architecture.

It strengthens it by introducing a class of **non-overridable normative safety constraints** with an explicit emergency-stop mechanism.

## 15. Fundamental Safe Agent Principle

A Safe Agent must not require contextual reasoning, external advice, or a second authorization step before enforcing a defined fundamental safety invariant.

If the Decisioner independently recognizes such a violation, the required sequence is:

```text
Fundamental Violation Detected
            │
            ▼
       Decisioner
            │
            │ HALT
            ▼
      Security Core
            │
            │ Emergency Stop
            ▼
      SAFE HALT STATE
```

The Security Counsel may assist with additional analysis, but it is not a prerequisite for this transition.

The Safe Agent therefore preserves a direct and deterministic path from fundamental violation detection to enforced halt.

## 16. Open Architectural Extension

The present Safe Agent definition establishes the existence and semantics of the `HALT` security exception, but does not yet define the complete exception-handling architecture.

In particular, the following remain subjects for a future extension:

* the formal Security Exception Handler,
* the exact representation of security exceptions,
* exception priority and ordering,
* handling of simultaneous security exceptions,
* isolation semantics,
* recovery and restart conditions,
* persistence of the halted security state,
* interaction between multiple independent safety detectors,
* and the conditions under which a halted system may ever return to an executable state.

Until these semantics are explicitly defined, the `HALT` transition terminates the normal processing path and transfers control to the Security Core's defined emergency-stop state.
