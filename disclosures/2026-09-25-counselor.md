# Counselor

## Iterative Process Control Extension

**Type:** Technical Architecture Extension / Prior-Art Publication
**Extends:** Authorization Agent v1.0 and subsequent architectural extensions
**Status:** Public technical disclosure

---

## 1. Purpose

The **Counselor** is the iterative control component of the Authorization Agent architecture.

Its purpose is not to plan individual actions and not to authorize them.

Its purpose is to determine whether the Agent's process should:

* proceed to another iteration,
* terminate,
* or request a controlled break / escalation from the Core.

The Counselor therefore provides the architectural equivalent of an iterative:

```text
FOR / NEXT
```

control structure.

The fundamental principle is:

> **The Planner defines the instruction for an iteration. The Core determines and enforces the authorized effect. The Counselor determines whether another iteration should be initiated.**

---

# 2. Separation of Responsibilities

The three components have distinct responsibilities:

```text
Counselor
    → Should another iteration occur?

Planner
    → What should be attempted in this iteration?

Core
    → Is the requested transition authorized,
      and if so, what effect is produced?
```

This separation prevents the iterative control mechanism from becoming an implicit authorization mechanism.

---

# 3. Iterative Control Model

The basic process can be represented as:

```text
Counselor
    │
    │ NEXT
    ▼
Planner
    │
    │ iteration instruction
    ▼
Core
    │
    │ authorized effect
    ▼
System
    │
    │ resulting state / information
    ▼
Counselor
    │
    ├── NEXT ─────► new iteration
    │
    ├── BREAK ────► Core
    │
    └── COMPLETE ─► terminate process
```

The Counselor therefore controls the continuation of the process without controlling the authorization boundary of an individual action.

---

# 4. Counselor Objective

The Counselor has an explicit optimization objective:

> **Complete the authorized Objective successfully in as few iterations as reasonably possible.**

This is an optimization criterion for process control.

It does not mean that the Counselor is permitted to reduce or weaken authorization requirements in order to reduce the number of iterations.

Therefore:

```text
iteration efficiency
        ≠
authorization authority
```

The Counselor may seek a shorter path to successful completion, but every iteration remains subject to the existing authorization architecture.

---

# 5. Goal Assessment

The Counselor evaluates whether the Objective has been achieved.

Its assessment may use:

* the current protocol context,
* results of previous iterations,
* information explicitly available to it,
* state information supplied through the Core,
* and other authorized observations.

The Counselor may therefore ask the Core:

```text
GOAL_STATUS_REQUEST
```

For example:

```text
Counselor
    │
    │ "Is the authorized Objective satisfied?"
    ▼
Core
    │
    ▼
GOAL_STATUS_RESPONSE
```

The Core provides the response according to the defined protocol.

The Counselor does not infer that the goal has been achieved merely because the Planner reports success.

---

# 6. Planner Does Not Control Re-submission

The Planner may provide information relevant to goal achievement.

For example, the Planner may report:

```text
ITERATION_RESULT
GOAL_PROGRESS
RELEVANT_STATE
OBSERVATION
```

However, the Planner does not decide whether the process should be submitted for another iteration.

The decision to initiate another iteration belongs to the Counselor.

Thus:

```text
Planner:
    "This iteration produced result X."

        ≠

Planner:
    "Therefore submit another iteration."
```

The Counselor evaluates the result and determines whether another iteration is appropriate.

---

# 7. Re-submission Request

When the Counselor determines that additional work is required, it creates a new iteration request.

Conceptually:

```text
Counselor
    │
    │ RE-SUBMIT / NEXT
    ▼
Core
    │
    ▼
Planner
    │
    │ new iteration instruction
    ▼
Core
```

The re-submission does not bypass authorization.

A new iteration is a new authorization-relevant process step.

The applicable authorization rules continue to apply.

---

# 8. Break Request

The Counselor may also determine that continuation should stop or that the current process requires a controlled break.

It can therefore issue a:

```text
BREAK_REQUEST
```

to the Core.

Conceptually:

```text
Counselor
    │
    │ BREAK_REQUEST
    ▼
Core
```

The Core determines the corresponding deterministic transition according to the defined protocol.

The Counselor cannot use a break request to override a Core-enforced security state.

---

# 9. Counselor Does Not Control Maximum Iteration Count

The Counselor evaluates **goal achievement and process progress**.

It does not determine the maximum number of iterations permitted by the system.

The maximum iteration count is a Core-controlled constraint.

Therefore:

```text
Counselor:
    "Another iteration would be useful."

Core:
    "Iteration limit reached."

Core:
    HALT
```

The Counselor cannot override this transition.

This creates a deliberate separation:

```text
Counselor
    → qualitative / goal-oriented iteration decision

Core
    → deterministic iteration constraints
```

The Core therefore remains the final authority over hard iteration boundaries.

---

# 10. Counselor and Core

The Counselor may request a transition from the Core.

It cannot directly perform the transition.

For example:

```text
Counselor
    │
    │ NEXT_REQUEST
    ▼
Core
    │
    ├── permit next iteration
    ├── reject
    ├── escalate
    └── halt
```

Likewise:

```text
Counselor
    │
    │ BREAK_REQUEST
    ▼
Core
    │
    └── deterministic transition
```

This preserves the fundamental architecture:

> **The Counselor advises continuation. The Core controls the transition.**

---

# 11. Core as Conditional Enforcement

The architectural relationship can be understood through a programming analogy.

The Counselor provides the iterative control structure:

```text
FOR / NEXT
```

The Core provides the conditional enforcement boundary:

```text
IF authorized
    THEN effect
ELSE
    no effect / defined transition
```

The Planner supplies the instruction executed within an iteration.

Conceptually:

```text
COUNSELOR:
    NEXT?

        ↓

PLANNER:
    instruction for iteration N

        ↓

CORE:
    IF authorization permits instruction
        THEN perform effect
    ELSE
        enforce defined non-effect transition

        ↓

COUNSELOR:
    evaluate resulting state
```

This analogy is architectural rather than an implementation requirement.

---

# 12. Iteration Is Not Authorization

A new iteration must not inherit authorization merely because a previous iteration was authorized.

For example:

```text
Iteration 1
    Action A
    → ALLOW
    → EFFECT
```

does not imply:

```text
Iteration 2
    Action B
    → ALLOW
```

The second iteration remains subject to the applicable authorization process.

Thus:

> **Iteration continuity does not imply authorization continuity.**

---

# 13. Goal Achievement Does Not Authorize the Final Action

Likewise, the Counselor's assessment that an Objective has been achieved does not itself constitute authorization.

The Counselor may determine:

```text
GOAL = ACHIEVED
```

but the Core remains responsible for enforcing the resulting process transition.

Similarly, the Counselor may determine:

```text
GOAL = NOT ACHIEVED
```

without thereby acquiring authority to execute another action.

The Counselor requests another iteration.

The Planner proposes the next instruction.

The Core evaluates and enforces the applicable authorization transition.

---

# 14. Iteration State

Each iteration should have an explicit identifier.

For example:

```text
ITERATION_ID
PARENT_ITERATION_ID
OBJECTIVE_ID
COUNSELOR_DECISION
PLANNER_INSTRUCTION
CORE_DECISION
CORE_RESULT
GOAL_STATUS
TIMESTAMP
```

This permits the process to be reconstructed as:

```text
Iteration 1
    ↓
Iteration 2
    ↓
Iteration 3
    ↓
...
    ↓
Goal achieved / break / halt
```

The iteration history is part of the Core-controlled protocol context.

---

# 15. Counselor Optimization Boundary

The Counselor's optimization target is bounded.

It seeks:

```text
successful Objective completion
```

while minimizing:

```text
number of iterations
```

subject to:

```text
authorization constraints
Core constraints
information constraints
safety constraints
iteration limits
escalation rules
```

Therefore the Counselor cannot optimize by weakening the constraints themselves.

Formally:

```text
minimize iterations

subject to:
    authorization constraints
    Core invariants
    information boundaries
    execution constraints
    iteration limits
```

---

# 16. Fundamental Separation

The architecture can therefore be summarized as:

```text
                 COUNSELOR
                 FOR / NEXT
                     │
                     │
                     ▼
                  PLANNER
             iteration instruction
                     │
                     ▼
                   CORE
               IF / ENFORCE
                     │
                     ▼
                  EFFECT
                     │
                     ▼
               RESULT / STATE
                     │
                     ▼
                 COUNSELOR
```

Each component has a distinct function:

```text
Counselor
    → iteration control

Planner
    → iteration instruction

Decisioner
    → authorization decision

Core
    → deterministic enforcement
```

The central principle is:

> **The Counselor controls iteration, the Planner defines the work of an iteration, the Decisioner evaluates authorization, and the Core enforces the resulting transition.**

The Counselor may extend the process horizontally through additional iterations, but it cannot extend the Agent's authorization boundary.

---

## Initial Disclosure of This Extension

**Date:** 2026-09-25

**Extension:** Counselor — Iterative Process Control

**Base Architecture:** Authorization Agent v1.0, 2026-09-23

**Purpose:** Public technical disclosure / prior-art publication
