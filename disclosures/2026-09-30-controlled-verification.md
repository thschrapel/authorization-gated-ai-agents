# Addendum — Controlled Verification and Local Security Policy Extensions

## 1. Controlled Observability

The Security Core is not required to make its internal decision process, security rules, or complete reaction behavior publicly observable.

Trust in a Security Core may instead be established through controlled verification by parties with a legitimate and defined interest.

Such parties may include:

* accredited expert groups,
* certified evaluation laboratories,
* authorized auditors,
* certification authorities,
* or other explicitly trusted verification bodies.

The relevant system behavior may therefore be evaluated under controlled conditions without requiring unrestricted public access to the Security Core or its internal security mechanisms.

## 2. Controlled Verification Conditions

A Security Core may demonstrate its security properties under defined and potentially closed test conditions.

Such conditions may specify:

* which Core behavior is being tested,
* which inputs and contexts are permitted,
* which security properties are being evaluated,
* which Effects are available,
* and which evaluation procedures are authoritative.

The verification environment itself may therefore be controlled and trusted.

The architectural principle is:

> **Verifiability does not require unrestricted observability.**

A system may establish evidence of compliance through controlled, reproducible, and appropriately trusted evaluation procedures.

## 3. Local Security Policy Extensions

The architecture should permit additional local security restrictions to be installed without modifying the fundamental Security Core architecture.

A local policy may, for example, introduce additional `DENY` conditions that are stricter than the globally defined baseline.

Such an extension may be used to adapt a system to:

* a particular deployment environment,
* organizational requirements,
* local safety requirements,
* user-defined restrictions,
* or newly identified security concerns.

## 4. Core Authorization of Policy Extensions

A local policy extension does not become active merely because it is present on the system.

The Security Core must evaluate and authorize the installation or activation of the extension before it can influence authorization decisions.

Conceptually:

```text id="f3q8zn"
Policy Extension
       │
       │ installation request
       ▼
Security Core
       │
       ├── validate source
       ├── validate policy
       ├── validate scope
       └── authorize activation
                │
                ▼
          Active Policy
```

The extension therefore becomes part of the authorization architecture only after Core approval.

## 5. Trusted Policy Sources

A local policy extension must originate from a source that is authorized to modify the applicable security policy.

Possible sources include:

* an authorized system administrator,
* an explicitly authorized user,
* a manufacturer-controlled policy channel,
* a certified deployment authority,
* or another Core-defined trusted source.

The source itself must be authenticated according to the applicable security architecture.

An arbitrary component must not be able to install or activate a policy extension merely by requesting it.

## 6. Local DENY Conditions

Local policy extensions should primarily be able to **restrict** the set of permitted operations.

A local extension may therefore add conditions such as:

```text id="x9v2kc"
baseline policy
      │
      ▼
additional local DENY condition
      │
      ▼
effective authorization policy
```

The extension must not silently bypass or weaken mandatory Core security requirements.

The resulting policy is therefore conceptually:

```text id="5j4r7p"
Effective Policy
    =
    Core Baseline
    +
    authorized local restrictions
```

where local restrictions can reduce the set of permitted transitions but cannot create authorization that the Core baseline does not permit.

## 7. Architectural Invariant

The resulting principle is:

> **Security policy may be locally extended, but never locally bypassed.**

Likewise:

> **Security Core behavior may be independently verified without requiring unrestricted public disclosure of the Core's internal decision mechanisms.**

This permits systems to combine:

* controlled certification,
* expert evaluation,
* privacy-preserving verification,
* deployment-specific security policies,
* and a fixed authoritative Core

without making the security boundary dependent on public disclosure of its internal implementation.
