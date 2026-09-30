# Definition — Isolated System Context

## 1. System Context

For the purposes of this architecture, a **System** is the isolated execution context in which an Agent and all components belonging to that Agent are operated under the control of the Security Core.

The System is therefore defined by its **security boundary**, not by a particular physical device, operating system, process, or network location.

A System may be implemented on:

* a single physical device,
* a virtual machine,
* a containerized environment,
* dedicated hardware,
* an embedded platform,
* or a distributed set of resources,

provided that the complete execution environment is subject to the defined Security Architecture.

## 2. Isolated Context

The **isolated context** comprises the resources and communication channels that are available to the Agent and its components within the System boundary.

This includes, as applicable:

* computation,
* memory,
* persistent storage,
* local files,
* devices,
* network interfaces,
* credentials and cryptographic capabilities,
* inter-component communication,
* and protected Effects.

The isolation boundary determines which resources are considered part of the System and which are external to it.

## 3. Meaning of “Local”

The terms **local**, **locally**, and **local policy** refer to this isolated System context.

They do not necessarily mean:

* physically attached,
* located on the same machine,
* implemented in the same process,
* or disconnected from networks.

An external service may therefore remain external even when it is physically close to the System, while a virtualized resource may be considered part of the System if it is included within the defined security boundary.

Conversely, communication across the System boundary is an external information flow and is subject to the applicable Core authorization.

## 4. Agent Scope

The Agent comprises the complete set of components operating within the System under the defined architecture.

Conceptually:

```text
┌─────────────────────────────────────────────┐
│              Isolated System                │
│                                             │
│   ┌─────────────────────────────────────┐   │
│   │             Agent                   │   │
│   │                                     │   │
│   │ Planner   Counselor   other modules │   │
│   │          Security Core              │   │
│   │          Decisioner                 │   │
│   └─────────────────────────────────────┘   │
│                                             │
│        controlled resources / Effects       │
└─────────────────────────────────────────────┘
                     │
                     │ Core-mediated
                     ▼
              External Context
```

The exact physical placement of the components is therefore an implementation property.

The **security boundary** is the architectural property.

## 5. Local Policy

A **local security policy** is a policy extension associated with this isolated System context.

It applies to the Agent and its execution environment within that boundary.

Such a policy must be authenticated and authorized by the Security Core before it becomes active, as defined by the policy-extension rules.

The term `local` therefore means:

> **belonging to, and applicable within, the defined isolated System security context.**

It does not mean that the policy is inherently less trusted, nor does it imply that the policy may bypass the Security Core.

## 6. Architectural Invariant

The relevant unit of security is consequently not the machine or process but the **isolated System context**.

> **All components belonging to an Agent operate within a defined security boundary, and every information flow crossing that boundary is subject to the applicable Security Core authorization.**
