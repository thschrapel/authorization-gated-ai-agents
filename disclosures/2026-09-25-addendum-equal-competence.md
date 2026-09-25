# Counselor Addendum — Equal Competence, Different Optimization Objectives

## 1. Planner and Counselor as Peer Modules

The Planner and Counselor are not inherently hierarchical components.

Both may be highly capable, specialized AI modules operating at comparable levels of competence.

Their distinction is not primarily one of capability or rank.

Their distinction is their **optimization objective**.

The architecture therefore permits:

```text
Planner     → optimization of the next iteration
Counselor   → optimization of the process trajectory
```

Neither module derives authority over the other from its competence.

---

## 2. Different Optimization Objectives

The Planner is responsible for determining an appropriate instruction for an opened iteration.

Its central question is:

> **What should be done next?**

The Counselor is responsible for evaluating whether and how the process should continue.

Its central question is:

> **Should there be a next iteration, and is continued effort justified?**

These questions are related but not identical.

A Planner may determine that a particular action is technically or strategically promising.

The Counselor may determine that performing that action is no longer justified by the expected value relative to its cost.

Both assessments may be correct simultaneously.

---

## 3. Local and Global Optimization

The distinction can be understood as two different optimization horizons.

### Planner

The Planner performs **local optimization** around the current iteration.

It may consider:

* the current problem state,
* available information,
* possible actions,
* expected outcome,
* technical constraints,
* and the best instruction for the current iteration.

### Counselor

The Counselor performs **process-level optimization**.

It may consider:

* accumulated results,
* previous unsuccessful approaches,
* objective progress,
* actual process costs,
* estimated next-step costs,
* expected additional value,
* diminishing returns,
* resource consumption,
* and whether further iterations remain justified.

Thus:

```text
Planner:
    Optimize the next step.

Counselor:
    Optimize the continuation of the process.
```

---

## 4. Neither Module Controls the Other

The Planner does not determine whether another iteration must occur.

The Counselor does not determine what the next instruction must be.

The Counselor may request another iteration.

The Planner may then define the instruction for that iteration.

The resulting instruction remains subject to the normal authorization process.

Likewise, the Planner may produce a highly promising instruction, while the Counselor may determine that requesting another iteration is not justified.

Neither component can unilaterally impose its preference on the other.

---

## 5. Competence Does Not Imply Authority

A highly capable Planner does not acquire authorization authority through its competence.

A highly capable Counselor does not acquire authorization authority through its process knowledge.

In particular:

```text
Competence ≠ Authority
```

and:

```text
Optimization ≠ Authorization
```

The architecture intentionally prevents the system from deriving security authority from the perceived intelligence, confidence, usefulness, or sophistication of a component.

---

## 6. The Role of the Core

The Core provides the authoritative boundary between these peer optimization functions.

The Core does not need to determine which module is more intelligent.

It determines whether the requested state transition is permitted under the applicable authorization and process rules.

The resulting separation is:

```text
Planner
    │
    │ defines next instruction
    ▼
Decisioner
    │
    │ evaluates authorization
    ▼
Core
    │
    │ enforces authorized state transition
    ▼
Effect


Counselor
    │
    │ evaluates process continuation
    ├── NEXT_ITERATION_REQUEST
    └── BREAK_REQUEST
             │
             ▼
            Core
```

The Core therefore does not replace the intelligence of either module.

It constrains what either module is allowed to cause.

---

## 7. Complementary Competence

The architecture deliberately allows the Planner and Counselor to be independently sophisticated.

A Planner may be exceptionally capable at solving the immediate technical problem.

A Counselor may be exceptionally capable at recognizing when solving that problem further is no longer worthwhile.

The system benefits from both capabilities.

The architecture therefore does not assume:

> "More reasoning is always better."

Nor does it assume:

> "Stopping early is always better."

Instead, the Counselor evaluates the continuation of the process using the information available to it, including actual and estimated process costs and the value of available or expected results.

---

## 8. Role Definition for AI Development

This separation provides a practical role definition for AI system development:

| Module         | Primary question                                    | Optimization horizon           |
| -------------- | --------------------------------------------------- | ------------------------------ |
| **Planner**    | What should be done next?                           | Current iteration              |
| **Counselor**  | Should and how should the process continue?         | Entire process                 |
| **Decisioner** | Is the requested operation authorized?              | Current authorization decision |
| **Core**       | What state transition is permitted and enforceable? | Authoritative system state     |

These roles are orthogonal.

A system may use different models, architectures, capabilities, or optimization methods for each role.

The roles remain defined by **function and authority**, not by implementation technology.

---

## 9. Fundamental Principle

The architecture can therefore be summarized as:

> **The Planner optimizes the next instruction. The Counselor optimizes the process trajectory. The Decisioner evaluates authorization. The Core enforces the authorized state transition.**

Or, in compact form:

```text
Planner     → WHAT NEXT?
Counselor   → CONTINUE OR BREAK?
Decisioner  → AUTHORIZED?
Core        → WHAT STATE TRANSITION IS PERMITTED?
```

This separation permits highly capable AI modules to operate as peers while preventing competence in one optimization domain from becoming implicit authority in another.
