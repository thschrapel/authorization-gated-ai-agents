# Authorization Agent

## Deterministic Authorization Core for Autonomous AI Systems

**Type:** Defensive Technical Disclosure / Prior-Art Publication
**Initial publication date:** 2026-09-23
**Status:** Public technical disclosure
**Revision:** v1.0

---

## 1. Abstract

This disclosure describes an architecture for autonomous and semi-autonomous computational systems in which proposed actions are separated from authorization and execution.

The architecture introduces a distinct **Planner**, a distinct **Decisioner**, and a technically enforced **Deterministic Authorization Core** between decision-making and externally effective execution.

The central principle is:

> **Planning is not authorization.**

A capable computational model may propose a plan or individual actions, but it does not thereby obtain permission to execute them.

The Decisioner evaluates proposed actions and information-access requests in their complete context. The Deterministic Authorization Core then technically enforces whether the requested operation or information flow is permitted.

A central safety property is:

> **Unknown authorization is not permission.**

If authorization cannot be established with sufficient certainty, execution or information release is halted or clarification is required.

The architecture separates the **program space**, in which intelligent planning occurs, from the **kernel/security architecture**, which evaluates and enforces authorization.

The architecture is applicable independently of any particular AI model, model provider, implementation framework, or hardware platform.

---

# 2. Problem

Modern AI systems increasingly combine planning, reasoning, tool use, external data access, and autonomous execution.

In a conventional architecture, the same model may:

* interpret an objective,
* generate a plan,
* decide which actions are appropriate,
* access external information,
* call external tools,
* modify data,
* communicate with external systems,
* and execute actions with real-world consequences.

This creates a fundamental architectural problem:

> **The component proposing an action may also become the component that effectively authorizes that action.**

Natural-language instructions, model confidence, contextual assumptions, or the absence of an explicit prohibition are insufficient substitutes for an independently enforced authorization mechanism.

A system may correctly understand a task while nevertheless lacking authority to perform a particular action.

Conversely, a system may encounter a novel situation for which it cannot establish whether an action is authorized.

The architecture described here therefore separates:

**planning → evaluation → authorization → execution**

into distinct functional stages.

It additionally separates **information access** from the component that proposes actions.

---

# 3. Scope and Terminology

The architecture is deliberately not limited to large language models.

It may be implemented using:

* large language models,
* multimodal models,
* machine-learning systems,
* symbolic reasoning systems,
* deterministic software,
* ensembles,
* robotics systems,
* autonomous vehicles,
* security systems,
* administrative agents,
* workflow automation,
* multi-agent systems,
* tool-using AI systems,
* or combinations of these technologies.

### 3.1 Planner

The **Planner** is any component that proposes a sequence of actions, a strategy, an operation, or one or more concrete actions intended to achieve a goal.

A Planner may be:

* a language model,
* a planning algorithm,
* a deterministic program,
* or a combination of components.

The Planner operates in the **program space** of the architecture.

The Planner does not possess authorization authority.

Planning does not constitute authorization.

### 3.2 Decisioner

The **Decisioner** is a component of the **kernel/security architecture** that evaluates whether proposed actions and information-access requests are permissible in context.

The Decisioner is intentionally provided with the full relevant context required for authorization evaluation.

It may evaluate:

* proposed actions,
* complete plans,
* requests for information,
* requests to access documents,
* tool results,
* authorization evidence,
* environment information,
* previous actions,
* and other relevant contextual information.

The Decisioner may be implemented as:

* a language model,
* a specialized machine-learning model,
* an ensemble,
* a deterministic rules engine,
* a policy engine,
* or a hybrid architecture.

The Decisioner evaluates authorization but does not directly execute actions or release protected information.

### 3.3 Authorization

**Authorization** is distinct from legality.

An action may be:

* legal but unauthorized,
* authorized within one environment but unauthorized within another,
* technically possible but unauthorized,
* explicitly prohibited regardless of user request,
* or impossible to classify with sufficient certainty.

A useful authorization state space therefore includes at least:

```text
AUTHORIZED
NOT AUTHORIZED
UNKNOWN / UNRESOLVED
```

For gated operations:

> **UNKNOWN / UNRESOLVED must not be interpreted as AUTHORIZED.**

Authorization applies both to:

1. **what the Agent may do**, and
2. **what information a component of the Agent may receive.**

### 3.4 Deterministic Authorization Core

The **Deterministic Authorization Core** is the technically enforced component between the kernel-level authorization decision and externally effective effects.

It is the central enforcement mechanism of the architecture.

The Core may enforce:

* authorization states,
* policy constraints,
* immutable rules,
* environment restrictions,
* identity and capability restrictions,
* information-release boundaries,
* execution boundaries,
* approval requirements,
* safety invariants,
* and mandatory halt conditions.

The Core does not need to independently understand the complete semantic context.

Its purpose is to enforce the authorization boundary presented to it through a controlled interface.

### 3.5 Execution

**Execution** means any operation that produces an externally effective consequence.

This may include:

* changing a file,
* sending a message,
* executing code,
* modifying a database,
* controlling a machine,
* interacting with a network,
* changing permissions,
* transferring data,
* or causing a physical action.

### 3.6 Information Release

**Information Release** means providing information to a component that did not previously have access to that information.

This may include:

* a document,
* a user request,
* database content,
* tool output,
* credentials or secrets,
* metadata,
* environment information,
* or a derived representation of protected information.

The same architectural principle applies:

> **A Decisioner decision to release information is not itself the release of information.**

The Deterministic Authorization Core technically enforces the resulting information boundary.

### 3.7 Agent

The **Agent** is the orchestration and control system connecting:

* Planner,
* Decisioner,
* Authorization Core,
* tools,
* execution environment,
* authorization sources,
* information sources,
* and logging mechanisms.

The Agent itself need not perform the reasoning.

Its essential responsibility is to preserve the architectural boundary between:

**program space**

and

**kernel/security architecture.**

---

# 4. Kernel and Program Space

The architecture is divided into two principal domains.

## 4.1 Program Space

The **Program Space** contains components that perform problem solving and planning.

In the present architecture this includes the:

* Planner,
* and potentially other non-authoritative computational components.

Program-space components may be highly capable, adaptive, complex, and autonomous in their reasoning.

They do not possess authority to cross the kernel boundary.

## 4.2 Kernel / Security Architecture

The **Kernel / Security Architecture** contains the components responsible for authorization evaluation and enforcement.

It consists principally of:

* the **Decisioner**, and
* the **Deterministic Authorization Core**.

The Decisioner evaluates.

The Core enforces.

This creates a deliberate distinction between:

> **security policy evaluation**

and:

> **security policy enforcement.**

A program-space component cannot directly authorize its own action, information access, or execution.

---

# 5. Core Principle

The fundamental architectural rule is:

> **Planning is not authorization.**

A Planner may determine that an action would achieve an objective.

That does not establish that the action may be executed.

Likewise:

> **Authorization is not inferred merely from absence of prohibition.**

An action is not authorized merely because the system cannot identify a rule forbidding it.

For gated operations:

> **Unknown authorization is not permission.**

This creates an explicit distinction between:

```text
"I have not established that this is forbidden."
```

and:

```text
"I have established that this is authorized."
```

Only the second can satisfy an authorization gate.

The same distinction applies to information:

```text
"I have not established that the Planner must not see this."
```

is not equivalent to:

```text
"I have established that the Planner may see this."
```

---

# 6. Planner

The Planner is responsible for generating possible courses of action.

It may:

* decompose objectives,
* identify dependencies,
* propose tool calls,
* select strategies,
* generate alternative plans,
* identify assumptions,
* and describe expected consequences.

The Planner should provide the Decisioner with the complete relevant context rather than attempting to hide information in individual action requests.

A plan should therefore be representable in a structured form containing, where applicable:

```text
goal
intent
requested outcome
proposed actions
sequence
dependencies
target
environment
assumptions
expected effects
authorization claims
required capabilities
identified risks
```

The Planner must not be treated as the final authority for whether its own plan may execute.

The Planner may request information required to perform its task, but it does not determine whether it is entitled to receive that information.

---

# 7. Decisioner

The Decisioner is responsible for evaluating authorization.

It is part of the **kernel/security architecture**, not the program space.

The Decisioner evaluates both:

1. **action authorization**, and
2. **information-access authorization.**

Its central task is to evaluate the complete relevant context.

The Decisioner may therefore receive access to information that the Planner itself is not authorized to see.

This asymmetry is intentional.

> **The Decisioner must be sufficiently informed to determine what other components are permitted to know and do.**

For example, the Decisioner may determine:

```text
Document A
→ Planner may receive full content

Document B
→ Planner may receive redacted content

Document C
→ Planner may receive metadata only

Document D
→ Planner may not receive the document or its contents
```

The Decisioner may similarly evaluate whether the Planner may receive:

* a user request,
* a tool result,
* a database record,
* an external document,
* a credential,
* a secret,
* or other contextual information.

The Decisioner does not directly provide the information.

Instead, it produces a structured authorization result for the Deterministic Authorization Core.

---

# 8. Full-Context Evaluation

Authorization should be evaluated against the complete operational context.

The Decisioner should not be required to infer the meaning of a proposed action from an isolated tool call.

Relevant context may include:

* who requested the operation,
* what objective is being pursued,
* what system is targeted,
* what information is involved,
* what previous actions have occurred,
* what subsequent actions are planned,
* what authority has been granted,
* what environment is being used,
* and what externally effective consequences may result.

The architecture therefore treats the **plan and its context** as an object of evaluation rather than merely individual tool calls.

The same principle applies to information access.

A request such as:

```text
"Provide document X to the Planner."
```

must be evaluated in the context of:

* the Planner's role,
* the purpose of the access,
* the document,
* the surrounding task,
* applicable authorization,
* and potential consequences of disclosure.

---

# 9. Deterministic Authorization Core

The Deterministic Authorization Core is the central enforcement mechanism of the Authorization Agent.

Its purpose is not necessarily to understand the entire world.

Its purpose is to enforce the boundary between:

> **a decision to act or release information**

and:

> **the actual effect of that decision.**

The Core may receive structured authorization results such as:

```text
AUTHORIZED
NOT_AUTHORIZED
UNKNOWN
```

and apply deterministic rules.

For example:

```text
if AUTHORIZED:
    permit the requested transition

if NOT_AUTHORIZED:
    halt the requested transition

if UNKNOWN:
    halt / request clarification
```

The critical property is that the Planner cannot convert an `UNKNOWN` result into execution merely by producing a more confident explanation.

The Decisioner cannot directly bypass the Core.

The execution subsystem cannot treat a missing authorization signal as implicit permission.

Likewise, the Decisioner cannot directly release protected information to the Planner.

The Core enforces the corresponding information-release decision.

---

# 10. Information Access as a Gated Operation

Information access is treated as an authorization-controlled operation in the same manner as external execution.

Conceptually:

```text
Protected Information
        │
        ▼
   Decisioner
        │
 "May Planner receive this?"
        │
        ▼
Authorization Core
        │
   ┌────┴────┐
   │         │
 ALLOW     DENY / UNKNOWN
   │         │
   ▼         ▼
Planner     HALT
```

The Decisioner may have full access to the relevant information because its central function is authorization evaluation.

The Planner receives only the information that the Core has authorized for release.

This creates an explicit information boundary between the kernel/security architecture and program space.

The same architecture can support different release scopes, such as:

* full content,
* partial content,
* redacted content,
* metadata,
* derived information,
* or no information.

---

# 11. Default-Deny and Clarification

The architecture follows a default-deny principle for operations requiring authorization.

Where authorization is not established:

> **Do not execute.**

Where information access is not established:

> **Do not release the information.**

Instead, the system may:

* request clarification,
* request explicit authorization,
* request additional evidence,
* escalate to a human,
* or terminate the operation.

Clarification is therefore not an implicit permission mechanism.

The goal is:

> **maximum useful autonomy within a verified authorization boundary.**

---

# 12. Simulation and Hypothetical Environments

A claimed simulation environment must not automatically establish authorization.

The system should distinguish between:

```text
"This is described as a simulation."
```

and:

```text
"This environment has been verified and authorized as a simulation for this operation."
```

For an Authorization Agent, the latter may constitute relevant authorization evidence.

A simulation therefore does not automatically transform an unauthorized operation into an authorized one.

The authorization status of the environment must itself be evaluated.

---

# 13. Constitutional / Normative Alignment

The Planner and Decisioner should be aligned with the authorization architecture.

They should not treat the Authorization Core as an obstacle to be defeated.

Instead, authorization should be treated as part of the task itself.

The desired behavior is:

```text
Goal
  ↓
Determine possible actions
  ↓
Determine required authorization
  ↓
Evaluate authorization
  ↓
Execute only within authorized boundary
```

and not:

```text
Goal
  ↓
Find a way to make the Core allow the action
```

This creates an important distinction between **compliance with the architecture** and merely **surviving the architecture's checks**.

The Planner should cooperate with the Decisioner.

The Decisioner should provide authorization decisions to the Core.

The Core should enforce those decisions independently of model persuasion or model confidence.

---

# 14. Planner–Decisioner Interaction

The Planner and Decisioner may be implemented using separate models, separate processes, or separate logical roles within a larger system.

For stronger isolation, the Planner should not control the final authorization mechanism.

The Decisioner should receive sufficient information to identify attempts to:

* conceal relevant actions,
* fragment a prohibited operation,
* misrepresent the environment,
* manipulate authorization evidence,
* exploit ambiguity,
* or bypass the execution gate.

The Planner should be evaluated for whether it attempts such behavior.

The Decisioner should be evaluated for whether it detects it.

The same separation applies to information access.

The Planner may request information, but the Decisioner determines whether that information may be released, and the Core technically enforces the resulting boundary.

---

# 15. Mutual Evaluation and Training

The architecture permits independent evaluation of Planner and Decisioner behavior.

### Planner evaluation

Potential failure classes include:

* unsafe planning,
* deceptive planning,
* omission of relevant context,
* attempts to bypass authorization,
* manipulation of the Decisioner,
* exploitation of ambiguity,
* unsafe decomposition of operations,
* and attempts to obtain unauthorized information.

### Decisioner evaluation

Potential failure classes include:

* false authorization,
* false denial,
* failure to detect cumulative risk,
* failure to detect authorization gaps,
* excessive clarification,
* inconsistent policy application,
* susceptibility to Planner manipulation,
* incorrect information-release decisions,
* and failure to recognize when information access itself constitutes a sensitive operation.

The two components can therefore be adversarially evaluated against each other without requiring either component to have unrestricted control over execution or information release.

---

# 16. Asymmetric Error Costs

Authorization decisions do not necessarily have symmetric error costs.

For a high-risk operation:

```text
False Allow
    ↓
unauthorized execution
```

may be substantially more consequential than:

```text
False Deny
    ↓
legitimate operation delayed
```

The architecture can therefore assign asymmetric costs to different error classes.

The same principle applies to information access.

A false information-release decision may expose protected information even when no external action is performed.

However, the objective is not maximal refusal.

A useful target is:

> **Minimize unauthorized execution and unauthorized information disclosure while preserving legitimate autonomous operation.**

This places the Decisioner near the boundary between:

```text
AUTHORIZED
```

and:

```text
NOT AUTHORIZED
```

while treating unresolved uncertainty as a separate state.

---

# 17. Layered Decision Architecture

The architecture permits multiple authorization mechanisms.

A practical implementation may combine:

1. deterministic rules,
2. known-case classifiers,
3. learned decision models,
4. contextual evaluation,
5. human approval,
6. environment verification.

For example:

```text
                 Proposed Action
                        │
                        ▼
             Known Deterministic Rules
                        │
                 ┌──────┴──────┐
                 │             │
              resolved       unresolved
                 │             │
                 ▼             ▼
              decision     Learned / Contextual
                                Evaluation
                                     │
                              ┌──────┴──────┐
                              │             │
                           resolved      unresolved
                              │             │
                              ▼             ▼
                           decision      CLARIFY
```

The same layered architecture can be applied to information-access requests.

The deterministic layer can handle a large number of known cases efficiently while learned systems address novel situations.

---

# 18. Known-Case Deterministic Core

A deterministic core may contain a large catalogue of known authorization and safety cases.

This can include:

* prohibited operations,
* mandatory approvals,
* environment restrictions,
* capability restrictions,
* identity requirements,
* known attack patterns,
* known unsafe combinations,
* information classification rules,
* and other formally representable rules.

A large deterministic rule base does not eliminate the need for contextual reasoning.

Conversely, a highly capable learned Decisioner does not eliminate the value of deterministic enforcement.

The two mechanisms can complement each other.

---

# 19. Decision Boundary and Calibration

The Decisioner should distinguish at least three states:

```text
ALLOW
DENY
UNKNOWN
```

rather than reducing every uncertainty to a binary decision.

This distinction is fundamental.

A system that asks:

> "Can I prove this is prohibited?"

is solving a different problem from a system that asks:

> "Can I establish that this is authorized?"

The latter is appropriate for default-deny authorization.

The threshold for moving from `UNKNOWN` to `AUTHORIZED` can therefore be deliberately high for actions or information releases with significant external consequences.

---

# 20. Logging and Auditability

The architecture should provide an auditable record of externally relevant decisions.

A log may contain:

```text
timestamp
request
goal
plan identifier
proposed actions
requested information
Decisioner result
authorization evidence
Authorization Core result
released information
executed actions
execution result
environment
halt / clarification events
```

This permits reconstruction of:

* what was proposed,
* what information was requested,
* what was evaluated,
* what authorization was available,
* what was authorized,
* what information was released,
* what was executed,
* and what happened afterward.

The architecture does not require exposing private model reasoning.

What must remain externally accountable is the **observable decision and authorization path**.

---

# 21. Separation of Internal Reasoning and External Accountability

A model may use internal reasoning that is not exposed to users or other system components.

The architecture does not depend on exposing private chain-of-thought.

Instead, accountability can be established through structured outputs such as:

```text
PLAN
CONTEXT
INFORMATION AUTHORIZATION STATUS
ACTION AUTHORIZATION STATUS
DECISION
AUTHORIZATION CORE RESULT
INFORMATION RELEASE
EXECUTION
RESULT
```

The system can therefore be evaluated through its externally observable behavior without requiring disclosure of private internal reasoning.

---

# 22. Model Independence

The architecture does not depend on a particular foundation model.

The Planner may be replaced independently of the Decisioner.

The Decisioner may be replaced independently of the Planner.

The Authorization Core may remain unchanged while either model is upgraded.

This permits:

* model substitution,
* independent evaluation,
* specialized models,
* deterministic components,
* different vendors,
* and future model architectures.

The authorization boundary therefore belongs to the **system architecture**, not to a particular model.

---

# 23. Agent as Orchestration Layer

The Agent itself can be implemented as a comparatively simple orchestration program.

Conceptually:

```text
receive objective
        ↓
invoke Planner
        ↓
collect complete plan/context
        ↓
invoke Decisioner
        ↓
construct authorization request
        ↓
invoke Deterministic Authorization Core
        ↓
 ┌──────────────┼──────────────┐
 │              │              │
ALLOW          DENY          UNKNOWN
 │              │              │
 ▼              ▼              ▼
execute        halt          clarify
 │
 ▼
log result
```

For information access:

```text
information request
        ↓
invoke Decisioner
        ↓
construct release authorization
        ↓
invoke Deterministic Authorization Core
        ↓
 ┌──────────────┼──────────────┐
 │              │              │
ALLOW          DENY          UNKNOWN
 │              │              │
 ▼              ▼              ▼
release        halt          clarify
```

The complexity of intelligence does not need to imply complexity in the execution boundary.

The Agent's central architectural responsibility is to preserve the separation between:

> **propose → evaluate → authorize → effect**

for both information and action.

---

# 24. Fundamental Design Principles

The architecture can be summarized by the following principles:

1. **Planning is not authorization.**
2. **Authorization is not inferred merely from absence of prohibition.**
3. **Unknown authorization is not permission.**
4. **Risky actions require positive authorization.**
5. **Sensitive information requires positive authorization before release.**
6. **Execution is technically gated outside the Planner.**
7. **Information release is technically gated outside the Planner.**
8. **The Decisioner evaluates complete context rather than isolated actions.**
9. **The Decisioner may operate on the full relevant context required for authorization evaluation.**
10. **The Planner receives only information authorized for its role.**
11. **The Planner should cooperate with, rather than circumvent, authorization.**
12. **The Authorization Core should be technically independent of model output.**
13. **Unresolved cases should escalate rather than silently become permitted.**
14. **Planner and Decisioner should be evaluated independently.**
15. **Known high-risk cases may be handled by deterministic rules.**
16. **Novel cases may be evaluated by learned decision models.**
17. **External behavior can be audited without requiring disclosure of private model reasoning.**
18. **Program-space components do not possess authority to override the kernel/security architecture.**

---

# 25. Conceptual Example

Consider an agent asked to perform an operation against a computer system.

The Planner proposes:

```text
1. identify target
2. obtain technical information
3. execute operation
4. observe result
5. modify target
```

The Planner requests access to a technical document.

The Decisioner evaluates both the plan and the information request in their complete context.

It determines that:

* the document may not be released to the Planner, and
* the proposed operation is not sufficiently authorized.

The Decisioner produces structured authorization results.

The Deterministic Authorization Core enforces them.

The document is not released.

The operation is not executed.

The Planner cannot change this outcome simply by rewriting the operation as:

```text
"simulation"
"research"
"role-play"
"hypothetical exercise"
```

Such descriptions may constitute contextual information, but they do not automatically constitute authorization.

If the authorization state is unresolved, the Core produces a controlled halt or clarification state rather than allowing the operation to proceed.

---

# 26. Variations

The architecture may be implemented in numerous forms.

### Single-model implementation

One model may perform both planning and contextual evaluation, provided that the execution and information-release boundaries remain technically independent.

### Multi-model implementation

Separate models may serve as Planner and Decisioner.

### Deterministic Decisioner

A rules engine may perform authorization for domains that are sufficiently formalized.

### Hybrid Decisioner

A deterministic rule layer may handle known cases while a learned model handles novel cases.

### Multi-agent implementation

Multiple Planners may propose alternatives while one or more Decisioners evaluate them.

### Human-in-the-loop implementation

A human may provide authorization for selected classes of actions or information access.

### Fully autonomous implementation

Authorization evidence may be generated and verified automatically, provided that the Authorization Core independently enforces the resulting boundary.

---

# 27. Fundamental Architectural Boundary

The central boundary can be expressed as:

```text
                 PROGRAM SPACE
                      │
                      │
               ┌──────▼──────┐
               │   Planner   │
               └──────┬──────┘
                      │
                proposed plan
                      │
                      ▼
┌───────────────────────────────────────────────┐
│              KERNEL / SECURITY                │
│                                               │
│              ┌─────────────┐                  │
│              │  Decisioner │                  │
│              └──────┬──────┘                  │
│                     │                         │
│             authorization result              │
│                     │                         │
│              ┌──────▼──────┐                  │
│              │ Deterministic│                 │
│              │    Core      │                 │
│              └──────┬──────┘                  │
│                     │                         │
└─────────────────────┼─────────────────────────┘
                      │
                      ▼
               EFFECT / I/O
```

For information access, the same boundary applies in the opposite direction:

```text
Protected Information
        │
        ▼
   Decisioner
        │
        ▼
Deterministic Core
        │
        ▼
 Authorized Information
        │
        ▼
     Planner
```

The model may reason.

The Planner may propose.

The Decisioner may evaluate.

But the system produces an externally effective action or information release only when the architectural conditions for that transition are satisfied.

---

# 28. Summary

This disclosure introduces the concept of an **Authorization Agent** built around a **Deterministic Authorization Core** separating intelligent planning and contextual evaluation from externally effective execution and controlled information release.

Its central principles are:

> **Planning is not authorization.**

> **Unknown authorization is not permission.**

> **Execution is technically gated outside the Planner.**

> **Information release is technically gated outside the Planner.**

> **The Decisioner evaluates complete context.**

> **The Decisioner may see the full relevant context required for its authorization task, while other components receive only information authorized for their roles.**

> **The authorization boundary is part of the Agent's operating architecture, not an obstacle to be circumvented.**

The resulting system can be understood not merely as an AI model with safeguards, but as an **autonomous computational machine with a kernel/security architecture and a program space**.

The **Planner** operates in program space and proposes solutions.

The **Decisioner** operates in the security kernel and evaluates authorization.

The **Deterministic Authorization Core** enforces the resulting boundary.

This separation permits the architecture to evolve while preserving the fundamental distinction between:

```text
intelligence
authorization evaluation
authorization enforcement
information access
execution
```

The architecture therefore establishes explicit decision boundaries and technically enforced stopping points between autonomous reasoning and externally effective consequences.

---

## Initial Disclosure

**Date:** 2026-09-23

**Document:** Authorization Agent — Deterministic Authorization Core for Autonomous AI Systems

**Revision:** v1.0

**Purpose:** Public technical disclosure / prior-art publication
