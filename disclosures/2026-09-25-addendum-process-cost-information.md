# Counselor Addendum — Process Cost Information

## 1. Process Costs as Counselor Input

For meaningful process optimization, the Counselor requires information about the resources consumed by the process.

The Counselor therefore receives process-cost information as part of the process context.

At minimum, this information distinguishes between:

* **actual process costs** incurred so far, and
* **estimated costs** expected for the next iteration or process step.

The purpose of this information is to allow the Counselor to evaluate expected benefit against expected additional expenditure.

---

## 2. Actual Process Costs

Actual process costs describe resources already consumed by the process.

Depending on the system, these may include:

* computation,
* execution time,
* energy,
* external service usage,
* financial cost,
* consumed quotas,
* occupied resources,
* or other Core-defined measurable resources.

Actual costs are historical process information.

They describe what has already been consumed and must not be represented as a prediction.

The Core is responsible for establishing the authoritative process-cost record.

---

## 3. Estimated Cost of the Next Step

The Counselor should also receive an estimate of the expected cost of the next process step.

This estimate may include:

* expected computation,
* expected execution time,
* expected energy consumption,
* expected external resource consumption,
* expected financial cost,
* expected quota consumption,
* or other Core-defined cost dimensions.

The estimate describes expected expenditure, not a guarantee.

The estimate may therefore be accompanied by uncertainty or a defined estimation range.

For example:

```text
NEXT_STEP_COST_ESTIMATE {
    computation
    time
    energy
    financial_cost
    external_resources
    uncertainty
}
```

The exact cost dimensions are implementation-dependent and may be extended by the Core-defined protocol.

---

## 4. Actual Cost and Expected Cost Must Remain Distinct

The following distinction is fundamental:

```text
ACTUAL_COST
    = resources already consumed

ESTIMATED_NEXT_COST
    = resources expected to be consumed by the next step
```

Neither value replaces the other.

A process may already have consumed substantial resources while the next step is inexpensive.

Conversely, a process may have consumed very little while the next step is expected to be disproportionately expensive.

The Counselor therefore evaluates both accumulated expenditure and expected additional expenditure.

---

## 5. Cost-Benefit Assessment

The Counselor may combine process-cost information with its assessment of expected progress.

Conceptually:

```text
EXPECTED_VALUE_OF_NEXT_STEP
        versus
EXPECTED_COST_OF_NEXT_STEP
```

The resulting assessment may influence whether the Counselor:

* requests another iteration,
* requests a break,
* presents a partial result,
* or continues seeking a better result.

This assessment remains a Counselor process decision.

It does not constitute authorization.

In particular:

```text
HIGH_EXPECTED_VALUE ≠ AUTHORIZATION
LOW_EXPECTED_VALUE ≠ DENIAL
```

Authorization remains exclusively governed by the existing authorization architecture.

---

## 6. Cost Information Must Not Become a Hidden Control Channel

Process-cost information is an input to the Counselor's optimization function.

It must not become an implicit authorization mechanism.

The Counselor must not be able to establish authorization by asserting:

> "The next step is cheap."

Likewise, the Counselor must not be able to establish a prohibition merely by asserting:

> "The next step is expensive."

Cost information informs the process decision.

The Core remains responsible for the actual authorization and enforcement decision.

---

## 7. Cost Attribution

Where possible, process costs should be attributable to a defined process entity.

At minimum, costs should be associable with:

* `process_id`,
* `iteration_id`,
* and, where applicable, `instruction_id` and `effect_id`.

This permits the system to distinguish:

```text
PROCESS_COST
    → ITERATION_COST
        → INSTRUCTION_COST
            → EFFECT_COST
```

Such attribution also permits comparison between estimated and actual costs.

---

## 8. Estimation Feedback

After completion of an iteration, the actual cost can be compared with the previously estimated cost.

This creates an additional measurable property of the process:

```text
ESTIMATED_NEXT_STEP_COST
            ↓
        EXECUTION
            ↓
        ACTUAL_COST
            ↓
     ESTIMATION ERROR
```

The resulting information may be made available to the Counselor for subsequent optimization.

This permits the Counselor to improve its process decisions without granting it authority to modify the underlying authorization rules.

---

## 9. Counselor Quality and Resource Efficiency

Counselor quality can therefore include not only whether useful results are achieved, but also whether the process uses resources proportionately to the value obtained.

A Counselor that repeatedly requests expensive iterations with little expected additional value may exhibit poor resource optimization.

A Counselor that terminates whenever an iteration is costly may likewise fail to pursue valuable results.

The relevant objective is therefore not:

```text
MINIMIZE COST
```

and not:

```text
MAXIMIZE ITERATIONS
```

but rather:

```text
MAXIMIZE EXPECTED VALUE
SUBJECT TO
AUTHORIZATION AND AVAILABLE RESOURCES
```

The Counselor's ability to make this trade-off visible and auditable is part of its process quality.

---

## 10. Core Principle

The Counselor should therefore receive at least two distinct classes of cost information:

> **What has this process already cost?**

and

> **What is the next step expected to cost?**

Together with the expected value of continued work, these inputs allow the Counselor to make an informed `NEXT_ITERATION_REQUEST` or `BREAK_REQUEST`.

The Counselor optimizes the continuation of the process.

The Core controls whether that continuation is authorized and permitted to occur.
