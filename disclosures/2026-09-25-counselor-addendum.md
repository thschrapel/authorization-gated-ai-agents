# Counselor Addendum — Break, Resource Efficiency and Partial Results

## 1. Break as an Optimization Decision

A `BREAK_REQUEST` is not merely a request to stop the process.

Behind a break may be a complex optimization problem involving, among other factors:

* expected value of further iterations,
* expected information gain,
* probability of meaningful progress,
* computational cost,
* time,
* energy consumption,
* resource consumption,
* diminishing returns,
* risk of producing irrelevant or misleading results,
* quality of already obtained results,
* and the possibility that further authorized effort will no longer materially improve the outcome.

The Counselor may evaluate these factors when determining whether continued iteration is useful.

The Counselor does not thereby acquire authority to terminate an iteration independently. It submits a `BREAK_REQUEST` to the Core.

The Core remains authoritative over the resulting process state.

---

## 2. Good Intent Does Not Imply Useful Continuation

A process may remain fully authorized while additional iterations nevertheless become inefficient.

An iteration can be:

* technically valid,
* authorized,
* well-intentioned,
* and correctly executed,

while still having insufficient expected value to justify further effort.

Therefore:

> Authorization to continue does not imply that continuation is optimal.

The Counselor may recognize that continued effort would constitute resource consumption without sufficient expected benefit.

This distinction is important because optimization is not limited to achieving the Objective. It also includes avoiding unnecessary expenditure in pursuing it.

---

## 3. Prevention of Waste

The Counselor has a process-level responsibility to consider whether further iterations are justified by their expected contribution.

Waste prevention may include recognizing situations in which:

* repeated iterations produce substantially the same result,
* new information is unlikely to change the outcome,
* the remaining uncertainty cannot reasonably be reduced,
* additional computation has diminishing expected value,
* the available resources are disproportionate to the expected improvement,
* or continued execution would consume resources without a correspondingly meaningful increase in result quality.

Waste prevention does not mean that the Counselor may arbitrarily stop difficult work.

Rather, the Counselor should be capable of distinguishing:

`DIFFICULT BUT PRODUCTIVE`

from

`CONTINUED EFFORT WITH INSUFFICIENT EXPECTED VALUE`.

---

## 4. Partial Results

A process does not necessarily have to produce a complete achievement of the Objective to produce useful output.

The Counselor may therefore cause partial results to be presented when the available result has independent value, even if the complete Objective has not been achieved.

A partial result may include:

* completed sub-results,
* verified intermediate findings,
* identified constraints,
* unresolved questions,
* failed approaches,
* remaining work,
* or a bounded statement of what could and could not be established.

A partial result must not be represented as complete achievement of the Objective.

The distinction is therefore:

`RESULT ≠ OBJECTIVE ACHIEVEMENT`

and:

`PARTIAL RESULT ≠ FAILED PROCESS`

A process may terminate with a useful partial result.

---

## 5. Justified Abortion

A `BREAK_REQUEST` may contain a structured justification.

The justification may describe why further iteration is not expected to provide sufficient additional value relative to its cost.

Possible justification categories include:

* diminishing returns,
* insufficient expected information gain,
* repeated unsuccessful approaches,
* resource exhaustion,
* time constraint,
* unresolved dependency,
* unavailable required information,
* risk of degrading result quality,
* or another Core-defined termination reason.

The justification explains the Counselor's process assessment.

It does not itself authorize termination.

The Core evaluates the request and establishes the resulting process state.

---

## 6. Counselor Quality

The quality of a Counselor cannot be measured solely by the amount of work it causes to be performed.

A Counselor that continuously requests additional iterations may consume substantial resources while producing little additional value.

Conversely, a Counselor that terminates a process prematurely may fail to realize substantial available value.

Counselor quality therefore includes the ability to determine when:

* another iteration is likely to provide meaningful value,
* another iteration is unlikely to provide meaningful value,
* a partial result is already sufficiently valuable to present,
* or further effort should be requested despite its cost.

This creates an important optimization dimension:

> The Counselor must optimize not only for progress, but also against unnecessary effort.

The ability to prevent waste can therefore serve as an observable property of Counselor quality.

---

## 7. No Expansion of Authority

Resource optimization does not expand authorization.

The Counselor may decide that additional work appears useful, but:

`USEFUL ≠ AUTHORIZED`

Likewise:

`INEFFICIENT ≠ UNAUTHORIZED`

The Counselor may request another iteration only through the defined process protocol.

The Core remains the sole authority for:

* opening an iteration,
* enforcing authorization,
* applying iteration constraints,
* accepting or rejecting a break,
* and establishing terminal process states.

The Counselor optimizes **within** the authorization boundary.

It does not redefine that boundary.

---

## 8. Fundamental Principle

The Counselor's role can therefore be summarized as:

> **The Counselor seeks to complete the authorized Objective with meaningful results while minimizing unnecessary iterations and resource expenditure.**

This includes the ability to recognize when continuation is no longer justified, to request a break with an explicit rationale, and to preserve and present valuable partial results.

The ability to stop is therefore not merely a termination capability.

It is part of the Counselor's optimization problem.
