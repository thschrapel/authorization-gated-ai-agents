# Rule 0 — Core-Encapsulated Information Flow

Every information flow between components is encapsulated by the Security Core.

No component is assumed to communicate directly with another component, a protected Effect, or an external interface without Core-controlled mediation.

## 0.1 The Universal Transition Pattern

Every controlled transition follows the same basic pattern:

```text
Source
   │
   │ OUTPUT
   ▼
Security Core
   │
   │ INPUT / EFFECT
   ▼
Target
```

The Security Core independently evaluates the source-side output and the target-side reception or Effect.

The corresponding outcomes are:

| Transition            | Source-side outcome            | Target-side outcome            |
| --------------------- | ------------------------------ | ------------------------------ |
| Component → Component | `OUTPUT_ALLOW` / `OUTPUT_DENY` | `INPUT_ALLOW` / `INPUT_DENY`   |
| Component → Effect    | `OUTPUT_ALLOW` / `OUTPUT_DENY` | `EFFECT_ALLOW` / `EFFECT_DENY` |

Both outcomes are required for the transition to occur.

`OUTPUT_ALLOW` therefore never implies either `INPUT_ALLOW` or `EFFECT_ALLOW`.

Likewise, target-side authorization does not imply authorization of the source-side output.

## 0.2 Boundary-Specific DENY

`DENY` always stops the **specific boundary** to which the outcome applies.

```text
OUTPUT_DENY
    → the source output does not pass the output boundary

INPUT_DENY
    → the receiving component does not receive the input

EFFECT_DENY
    → the protected Effect does not cross the Effect boundary
```

A `DENY` therefore does not represent an abstract or global prohibition.

It is an authoritative instruction that the corresponding transition must not occur.

For a component-to-component transition, both boundaries must be passed:

```text
Source
   │
   │ OUTPUT
   ▼
Security Core
   │
   │ INPUT
   ▼
Target
```

Consequently:

* `OUTPUT_DENY` prevents the source output from proceeding into the transition.
* `INPUT_DENY` prevents the receiving component from receiving the information.
* only `OUTPUT_ALLOW` **and** `INPUT_ALLOW` together permit the complete transition.

The same principle applies to Effects: `OUTPUT_ALLOW` alone cannot produce an Effect; `EFFECT_ALLOW` is additionally required.

## 0.3 Output-to-Input Semantics

A component-to-component transition consists of two independently authorized boundaries.

The first authorization concerns the output of the producing component.

The second authorization concerns the input to the receiving component.

Consequently:

```text
OUTPUT_ALLOW ≠ INPUT_ALLOW
```

`OUTPUT_ALLOW` permits the output to proceed to the Security Core for the specified transition.

`INPUT_ALLOW` permits the receiving component to receive that information in the specified context.

If either authorization is denied, the complete component-to-component transition does not occur.

## 0.4 Effect Semantics

A protected Effect uses the same transition model.

The only difference is that the target-side authorization is an `EFFECT` authorization rather than an `INPUT` authorization.

An authorized component output therefore does not itself authorize an Effect.

The Effect requires its own explicit Core authorization.

## 0.5 Compact Architectural Notation

A diagram such as:

```text
A ──► B ──► C ──► Effect
```

is shorthand for a sequence of Core-mediated transitions.

The Security Core is therefore implicit between every pair of nodes.

The compact notation hides the repeated Core boundaries; it does not remove them.

## 0.6 Context-Dependent Evaluation

Every input, output, and Effect is evaluated in its specific context.

The same information may receive different authorization outcomes depending on:

* the component receiving or producing it,
* the intended operation,
* the requested Effect,
* the current authorization state,
* the applicable security rules,
* and the relevant execution context.

Authorization is consequently not merely a property of the information itself.

It is a property of the evaluated transition and its context.

## 0.7 No Implicit Trust Between Components

A component does not acquire permission to receive or transmit information merely because another component produced it.

Likewise, successful processing by one component does not authorize its output for another component or for an Effect.

Every transition remains subject to Core evaluation.

This applies equally to:

* Planner,
* Counselor,
* Decisioner,
* Security Counsel,
* Anonymizer,
* Security Anonymizer,
* Translator,
* external AI systems,
* and other future components.

## 0.8 Architectural Invariant

The fundamental invariant is:

> **Every component input, component output, and protected Effect is subject to explicit, context-specific Security Core authorization, and every `DENY` stops the corresponding boundary.**

Thus the architecture does not define direct component-to-component communication.

It defines **Core-mediated transitions**.

This allows complex architectures to be represented concisely while preserving the central architectural property:

> **Components provide processing and intelligence; the Security Core controls the transitions between them and the resulting Effects.**
