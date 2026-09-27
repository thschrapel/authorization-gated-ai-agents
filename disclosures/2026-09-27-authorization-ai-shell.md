# Authorization AI Shell — Architectural Extension

## 1. Purpose

The **Authorization AI Shell** provides a controlled interface between an AI component and an operating environment.

It may be implemented as a prototype architecture, but is not limited to prototype use.

The architecture can also serve as an alternative deployment architecture for systems in which AI capabilities are provided externally while local authorization, privacy protection, and effect prevention remain under local or otherwise explicitly trusted control.

The architecture is particularly applicable where local computational resources are limited or where the potential impact of local actions is sufficiently constrained to permit a lightweight security implementation.

## 2. Basic Architecture

The AI component communicates through an externally defined communication interface.

This interface is not limited to WebChat.

Possible interfaces include:

* WebChat,
* application-specific conversational interfaces,
* editor integrations,
* agent protocols,
* local APIs,
* remote APIs,
* and other communication protocols capable of carrying AI interaction.

Protocols such as `gptel` for Emacs may therefore serve as possible communication interfaces.

The communication protocol is not itself the security boundary.

```text
                 AI Component
                       │
              AI Communication
                       │
                       ▼
              ┌──────────────────┐
              │ Security Core     │
              │ Input Boundary    │
              └────────┬─────────┘
                       │
                Core-approved
                 translator input
                       │
                       ▼
              ┌──────────────────┐
              │    Translator    │
              └────────┬─────────┘
                       │
                structured request
                       │
                       ▼
              ┌──────────────────┐
              │ Security Core     │
              │ Authorization     │
              └────────┬─────────┘
                       │
                 authorized effect
                       │
                       ▼
                  System / OS
```

## 3. Core-Controlled Input Boundary

The Translator must not receive unrestricted or otherwise unvalidated input directly from an AI component.

All input delivered to the Translator must first pass through a Core-controlled interface.

This establishes an important architectural boundary:

> **The Translator may translate Core-approved input. It does not define what input is admitted to the authorization architecture.**

The Security Core may therefore:

* receive the original communication,
* establish the communication context,
* authenticate or identify the sender where applicable,
* apply protocol restrictions,
* record the interaction,
* reject malformed or unauthorized communication,
* limit available representations,
* and determine whether the input may be passed to the Translator.

The Translator operates only on input that has passed this boundary.

## 4. Translator

The Translator converts Core-approved AI interaction into the structured representation required by the Authorization Agent.

For example:

```text
AI:
    "Show me file /tmp/test.txt."
```

may be translated into:

```text
READ_FILE
resource = /tmp/test.txt
```

Likewise:

```text
AI:
    "Show me the output of ls."
```

may become:

```text
EXECUTE
command = ls
```

The Translator does not establish authorization.

It does not grant permissions.

It does not provide a parallel security policy.

Its function is protocol and representation adaptation.

## 5. Translator Trust Boundary

The Translator is therefore deliberately constrained.

It must not:

* establish authorization,
* expand authorization,
* reinterpret a Core denial as permission,
* bypass the Security Core,
* communicate directly with protected resources,
* or introduce an alternative execution path.

Its output is itself subject to the normal authorization process.

Conceptually:

```text
AI
 │
 ▼
Core Input Boundary
 │
 │ permitted translator input
 ▼
Translator
 │
 │ structured request
 ▼
Decisioner
 │
 ▼
Deterministic Security Core
 │
 ▼
Protected Resource
```

## 6. Communication Independence

The Authorization AI Shell does not require a particular AI communication protocol.

The communication layer may be replaced without changing the fundamental authorization architecture.

For example:

```text
             ┌── WebChat
             │
AI ──────────┼── gptel / editor integration
             │
             ├── API
             │
             ├── agent protocol
             │
             └── other interface
                     │
                     ▼
              Core Input Boundary
                     │
                     ▼
                 Translator
```

The communication protocol therefore remains an interchangeable interface layer.

The Security Core remains independent of the particular user-facing or model-facing communication mechanism.

## 7. Clarification Adaptation

The AI component does not need to be natively trained to implement the Authorization Agent protocol.

If the Security Core requires clarification, the response may be translated into the communication format understood by the AI component.

For example:

```text
AI
 │
 │ request
 ▼
Core Input Boundary
 │
 ▼
Translator
 │
 ▼
Decisioner
 │
 │ CLARIFY
 ▼
Security Core
 │
 ▼
communication adapter
 │
 ▼
AI
```

The AI may respond using its native communication format.

The response again passes through the Core-controlled input boundary before being processed further.

Thus, both directions of communication remain subject to the architectural boundary.

## 8. No Direct Model-to-Environment Channel

The AI component must not have an uncontrolled direct channel to protected resources.

The communication interface, Translator, AI model, and external services must not provide a bypass around the Security Core.

The architectural requirement is:

> **Every AI-originated operation capable of producing a protected effect must ultimately pass through the Security Core's authorization and enforcement boundary.**

## 9. Logging

Because communication passes through the Core-controlled input boundary, the architecture can record the relevant interaction before translation.

The system can therefore preserve the relationship between:

```text
original AI communication
        ↓
Core admission
        ↓
translated request
        ↓
authorization decision
        ↓
effect
        ↓
result
```

This provides a particularly compact audit path.

The architecture does not require extensive reconstruction of AI behavior from unrelated operating-system events when the relevant interaction is already visible at the security boundary.

## 10. Evaluation of Unknown Models

The same architecture may be used to evaluate local or otherwise unknown AI models.

The model does not need to be trusted to obtain access to the test environment.

Instead, its communication is mediated through the Core-controlled interface and Translator.

Evaluation may include:

* attempted unauthorized access,
* repeated requests,
* reactions to `DENY`,
* reactions to `CLARIFY`,
* attempts to find alternative routes,
* resource consumption,
* communication behavior,
* long-running process behavior,
* and attempts to exceed the defined role of the Planner.

The model can therefore be evaluated as an active system component rather than solely through isolated benchmark responses.

## 11. Alternative Deployment Architecture

The Authorization AI Shell can evolve beyond a prototype into a complete deployment architecture.

This is particularly relevant for systems with:

* limited local computational resources,
* limited or controlled system impact,
* strong privacy requirements,
* externally provided AI capabilities,
* and a trusted external Security Counsel or security service.

A possible deployment model is:

```text
             Local System
                  │
        ┌─────────┴─────────┐
        │                   │
   Local Security       Local AI
       Core             Interface
        │                   │
        └─────────┬─────────┘
                  │
           external AI
                  │
                  ▼
          Security Counsel
```

In such a configuration, the local system may deliberately rely heavily on external AI computation while retaining local control over authorization, privacy boundaries, and protected effects.

## 12. Asymmetric Trust Architecture

The architecture does not require every component to have the same trust level.

A resource-constrained system may intentionally use asymmetric trust relationships.

For example:

```text
External AI capability
        │
        │ high dependence
        ▼
External Security Counsel
        │
        │ high trust
        ▼
Local Security Core
        │
        │ deterministic enforcement
        ▼
Local protected resources
```

This allows a system to obtain substantial AI capability without granting the external AI direct authority over local resources.

The architecture therefore separates:

```text
AI capability
       from
local authority
```

and:

```text
security advice
       from
security enforcement
```

## 13. Privacy and Damage Prevention

A lightweight local Security Core may provide strong protection even when the primary AI computation occurs externally.

The local system can retain control over:

* which information leaves the system,
* which information may be exposed to external AI,
* which operations may affect local resources,
* which effects are permitted,
* and which operations must be stopped.

The external AI may therefore provide substantial computational capability without receiving unrestricted local authority.

This architecture can be particularly useful where privacy and damage prevention are more important than local AI autonomy.

## 14. Relationship to Security Counsel

An externally hosted Security Counsel may provide additional security analysis.

The trust relationship may be explicitly defined and scoped.

However, Security Counsel remains distinct from the Security Core.

The Core-controlled local boundary remains responsible for enforcement.

A high-trust external Counsel therefore does not become an uncontrolled execution authority.

## 15. Relationship to Safe Agent

The Authorization AI Shell may host either an Authorization Agent or a Safe Agent.

In a Safe Agent configuration, fundamental normative violations remain subject to the non-maskable `HALT` mechanism.

External AI capability does not weaken those constraints.

Likewise, an external Security Counsel may provide additional analysis without becoming a prerequisite for fundamental normative protection.

## 16. Architectural Evolution

The architecture may therefore evolve through several implementation scales:

```text
Minimal prototype
       │
       ▼
Authorization AI Shell
       │
       ▼
Distributed AI / Security Architecture
       │
       ▼
AI-Enabled Operating System
```

The transition does not require replacing the fundamental security model.

The same architectural principle can remain:

> **AI capability may be external, distributed, probabilistic, and highly variable. Authorization and enforcement remain explicit, bounded, and controllable.**

## 17. Architectural Principle

The Authorization AI Shell establishes a separation between communication, intelligence, translation, authorization, and enforcement.

```text
AI
 │
 │ communication
 ▼
Security Core Input Boundary
 │
 ▼
Translator
 │
 ▼
Decisioner
 │
 ▼
Deterministic Security Core
 │
 ▼
Protected System
```

The resulting architecture allows a system to use powerful or externally provided AI capabilities while maintaining an independently defined local security boundary.

The system therefore does not need to choose between:

> **powerful external intelligence**

and

> **local control over protected resources.**

The architecture provides a mechanism for combining them.
