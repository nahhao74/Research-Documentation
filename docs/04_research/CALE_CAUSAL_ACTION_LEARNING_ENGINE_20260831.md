# CALE — Causal Action Learning Engine

**Date:** 2026-08-31  
**Status:** research hypothesis / theory-design track  
**Scope:** Moving-Mode AURA–WISE–World Model–AEGIS vNext

CALE is a causal-learning architecture, not a flight controller and not a replacement for CTEE, AURA, FAST/T1/C1, WISE, AEGIS or PX4.

Current authority is unchanged:

```text
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false
OPTION_B_CONTRACT_READY=false
FRESH_SCIENCE=BLOCKED
production_authority=false
```

## 1. Why CALE replaces CEORP as the primary research abstraction

Claim-level prior-art review found strong precedent for orthonormal-basis observers, Laguerre/Kautz predictors, adaptive RLS, set-membership/event-triggered updates, concurrent-learning informative sample selection, closed-loop probing, dynamic treatment-effect state-space models and lightweight UAV residual prediction.

Therefore CEORP is retained only as a possible numerical backend:

```text
CEORP-v3 ≈ CALE-OBF
```

The research contribution, if one exists, must sit above the backend.

## 2. Causal object identities

CALE must preserve:

```text
Z           randomized assignment
U_requested requested bounded candidate
U_A         actual accepted action
T_D         causal decision frontier
T_A         actual accepted-action frontier
Y_h         future closed-loop outcome
```

These are distinct causal objects.

```text
Z
 ↓
U_requested
 ↓
AEGIS / projection / bounds / freshness / authority
 ↓
U_A at T_A
 ↓
closed-loop response Y_h
```

## 3. Pre-treatment context

A compact context state may use an OBF or another fixed-order representation:

\[
z^B_{k+1}=A_\phi z^B_k+B_\phi\nu^B_k
\]

but must satisfy:

\[
z^B(T_D)\in\mathcal F_{T_D^-}.
\]

The pre-treatment state must not read future accepted action, T_A, future ACK, future outcome or post-treatment response.

## 4. Minimal action-effect head

Initial reference:

\[
\psi(U)=
[z^B(T_D), X(T_D), U, z^B(T_D)\otimes U]^\top
\]

\[
\hat G_h=\Theta_h^\top\psi(U).
\]

Target:

\[
G^B_{action}(X,U,h)=Y(B+U,h)-Y(B+ZERO,h)
\]

with active baseline:

```text
B = PX4 + AURA + FAST/T1/C1
```

## 5. Causal Learning Admission Gate

A transaction may teach the model only when all required validity classes pass:

```text
pre-treatment validity
treatment identity / accepted exposure validity
target validity
source/provenance validity
```

Define:

\[
q^{causal}=q^{pre}q^{treatment}q^{target}q^{provenance}.
\]

If any required predicate is FAIL or UNKNOWN:

\[
q^{causal}=0
\]

and parameter state must not change:

\[
\Theta_{k+1}=\Theta_k.
\]

## 6. Informativeness gate

Only after causal admission succeeds:

\[
q^{learn}=q^{causal}q^{info}.
\]

Possible informativeness criteria include innovation magnitude, feature novelty, minimum singular-value improvement, condition-number improvement or context-coverage improvement.

Mandatory order:

```text
CAUSAL AUTHORITY
→ INFORMATION VALUE
→ UPDATE
```

not:

```text
interesting sample
→ update
```

## 7. Assigned versus accepted-action effects

CALE must distinguish:

\[
G_Z(X,Z,h)
\]

from:

\[
G_U(X,U_A,h).
\]

The first is an assignment/ITT-style effect. The second is a realized accepted-action effect.

Naive regression on U_A may be biased because AEGIS acceptance/projection can depend on system state.

## 8. Randomization-as-IV branch

Research hypothesis:

```text
randomized Z
→ U_requested
→ AEGIS mediation
→ U_A
→ Y
```

Evaluate whether Z can serve as an instrument for U_A.

Required assumptions must be explicit and tested/argued:

```text
relevance
exclusion restriction
monotonicity or replacement structural assumptions
arm-neutral availability
projection/saturation semantics
context conditioning
effect heterogeneity
```

No IV method is authorized automatically.

## 9. Theory targets

### T1 — pre-treatment non-leakage

Prove/mechanically establish that z_B(T_D) cannot depend on future treatment information.

### T2 — invalid-transaction non-update

Prove by implementation contract that q_causal=0 implies exact parameter-state invariance.

### T3 — bounded adaptation

For bounded features/residuals and projected parameter set Omega, establish bounded updates.

### T4 — causal identification

Derive the assumptions under which admitted randomized transactions identify assigned-action and/or instrumented accepted-action effects.

## 10. Backend ladder

```text
CALE-RIDGE
CALE-RLS
CALE-OBF        # CEORP-v3 implementation candidate
CALE-DMDc
CALE-SINDY
CALE-KOOPMAN
CALE-TINY-MLP   # later only
```

The backend must earn its place on accuracy/latency/complexity/runtime evidence.

## 11. Embedded target

First implementation should prefer:

```text
fixed-order state
fixed iteration count
no dynamic allocation on serving path
no online backprop
no large matrix inverse on FAST path
no iterative estimator optimizer
small bounded adaptation path
CPU first
SIMD/HVX/fixed-point only if useful
```

Target compute class: QCS8550 onboard. Desired condition:

```text
predictor compute << transport/scheduling latency
```

## 12. Benchmark

Compare:

```text
A frozen model / no adaptation
B ordinary error-triggered update
C informative-only concurrent-learning style update
D CALE causal-admission + informativeness update
```

Measure scientific correctness, H20/H40/H80 action-response prediction, session/context generalization, CPU/p99 latency/memory, state/buffer/branch count and forensic complexity.

## 13. Current novelty status

```text
CALE_STATUS=RESEARCH_HYPOTHESIS
CALE_PUBLICATION_NOVELTY=PLAUSIBLE_NOT_PROVEN
CALE_EXACT_PRIOR_ART_FOUND=false
CALE_COMPONENT_PRIOR_ART=VERY_STRONG
CALE_SYSTEMATIC_REVIEW_REQUIRED=true
PATENT_CLEARANCE=NOT_ESTABLISHED

CEORP_STATUS=BACKEND_CANDIDATE
CEORP_ROLE=CALE_OBF_IMPLEMENTATION_OPTION
```

The current research priority is:

```text
causal estimand / treatment identity
→ causal learning-admission invariants
→ minimal backend
→ higher capacity only if required
```
