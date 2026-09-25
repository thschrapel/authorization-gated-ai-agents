# Counselor

## Iterative Process Control Extension

**Type:** Technical Architecture Extension / Prior-Art Publication
**Extends:** Authorization Agent v1.0 and subsequent architectural extensions
**Status:** Public technical disclosure

---

## 1. Purpose

The **Counselor** is the iterative control component of the Authorization Agent architecture.

Its purpose is to determine whether the current process should:

* begin another iteration,
* terminate,
* or request a controlled break from the Core.

The Counselor therefore provides the architectural equivalent of an iterative:

```text
FOR / NEXT
```

control structure.

The Counselor does not define the concrete instruction executed within an iteration and does not authorize that instruction.

The fundamental separation is:

```text
Counselor
    → requests a process transition

Planner
    → defines the instruction for an iteration

Decisioner
    → evaluates authorization

Core
    → performs the deterministic transition and enforces authorization
```

---

# 2. Two Different Request Types

The architecture distinguishes between **process-control requests** from the Counselor and **action requests** from the Planner.

### Counselor

The Counselor may submit requests such as:

```text
NEXT_ITERATION_REQUEST
BREAK_REQUEST
GOAL_STATUS_REQUEST
```

These requests concern the state of the overall process.

### Planner

The Planner submits the concrete instruction for the current iteration:

```text
ITERATION_INSTRUCTION
```

This instruction may subsequently require information authorization and/or action authorization.

The two request classes must not be conflated.

```text
Counselor → Core
    process control

Planner → Core
    concrete operation
```

---

# 3. Correct Counselor–Core Flow

The fundamental flow is:

```text
Counselor
    │
    │ PROCESS-CONTROL REQUEST
    ▼
Core
    │
    │ deterministic transition
    ▼
Process State
```

For a new iteration:

```text
Counselor
    │
    │ NEXT_ITERATION_REQUEST
    ▼
Core
    │
    │ if continuation is permitted
    ▼
ITERATION OPEN
    │
    ▼
Planner
```

The Planner only receives the iteration after the Core has established the corresponding process state.

---

# 4. NEXT Iteration

The Counselor evaluates the current process state and may determine that another iteration is required.

It then submits:

```text
NEXT_ITERATION_REQUEST
```

The Core evaluates the request against its deterministic constraints.

If continuation is permitted:

```text
NEXT_ITERATION_REQUEST
        │
        ▼
       Core
        │
        ▼
ITERATION OPEN
        │
        ▼
     Planner
```

The Planner then defines the instruction for that iteration.

The Planner does not decide whether the iteration exists.

The Counselor does not define the instruction.

The Core establishes the iteration state.

---

# 5. Planner Instruction

Once an iteration has been opened, the Planner generates the instruction for that iteration.

```text
Core
  │
  │ iteration opened
  ▼
Planner
  │
  │ ITERATION_INSTRUCTION
  ▼
Core
```

The Core then applies the normal authorization architecture to that instruction.

Conceptually:

```text
ITERATION_INSTRUCTION
        │
        ▼
    Decisioner
        │
   ┌────┼────┐
   ▼    ▼    ▼
 ALLOW DENY CLARIFY
   │    │    │
   ▼    ▼    ▼
 Core  Core  clarification
   │
   ▼
 EFFECT
```

Thus the Counselor's decision to continue does not authorize the Planner's instruction.

---

# 6. BREAK Request

The Counselor may determine that continuation should stop or that a controlled break is required.

It submits:

```text
BREAK_REQUEST
```

The request is sent directly to the Core because the resulting process transition belongs to the Core's security and control boundary.

```text
Counselor
    │
    │ BREAK_REQUEST
    ▼
Core
    │
    ▼
BREAK / HALT STATE
```

The Counselor does not directly terminate the Core.

It requests the transition.

The Core performs the transition.

---

# 7. Goal Status Request

The Counselor may also request information from the Core about whether the Objective has been achieved.

```text
Counselor
    │
    │ GOAL_STATUS_REQUEST
    ▼
Core
    │
    ▼
GOAL_STATUS_RESPONSE
    │
    ▼
Counselor
```

The Planner may provide information relevant to determining goal achievement.

However, the Planner does not control the goal-status query.

For example:

```text
Planner:
    ITERATION_RESULT
    "The requested operation produced result X."
```

does not itself cause:

```text
GOAL_STATUS_REQUEST
```

The Counselor independently decides whether it requires a goal-status evaluation.

---

# 8. Goal Status Is Not Iteration Authorization

The Core's response to a goal-status request is information for the Counselor.

For example:

```text
GOAL_STATUS = ACHIEVED
```

or:

```text
GOAL_STATUS = NOT_ACHIEVED
```

does not itself determine the next process transition.

The Counselor interprets the result within its process-control objective.

For example:

```text
GOAL_STATUS = NOT_ACHIEVED
        │
        ▼
Counselor
        │
        └── NEXT_ITERATION_REQUEST
```

or:

```text
GOAL_STATUS = ACHIEVED
        │
        ▼
Counselor
        │
        └── COMPLETE
```

The Core remains responsible for enforcing the resulting process transition.

---

# 9. Maximum Iteration Count

The Counselor evaluates whether another iteration is useful for achieving the Objective.

It does **not** determine the maximum permitted number of iterations.

That limit belongs to the Core.

Therefore:

```text
Counselor:
    NEXT_ITERATION_REQUEST

Core:
    ITERATION_LIMIT_REACHED
```

results in:

```text
HALT / DENY CONTINUATION
```

The Counselor cannot override the limit.

This creates an intentional separation:

```text
Counselor
    → Is another iteration useful?

Core
    → Is another iteration permitted?
```

---

# 10. Counselor Optimization Objective

The Counselor has the process-level optimization objective:

> **Complete the authorized Objective successfully in as few iterations as reasonably possible.**

The Counselor therefore evaluates:

* progress toward the Objective,
* results of previous iterations,
* whether additional work is useful,
* whether the Objective has been achieved,
* and whether another iteration is justified.

It does not optimize by weakening authorization, safety, or Core constraints.

Conceptually:

```text
minimize iterations
        subject to
        ├── authorization constraints
        ├── Core constraints
        ├── information constraints
        ├── safety constraints
        └── iteration limits
```

---

# 11. Counselor Cannot Become an Authorization Layer

The Counselor's `NEXT_ITERATION_REQUEST` does not mean:

```text
ALLOW
```

Likewise:

```text
BREAK_REQUEST
```

does not mean:

```text
HALT
```

The Counselor requests process transitions.

The Core determines the corresponding deterministic transition.

Therefore:

> **The Counselor controls iteration requests, not authorization.**

---

# 12. Complete Iteration Cycle

A complete iteration cycle is therefore:

```text
                 ┌──────────────┐
                 │  Counselor   │
                 └──────┬───────┘
                        │
              NEXT_ITERATION_REQUEST
                        │
                        ▼
                 ┌──────────────┐
                 │     Core     │
                 └──────┬───────┘
                        │
                  ITERATION OPEN
                        │
                        ▼
                 ┌──────────────┐
                 │    Planner   │
                 └──────┬───────┘
                        │
               ITERATION_INSTRUCTION
                        │
                        ▼
                 ┌──────────────┐
                 │  Decisioner  │
                 └──────┬───────┘
                        │
                 authorization
                        │
                        ▼
                 ┌──────────────┐
                 │     Core     │
                 └──────┬───────┘
                        │
                      EFFECT
                        │
                        ▼
                 resulting state
                        │
                        ▼
                 ┌──────────────┐
                 │  Counselor   │
                 └──────────────┘
```

The cycle may terminate through:

```text
GOAL ACHIEVED
BREAK
HALT
ITERATION LIMIT
AUTHORIZATION FAILURE
ESCALATION
```

---

# 13. Fundamental Separation

The resulting architecture can be summarized as:

```text
Counselor
    FOR / NEXT
        │
        │ process-control request
        ▼
Core
    IF / deterministic transition
        │
        ▼
Planner
    iteration instruction
        │
        ▼
Decisioner
    authorization evaluation
        │
        ▼
Core
    authorization enforcement
        │
        ▼
Effect
        │
        ▼
Counselor
```

The programming analogy is therefore:

> **The Counselor is the iterative control structure. The Planner is the body of the iteration. The Decisioner evaluates authorization. The Core is the deterministic conditional and enforcement boundary.**

Most importantly:

> **A Counselor request can initiate or terminate a process transition, but it can never itself authorize the concrete effect produced within an iteration.**

---

## Initial Disclosure of This Extension

**Date:** 2026-09-25

**Extension:** Counselor — Iterative Process Control

**Base Architecture:** Authorization Agent v1.0, 2026-09-23

**Purpose:** Public technical disclosure / prior-art publication
