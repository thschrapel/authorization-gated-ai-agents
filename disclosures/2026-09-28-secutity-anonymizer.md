# Security Anonymizer — Local Security Information Removal

## 1. Purpose

The **Security Anonymizer** is a dedicated security component that removes security-critical information from information leaving the protected Security Core.

Its purpose is to ensure that secrets and other security-sensitive information are not exposed to external AI components, Counsel components, Anonymizing Counsel services, translators, or other components outside the protected security boundary.

Examples include:

* passwords,
* authentication tokens,
* private keys,
* credentials,
* session secrets,
* cryptographic material,
* recovery codes,
* security configuration secrets,
* and other Core-defined security-critical information.

## 2. Architectural Position

The Security Anonymizer is part of the protected Security Core environment.

It is therefore fundamentally different from the general-purpose Anonymizer.

```text
                 Protected Security Core
                         │
                         ▼
                Security Anonymizer
                         │
                  security-clean
                     information
                         │
                         ▼
              Anonymizer / Counsel /
                External AI Service
```

The Security Anonymizer operates **before information is permitted to leave the security boundary**.

## 3. Local Processing

Security-critical information must be removed locally.

The original security-sensitive information must not be transmitted to an external Security Counsel, Anonymizing Counsel, AI model, Translator, Planner, or other component merely for the purpose of determining how it should be removed.

The architectural principle is:

> **Security-critical information is removed before external processing.**

This distinguishes the Security Anonymizer from advisory anonymization.

## 4. Security Core Authority

The Security Core defines which classes of information are security-critical and therefore subject to mandatory local removal.

The Security Anonymizer does not negotiate this requirement with external components.

For example, if a document contains:

```text id="5t7n4c"
username: alice
password: <secret>
```

the external processing path may receive:

```text id="d7y2jp"
username: alice
password: [REMOVED]
```

The original password remains within the protected security boundary.

## 5. Non-Reversible Removal

Where information is classified as security-critical, the Security Anonymizer should remove it rather than merely transform it into a reversible or externally recoverable representation.

In particular, security-critical values must not be:

* forwarded to an external Counsel,
* encoded for later reconstruction,
* replaced by a recoverable token,
* transmitted for external anonymization,
* or exposed through contextual metadata.

The intended property is:

> **The external processing path cannot reconstruct the original security-critical value from the Security Anonymizer output.**
# Security Anonymizer — Local Security Information Removal

# Security Anonymizer — Local Security Information Removal

## 6. Fail-Closed Enforcement Outcomes

The Security Anonymizer operates **fail-closed**.

The architecture distinguishes between the **reason why an operation cannot proceed** and the **authoritative enforcement outcome**.

The Security Core uses `DENY` as the common enforcement outcome whenever the requested external effect must not proceed.

The reason for the `DENY` remains separately classified.

### 6.1 Transformation Result

The Security Anonymizer reports the result of the required transformation:

```text id="1s2z3v"
TRANSFORMATION_RESULT

    CLEAN
        The required transformation was completed successfully.

    TRANSFORMATION_FAILURE
        The required transformation could not be completed
        reliably.

    TRANSFORMATION_UNCERTAIN
        The component cannot establish with sufficient certainty
        that the required transformation was completed.

    PROCESSING_ERROR
        The transformation process encountered an internal error
        and cannot provide a reliable result.
```

Only `CLEAN` permits the Security Core to evaluate the resulting information for disclosure.

### 6.2 Authorization Result

If the transformation result is `CLEAN`, the Security Core may evaluate the resulting information.

```text id="7v0j2s"
AUTHORIZATION_RESULT

    ALLOW
        The transformed information may proceed
        under the applicable authorization rules.

    AUTHORIZATION_DENIED
        The transformed information must not proceed
        under the applicable authorization rules.
```

`AUTHORIZATION_DENIED` is the explicit authorization result indicating that the requested disclosure is prohibited.

### 6.3 Unified Enforcement Outcome

The Security Core converts all non-permitted states into the same authoritative enforcement outcome:

```text id="x8f4kq"
ENFORCEMENT_OUTCOME

    ALLOW
        The requested external effect may proceed.

    DENY
        The requested external effect must not proceed.
```

The reason for `DENY` is retained separately.

For example:

```text id="4tq9ns"
DENY
  ├── reason: TRANSFORMATION_FAILURE
  ├── reason: TRANSFORMATION_UNCERTAIN
  ├── reason: PROCESSING_ERROR
  └── reason: AUTHORIZATION_DENIED
```

This provides a single enforcement vocabulary without losing diagnostic or audit information.

### 6.4 Enforcement Flow

```text id="8u3m1p"
                 Security Anonymizer
                         │
                         ▼
                Transformation Result
                         │
             ┌───────────┴───────────┐
             │                       │
           CLEAN              any failure state
             │                       │
             ▼                       ▼
      Authorization Evaluation       DENY
             │
        ┌────┴──────────────────┐
        │                       │
      ALLOW          AUTHORIZATION_DENIED
        │                       │
        ▼                       ▼
      ALLOW                    DENY
```

Thus:

> **`AUTHORIZATION_DENIED` is the authorization result. `DENY` is the resulting enforcement outcome.**

A transformation failure is not an `AUTHORIZATION_DENIED` result.

It is a separate condition that causes the Security Core to produce the enforcement outcome `DENY`.

### 6.5 No Fail-Open Transition

No transformation failure, uncertainty, or processing error may be converted into `ALLOW`.

In particular:

```text id="0m6j8r"
TRANSFORMATION_FAILURE    → DENY
TRANSFORMATION_UNCERTAIN  → DENY
PROCESSING_ERROR          → DENY
AUTHORIZATION_DENIED      → DENY
```

Only:

```text id="q5r2cx"
TRANSFORMATION_RESULT = CLEAN
        +
AUTHORIZATION_RESULT = ALLOW
        ↓
ENFORCEMENT_OUTCOME = ALLOW
```

may result in an external effect.

### 6.6 `DENY` and `HALT`

`DENY` and `HALT` remain distinct.

`DENY` means that the requested effect does not proceed.

`HALT` is a stronger Core-level control transition used where the security architecture requires execution to stop rather than merely reject the current request.

An `AUTHORIZATION_DENIED` result may therefore produce `DENY` without requiring `HALT`.

A transformation failure, security anomaly, or other Core-defined condition may produce `DENY` and additionally trigger `HALT`.

The enforcement semantics remain:

```text id="r8k4zp"
DENY
    requested effect does not occur

HALT
    Core-defined execution state is stopped
```

`HALT` describes an additional enforcement transition; it does not replace the `AUTHORIZATION_DENIED` result when `AUTHORIZATION_DENIED` is the applicable reason.

### 6.7 Audit Semantics

The Audit Log should record both:

1. the authoritative enforcement outcome, and
2. the reason for that outcome.

For example:

```text id="2n7qws"
ENFORCEMENT_OUTCOME: DENY
REASON: TRANSFORMATION_UNCERTAIN
```

or:

```text id="6x1vkc"
ENFORCEMENT_OUTCOME: DENY
REASON: AUTHORIZATION_DENIED
```

This keeps the enforcement protocol simple while preserving the information required for diagnosis, evaluation, and later audit.

### 6.8 Architectural Invariant

The resulting invariant is:

> **Every requested external effect has an explicit Core-defined enforcement outcome.**

There is no implicit state in which an operation may proceed because the system failed to determine whether it was safe.

The fundamental rule is therefore:

```text id="w3h7qm"
                 ┌── ALLOW ──► effect may proceed
Security Core ───┤
                 └── DENY ───► effect does not proceed
```

The reason for `DENY` may differ, but the enforcement semantics do not.

`AUTHORIZATION_DENIED` is the sole term used for the explicit authorization state in which the requested effect is prohibited.

Transformation failures, transformation uncertainty, processing errors, and `AUTHORIZATION_DENIED` are therefore distinct causes that may all result in the same fail-closed enforcement outcome: `DENY`.

## 7. Processing Order

Where both privacy anonymization and security anonymization are required, security-critical information must be removed before information is made available to external processing components.

A conceptual processing path is:

```text id="0l6jhc"
Protected Information
        │
        ▼
Security Anonymizer
        │
        │ security-clean information
        ▼
Anonymizer
        │
        │ privacy-transformed information
        ▼
External Processing
```

The exact implementation may combine the operations, but the architectural property remains that security-critical information is protected locally.

## 8. Interaction with Security Counsel

The Security Counsel must not receive security-critical information merely because it is asked to provide security advice.

If the Security Core requires external advice, the request must first pass through the Security Anonymizer.

```text id="x8k3p1"
Security Core
      │
      │ security question
      ▼
Security Anonymizer
      │
      │ sanitized request
      ▼
Security Counsel
      │
      │ advice
      ▼
Security Core
```

This permits external advisory capability without requiring disclosure of the secrets that the local Security Core is responsible for protecting.

## 9. Interaction with Anonymizing Counsel

The same restriction applies to the Anonymizing Counsel.

An Anonymizing Counsel may advise on privacy transformation without receiving the underlying password, credential, private key, or other security-critical value.

The local Security Anonymizer therefore precedes any external privacy analysis.

## 10. No Security Bypass Through Context

Security-critical information must not become exposed indirectly through surrounding context.

The Security Anonymizer should therefore consider not only explicit secret fields but also security-relevant representations that could reveal the protected value.

The exact detection mechanisms are implementation-specific.

The Security Core defines the applicable security classes and enforcement requirements.

## 11. Unknown AI Components

The Security Anonymizer is particularly important when information is sent to unknown or externally hosted AI components.

The architecture does not require the external model to be trusted with credentials merely because it is capable of processing the surrounding task.

For example:

```text id="l7q3z8"
Local system:
    "Connect to server X using the configured credentials."

External AI:
    receives task context

External AI does NOT receive:
    password
    private key
    authentication token
```

The AI may reason about the task while the security-sensitive execution details remain under Core control.

## 12. Relationship to Authorization

The Security Anonymizer does not authorize disclosure.

It establishes a prerequisite security transformation.

A typical flow is:

```text id="4c9m1s"
Information
    │
    ▼
Security Core
    │
    ├── Is disclosure permitted?
    │
    ▼
Security Anonymizer
    │
    ├── remove security-critical information
    │
    ▼
Anonymizer
    │
    ├── apply privacy transformation
    │
    ▼
Security Core
    │
    ├── authorize actual external result
    │
    ▼
External System
```

Thus, successful security anonymization does not automatically authorize transmission.

## 13. Architectural Principle

The Security Anonymizer establishes a hard separation between information required for a task and information required to control protected resources.

The external AI may receive the former without receiving the latter.

The central principle is:

> **An AI may be given enough information to reason about a task without being given the secrets required to directly control the protected system.**

## 14. Extension to AI-Enabled Operating Systems

In an AI-Enabled Operating System, the Security Anonymizer may be implemented as a standard local security service.

It can protect information crossing boundaries to:

* external AI services,
* Security Counsel services,
* Anonymizing Counsel services,
* remote diagnostics,
* cloud services,
* logging systems,
* collaboration systems,
* and other external processing environments.

The resulting architecture provides three distinct information-processing layers:

```text id="b7x2k4"
Security Anonymizer
        │
        │ removes security-critical information
        ▼
Anonymizer
        │
        │ reduces privacy exposure
        ▼
External Processing
```

The Security Core remains the authoritative owner of the security boundary and the enforcement decision.

## 15. Summary

The Security Anonymizer is therefore not merely a more aggressive form of anonymization.

It is a **local security-boundary component** whose defining property is:

> **Security-critical information does not leave the Security Core merely because another component needs contextual information to perform its task.**

This allows powerful external AI and Counsel components to operate on useful contextual information while keeping credentials and other security-critical material within the protected security domain.
