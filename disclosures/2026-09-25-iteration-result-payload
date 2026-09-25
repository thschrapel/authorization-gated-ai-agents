## 13.5a Exact `ITERATION_RESULT` Payload

The `ITERATION_RESULT` is a Core-generated response containing the authoritative result of the immediately preceding iteration.

The payload has the following defined structure:

```text
ITERATION_RESULT {
    message_type
    process_id
    objective_id
    iteration_id
    instruction_id
    effect_id
    result_status
    result
    timestamp
}
```

### 13.5a.1 `message_type`

Fixed value:

```text
message_type: ITERATION_RESULT
```

This identifies the message as a Core Response and not as a Process Request.

---

### 13.5a.2 `process_id`

Identifies the overall process to which the iteration belongs.

```text
process_id: <unique process identifier>
```

All iterations belonging to the same process use the same `process_id`.

---

### 13.5a.3 `objective_id`

Identifies the authorized Objective under which the iteration was performed.

```text
objective_id: <unique objective identifier>
```

This binds the result to the Objective without asserting that the Objective has been achieved.

---

### 13.5a.4 `iteration_id`

Identifies the completed iteration.

```text
iteration_id: <unique iteration identifier>
```

The `iteration_id` is assigned by the Core when the iteration is opened.

---

### 13.5a.5 `instruction_id`

Identifies the specific `ITERATION_INSTRUCTION` executed during this iteration.

```text
instruction_id: <unique instruction identifier>
```

This establishes the relationship:

```text
ITERATION_OPEN
      ↓
ITERATION_INSTRUCTION
      ↓
instruction_id
      ↓
EFFECT
      ↓
ITERATION_RESULT
```

---

### 13.5a.6 `effect_id`

Identifies the concrete effect execution that produced the result.

```text
effect_id: <unique effect identifier>
```

Where an iteration produces no external effect, the field may contain the protocol-defined null value.

The absence of an effect must not be interpreted as a successful effect.

---

### 13.5a.7 `result_status`

Describes the technical status of the result.

The status is determined by the Core according to the effect and protocol semantics.

The permitted values are:

```text
SUCCESS
FAILURE
PARTIAL
NO_RESULT
```

These values describe the result of the iteration.

They do **not** mean:

```text
SUCCESS     = Objective achieved
FAILURE     = Objective impossible
PARTIAL     = another iteration required
```

Goal achievement remains a separate Counselor/Core query.

---

### 13.5a.8 `result`

Contains the actual protocol result produced by the executed effect and processed by the Core.

```text
result: <protocol-defined result data>
```

The representation of `result` must conform to the Core-defined protocol language and encoding rules.

The result is therefore not an uncontrolled raw communication channel from the effect to the Counselor.

The Core is the protocol boundary through which the result enters the process context.

---

### 13.5a.9 `timestamp`

Records the Core-defined timestamp associated with creation of the `ITERATION_RESULT`.

```text
timestamp: <Core timestamp>
```

The Core is authoritative for this timestamp.

---

## 13.5b Result Semantics

The `ITERATION_RESULT` answers one specific question:

> **What was the result of the last completed iteration?**

It does not answer:

> **Has the Objective been achieved?**

That question is handled separately through:

```text
GOAL_STATUS_REQUEST
        ↓
GOAL_STATUS_RESPONSE
```

Likewise, `ITERATION_RESULT` does not answer:

> **Should another iteration be performed?**

That decision belongs to the Counselor.

The separation is therefore:

```text
ITERATION_RESULT
    → What happened?

GOAL_STATUS_RESPONSE
    → Is the Objective achieved?

NEXT_ITERATION_REQUEST
    → Should another iteration be requested?
```

---

## 13.5c Result Handoff to the Next Iteration

When the Counselor requests another iteration, the immediately preceding `ITERATION_RESULT` becomes explicit input to the new Process Request.

The relevant structure is:

```text
NEXT_ITERATION_REQUEST {
    message_type
    process_id
    objective_id
    previous_iteration_id
    previous_iteration_result
    timestamp
}
```

where:

```text
previous_iteration_result
    =
    the complete ITERATION_RESULT payload
    of the immediately preceding iteration
```

Conceptually:

```text
ITERATION_RESULT(N)
        │
        ▼
    Counselor
        │
        │ NEXT_ITERATION_REQUEST
        │
        │ previous_iteration_result = ITERATION_RESULT(N)
        ▼
      Core
        │
        ▼
ITERATION_OPEN(N+1)
```

This makes the handoff between iterations explicit.

The Planner does not need to receive the previous effect output through a hidden or direct channel.

After the Core opens the next iteration, the Planner receives only the context authorized for its role.

---

## 13.5d Core Authority over the Result

The `ITERATION_RESULT` is created by the Core.

Therefore:

```text
Planner
    ≠
authoritative result source

Effect
    ≠
protocol result source

Counselor
    ≠
result authority
```

The authoritative process result is:

```text
Effect output
      ↓
Core processing
      ↓
ITERATION_RESULT
```

The Core does not delegate the creation of the authoritative `ITERATION_RESULT` to the Planner or Counselor.

---

## 13.5e Minimal Example

A concrete result may therefore look conceptually like:

```text
message_type: ITERATION_RESULT
process_id: P-1042
objective_id: O-77
iteration_id: I-03
instruction_id: INSTR-03
effect_id: EFFECT-03
result_status: SUCCESS
result: <Core-defined result data>
timestamp: <Core timestamp>
```

The Counselor may subsequently submit:

```text
message_type: NEXT_ITERATION_REQUEST
process_id: P-1042
objective_id: O-77
previous_iteration_id: I-03
previous_iteration_result:
    message_type: ITERATION_RESULT
    process_id: P-1042
    objective_id: O-77
    iteration_id: I-03
    instruction_id: INSTR-03
    effect_id: EFFECT-03
    result_status: SUCCESS
    result: <Core-defined result data>
    timestamp: <Core timestamp>
timestamp: <Core timestamp>
```

The Core then evaluates whether the next iteration may be opened.

The important property is:

> **The Counselor decides whether to request another iteration; the Core supplies the authoritative result of the previous iteration and determines whether the requested next iteration may be opened.**
