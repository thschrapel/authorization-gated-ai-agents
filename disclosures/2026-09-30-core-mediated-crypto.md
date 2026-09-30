# Addendum — Core-Mediated Cryptographic Operations

## 1. Cryptography Is a Core-Controlled Capability

Components must not independently perform cryptographic encryption or decryption for information that participates in the Security Architecture.

A component may **request** a cryptographic operation, but the cryptographic operation itself is performed by the Security Core or by a Core-controlled cryptographic subsystem.

The architectural rule is:

> **Components may request cryptographic operations; they may not establish uncontrolled cryptographic channels.**

This applies in particular to the AI-enabled OS, where unrestricted component-level cryptography could otherwise create an information path that bypasses Core inspection and authorization.

## 2. No Component-Level End-to-End Encryption

Components must not establish end-to-end encrypted communication channels with:

* other AI components,
* external services,
* Effects,
* local processes,
* or other communication endpoints,

outside the Core-controlled cryptographic interface.

In particular, a component must not be able to:

```text
Component A
    │
    │ encrypt locally
    ▼
opaque ciphertext
    │
    │ direct channel
    ▼
Component B
```

because the resulting ciphertext could prevent the Security Core from evaluating the information flow.

Instead:

```text
Component A
    │
    │ cryptographic request
    ▼
Security Core
    │
    │ Core-controlled operation
    ▼
Cryptographic Subsystem
    │
    ▼
Security Core
    │
    │ authorized result
    ▼
Component / external interface
```

## 3. Cryptographic Requests Are Not Authorization

A request such as:

```text
ENCRYPT(data, key, context)
```

does not itself authorize encryption.

It is an ordinary Core-mediated capability request.

The Security Core determines:

* whether the component may request the operation,
* which cryptographic operation is permitted,
* which key or key class may be used,
* which destination or purpose is authorized,
* whether the resulting ciphertext may leave the Core boundary,
* and whether the corresponding Effect is permitted.

Likewise, a component requesting:

```text
DECRYPT(ciphertext, key, context)
```

does not acquire unrestricted access to the plaintext or the underlying key.

## 4. Keys Are Core-Controlled

Cryptographic keys must not be freely exposed to AI components.

Where possible, components should operate through key references or cryptographic capabilities rather than receiving raw key material.

Conceptually:

```text
Component
    │
    │ SIGN / ENCRYPT / DECRYPT request
    ▼
Security Core
    │
    │ key reference
    ▼
Cryptographic Subsystem
```

The key itself remains within the Core-controlled security boundary.

## 5. Inspection Before Cryptographic Transformation

Where information is subject to Security Core evaluation, the relevant information flow must be evaluated **before it becomes opaque to the Core**.

Consequently, a component must not use encryption as a mechanism to hide information from the Core.

The general ordering is:

```text
component request
      ↓
Core evaluation
      ↓
authorized cryptographic operation
      ↓
Core-controlled result
      ↓
authorized next transition
```

Encryption therefore becomes a **controlled Effect**, not an escape mechanism from the authorization architecture.

## 6. Relationship to Rule 0

Cryptographic operations are subject to the same universal transition model as every other capability.

For example:

```text
Component
   │
   │ OUTPUT
   ▼
Security Core
   │
   │ EFFECT
   ▼
Cryptographic Subsystem
```

The Core therefore controls both:

1. the information supplied to the cryptographic operation, and
2. the resulting encrypted or decrypted information leaving the operation.

## 7. Architectural Invariant

The invariant is:

> **No component may create an information channel that becomes opaque to the Security Core merely by applying cryptography.**

Cryptography remains available as a capability of the system, but its use is mediated by the same authorization architecture as every other protected operation.

This distinction is particularly important for an AI-enabled OS: **cryptography is not prohibited; uncontrolled cryptographic authority is.**
