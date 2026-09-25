# Application Addendum — AI Agents on Security-Core Platforms

## 1. Program Space and Kernel Space as an Architectural Model

The terms **Program Space** and **Kernel Space** are used primarily as an architectural model.

They need not imply a conventional desktop or server operating system.

The same separation can be implemented on a wide range of computing platforms.

The essential distinction is functional:

```text
Program Space
    → Planner
    → Counselor
    → other AI components

Kernel / Security Core
    → Decisioner
    → Deterministic Authorization Core
```

The security boundary is therefore defined by authority and enforcement, not by the physical form of the device.

---

## 2. Kernel Components

The Decisioner and Deterministic Authorization Core can be implemented as kernel-level components comparable, conceptually, to security-relevant components of Unix-like operating systems.

The implementation may be:

* entirely software-based,
* partly implemented in privileged system software,
* partly implemented in firmware,
* or partly or entirely implemented in dedicated hardware.

The architectural requirement is not a particular implementation technology.

The requirement is that the authorization and enforcement boundary cannot be bypassed by the AI components operating outside that boundary.

---

## 3. Hardware-Assisted Security

The Decisioner and Core may therefore be implemented in or supported by dedicated security hardware.

Possible implementations include:

* secure processors,
* security controllers,
* trusted execution components,
* firmware-controlled security modules,
* hardware authorization engines,
* or other manufacturer-defined security components.

A hardware implementation can make the separation particularly strong because the enforcement boundary is physically or logically separated from the environment in which the AI components operate.

The architecture does not require all security functionality to be implemented in hardware.

A mixed implementation is possible:

```text
Decisioner
    ├── software component
    └── hardware-assisted component

Authorization Core
    ├── privileged software
    └── hardware enforcement
```

---

## 4. Platform Independence

The architecture is not restricted to PCs, servers, or conventional operating systems.

Potential platforms include, for example:

* smartphones,
* tablets,
* embedded computers,
* industrial controllers,
* robotics systems,
* vehicles,
* appliances,
* measurement and control hardware,
* network equipment,
* autonomous systems,
* and other programmable hardware.

The same principle applies regardless of platform:

> **The AI may operate on the platform, but it does not thereby acquire unrestricted authority over the platform.**

---

## 5. Web-Based and Local AI Components

Planner and Counselor do not necessarily have to execute locally.

They may operate:

* locally in Program Space,
* in a sandbox,
* in a separate application environment,
* on another computer,
* in a cloud environment,
* or through web-based services.

The architectural boundary remains meaningful as long as requests affecting the protected platform cross the defined Security-Core interface.

Conceptually:

```text
Remote / Web AI
      │
      │ request
      ▼
Security-Core Interface
      │
      ▼
Decisioner
      │
      ▼
Authorization Core
      │
      ▼
Protected Hardware
```

The location of the Planner or Counselor therefore does not itself determine their authority.

---

## 6. Manufacturer-Integrated Security Core

A particularly relevant deployment model is a hardware platform in which the manufacturer integrates the Security Core into the device.

The manufacturer may define:

* which operations are protected,
* which resources are accessible,
* which authorization policies apply,
* which AI interfaces are exposed,
* which operations require explicit authorization,
* and which hardware functions cannot be controlled without passing through the Security Core.

The manufacturer can then release the platform for use by AI agents together with the integrated security architecture.

This creates a fundamentally different relationship between an AI agent and the hardware.

The AI is not simply given unrestricted access to the device.

Instead:

```text
AI Agent
   │
   │ requests
   ▼
Security Core
   │
   │ authorized effects only
   ▼
Hardware
```

---

## 7. Platform-Level Authorization

Under this model, an AI agent can be granted meaningful access to the capabilities of a device without being granted unrestricted control over the device.

For example, an agent operating a device may be permitted to:

* read defined sensors,
* operate defined actuators,
* access defined communication interfaces,
* execute defined software operations,
* or control defined device functions.

Each such capability remains subject to the authorization architecture implemented by the platform.

The AI's general competence does not constitute permission to access every available hardware capability.

---

## 8. Manufacturer Responsibility

The security properties of such a deployment depend critically on the integrity of the manufacturer's Security Core and its interface.

A manufacturer that provides a platform for AI operation can therefore establish a security contract between:

```text
AI Agent
        ↕
Security-Core Interface
        ↕
Protected Platform
```

The agent can operate within the capabilities exposed by that contract without requiring the agent itself to be trusted with unrestricted control of the underlying hardware.

This allows the manufacturer to separate:

> **What the AI can request**

from:

> **What the hardware will actually permit.**

---

## 9. Generalization Beyond Conventional Computing

The architecture is therefore applicable wherever an intelligent software component needs access to consequential system capabilities.

The protected platform need not be a computer in the conventional sense.

It may be any system in which:

* an AI can formulate requests,
* a security boundary can evaluate those requests,
* and an enforcement mechanism can deterministically control the resulting effects.

The architecture can consequently be applied to both general-purpose computing platforms and specialized control systems.

---

## 10. Fundamental Deployment Principle

The central deployment principle is:

> **An AI agent may be highly capable without being highly privileged.**

A platform can provide powerful AI components with access to its capabilities while retaining an independent Security Core that controls whether requested operations are authorized and executable.

The resulting model is:

```text
                 AI AGENT
          ┌───────────────────┐
          │ Planner           │
          │ Counselor         │
          │ other components  │
          └─────────┬─────────┘
                    │
              requests
                    │
                    ▼
          ┌───────────────────┐
          │ SECURITY CORE     │
          │                   │
          │ Decisioner        │
          │ Authorization     │
          │ Core              │
          └─────────┬─────────┘
                    │
            authorized effects
                    │
                    ▼
          ┌───────────────────┐
          │ PLATFORM /        │
          │ HARDWARE          │
          └───────────────────┘
```

The physical implementation may vary.

The security principle does not:

> **The agent operates the platform through an independent authorization and enforcement boundary rather than by possessing unrestricted control of the platform.**
