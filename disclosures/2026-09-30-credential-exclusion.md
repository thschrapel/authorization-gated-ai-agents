# Addendum — Credential Exclusion from Logs

## 1. Credential-Free Logging

No log within the Security Architecture may store credentials.

This includes, in particular:

* passwords,
* authentication tokens,
* API keys,
* private cryptographic keys,
* session credentials,
* access tokens,
* recovery secrets,
* authentication cookies,
* and equivalent secret material.

The prohibition applies independently of the purpose or destination of the log.

> **No credential may become persistent log data.**

## 2. Logging Is Not a Secret Store

Logs must not be used as a fallback mechanism for storing authentication or authorization material.

A component may request the use of a credential through an authorized interface, but the credential itself must not be copied into:

* Context Logs,
* Audit Logs,
* diagnostic logs,
* execution logs,
* error logs,
* trace logs,
* or component-maintained logs.

Where a credential must be referenced in an event, the log may contain only a non-secret identifier or other Core-approved reference.

For example:

```text id="k3f7mx"
CREDENTIAL_USED
credential_ref = credential-42
```

is permissible in principle, whereas:

```text id="v8q1pz"
PASSWORD = "..."
API_KEY = "..."
TOKEN = "..."
```

is not.

## 3. Core-Level Enforcement

The Security Core is responsible for preventing credentials from entering protected logs.

Credential handling therefore follows the same general principle as the Security Anonymizer:

```text id="x6n2bc"
Sensitive information
        │
        ▼
Security Core
        │
        ├── permitted operational use
        │
        └── logging path → credential excluded
```

A logging operation must fail closed if the Core cannot establish that the data is free of credentials.

## 4. Architectural Invariant

The invariant is:

> **No credential is ever persisted as log content.**

This applies regardless of whether the credential originated from a component, an external system, an Effect, a user interaction, or an operating-system facility.

Logs may record that a credential-dependent operation occurred, but they must never record the credential itself.

This preserves the separation between:

**operational evidence**

and

**authentication authority**.

The log can document the fact that an authorized operation occurred without becoming a secondary repository of the secrets that enabled it.

