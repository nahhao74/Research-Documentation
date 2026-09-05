# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap

**Canonical active roadmap version:** `v6 — CALE causal-learning formalization / CEORP backend demotion — active direction`  
**Scope:** Moving Mode only  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9`

This document defines **future development direction**, not current execution permission.

Current runtime/scientific authority:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

Detailed CALE note:

[`CALE_CAUSAL_ACTION_LEARNING_ENGINE_20260831.md`](CALE_CAUSAL_ACTION_LEARNING_ENGINE_20260831.md)

---

# 1. Current execution boundary

As of 2026-09-05, Phase-0 Option-B and the known status-observer/C1/E8 pairing infrastructure blockers have bounded qualification, but no randomized root has completed the required 96/96 scientific matrix.

Current state:

```text
PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID

OPTION_B_CONTRACT_READY=true
OPTION_B_LIVE_RUNTIME_QUALIFIED=true
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US

WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED
STATUS_OBSERVER_SOURCE_FRONTIER_REPAIR=CLOSED_QUALIFIED
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
POST_RESET_E8_SOURCE_CAUSAL_HANDOFF_QUALIFICATION=VALID_NONSCIENTIFIC

FRESH_RANDOMIZED_G_ACTION_PILOT=INCOMPLETE_INVALID_INFRASTRUCTURE_ROOT
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

Latest root `fresh_33` stopped fail-closed because block 3 tried to arm while block 2's native event was still active.

Current next gate:

```text
native-event lifecycle ownership audit
→ prove inter-block CLEAR wait is implementation-preserving
→ canonical CLEAR/retirement readiness repair
→ deterministic regression
→ bounded non-scientific consecutive-event qualification
→ reverse processing + peeling
→ owner review
→ only then a new complete randomized pilot
```

No CALE/CTEE-F/CIBES/model-capacity work unblocks this mechanical lifecycle gate.

---

# 2. Phase-0 completion sequence

The remaining Phase-0 sequence is:

```text
P0-INFRA
native-event CLEAR/retirement lifecycle closure
        ↓
P0-NOSCIENCE
bounded consecutive-event qualification
        ↓
P0-OWNER
fresh-pilot authorization
        ↓
P0-SCIENCE
new immutable 8-session / 96-block randomized G_action root
        ↓
P0-ADMISSION
causal dataset acceptance
        ↓
P0-ID
G_action identification
```

A failed or partial root remains immutable infrastructure/scientific evidence and is never pooled to manufacture a complete dataset.

World-Model action-response training remains blocked until causal-dataset acceptance.

---

# 3. Research priority after Phase-0

The research priority is:

```text
causal estimand and exact treatment identity
>
causal learning-admission invariants
>
minimal numerical backend
>
higher model capacity
```

The project should not introduce a more complex predictor before causal data admission is trustworthy.

---

# 4. CTEE / CTEE-F / CALE / WISE / AEGIS responsibility split

```text
CTEE
= may this event/action occur now?

CTEE-F
= if admissible now, will the information/plan remain fresh when consumed?

CALE
= may this completed transaction teach the action-response model?

WISE
= which admissible candidate should be selected?

AEGIS
= what bounded action may actually reach PX4?

PX4
= authoritative inner-loop execution
```

These mechanisms must remain conceptually separate.

---

# 5. CALE — Causal Action Learning Engine

CALE is the preferred causal-learning abstraction after accepted randomized data exists.

Its graph preserves exact identities:

```text
pre-treatment context X(T_D), z_B(T_D)
        ↓
randomized assignment Z
        ↓
requested action U_requested
        ↓
AEGIS bounds / projection / freshness / generation / authority
        ↓
accepted action U_A at T_A
        ↓
closed-loop response
FAST / PX4 / allocation reactions included
        ↓
future outcome Y(T_A+h)
```

Never collapse:

```text
Z
U_requested
U_A
T_D
T_A
```

because requested and accepted treatment can differ under rejection, projection, saturation, timing or authority mediation.

CALE remains a research hypothesis until its causal-admission rules and benefit are demonstrated.

---

# 6. Action-blind pre-treatment context

A future compact baseline/context state must satisfy:

```text
z_B(T_D) belongs only to information available before treatment
```

Forbidden inputs before the context state is frozen:

```text
future accepted action
future ACK
T_A
future outcome
post-treatment response
```

Treatment enters only after the pre-treatment context is fixed.

---

# 7. Minimal G_action backend first

Initial action-effect modeling should remain linear-in-parameters or otherwise minimal enough to audit.

A generic feature form is:

```text
psi(U) = [z_B(T_D), X(T_D), U, interactions]
G_hat_h = Theta_h^T psi(U)
```

Scientific target remains:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The first model should answer whether a compact action-conditioned representation works before escalating model capacity.

---

# 8. Causal Learning Admission Gate

A completed transaction is not allowed to update an action-response model merely because its numerical error is large or its feature vector is informative.

Required ordering:

```text
causal/provenance validity
        ↓
numerical informativeness
        ↓
parameter update
```

Conceptually:

```text
q_causal = q_pre * q_treatment * q_target * q_provenance
q_learn  = q_causal * q_info
```

If any mandatory causal predicate is FAIL or UNKNOWN:

```text
q_causal = 0
model parameters do not update
```

This should become a machine-checkable learning invariant.

---

# 9. Assignment effect vs accepted-action effect

Future work must preserve two possible targets:

```text
G_Z(X,Z,h)
= effect of randomized assignment under the frozen policy

G_U(X,U_A,h)
= response to action actually accepted by the execution boundary
```

They are not automatically the same estimand.

Naive conditioning on realized accepted action may destroy randomized identification when acceptance depends on state.

The frozen assigned-arm/ITT-style analysis remains the scientific reference unless a later contract explicitly changes it.

---

# 10. Randomization-as-IV research branch

Potential graph:

```text
Z randomized
    ↓
U_requested
    ↓
AEGIS / projection / constraints
    ↓
U_A
    ↓
Y
```

Research question:

> Can randomized assignment `Z` serve as an instrument for accepted action `U_A` when estimating realized closed-loop action response?

Required assumptions must be explicit, including relevance, exclusion restrictions, context dependence, projection/saturation behavior and any monotonicity/structural assumptions.

No IV estimator is authorized merely because assignment is randomized.

---

# 11. CALE backend ladder

CALE remains representation-agnostic:

```text
CALE-RIDGE
small offline/online linear reference

CALE-RLS
recursive linear-in-parameters

CALE-OBF
Laguerre/Kautz orthonormal backend
CEORP maps here as an implementation option

CALE-DMDc
compact online dynamics challenger

CALE-SINDY
sparse equation challenger

CALE-KOOPMAN
small lifted model

CALE-TINY-MLP
only if structured models are insufficient
```

Representation choice comes after causal correctness.

---

# 12. Embedded/runtime target

Preferred first adaptive implementation characteristics:

```text
fixed-order state
fixed fast-path iteration count
no dynamic allocation on FAST path
no online backpropagation
no large matrix inversion on FAST path
small bounded adaptation path
CPU implementation first
```

Target deployment class:

```text
QCS8550 onboard compute
```

Desired engineering condition:

```text
predictor compute << transport/scheduling latency
```

Embedded execution by itself is not a novelty claim.

---

# 13. CALE benchmark / go-no-go

Compare:

```text
A — frozen offline model, no online adaptation
B — ordinary error-triggered RLS/update
C — informative-only update
D — CALE causal-admission + informativeness update
```

Scientific-correctness metrics:

```text
invalid transaction update count
cross-generation update count
post-treatment leakage
wrong-action-label rate
requested-vs-accepted mismatch
```

Prediction metrics:

```text
G_action H20/H40/H80 error
CALM/GUST generalization
session generalization
accepted-action prediction error
```

Runtime metrics:

```text
CPU/update
CPU/prediction
p50/p95/p99/max
memory
allocations
deadline misses
```

Retain CALE only if causal admission prevents a real invalid-learning failure class or materially improves robustness/generalization at acceptable runtime cost.

Current research state:

```text
CALE_STATUS=RESEARCH_HYPOTHESIS
CALE_PUBLICATION_NOVELTY=PLAUSIBLE_NOT_PROVEN
CALE_CURRENT_PHASE0_RUNTIME_DEPENDENCY=false
CALE_DOES_NOT_UNBLOCK_CURRENT_NATIVE_EVENT_LIFECYCLE_GATE=true

CEORP_STATUS=BACKEND_CANDIDATE
CEORP_ROLE=CALE_OBF_IMPLEMENTATION_OPTION
```

---

# 14. Other active research tracks

## CTEE

Live temporal/causal eligibility candidate. Compare against simpler timed-FSM/runtime-enforcement baselines; retain only if it provides a real additional causal/provenance guarantee.

## CTEE-F

Future freshness-aware extension:

```text
eligibility
+
Age of Information / age-at-application
+
remaining-delay/freshness envelope
+
atomic identity recheck
```

It is a future shadow/Phase-1+ hypothesis, not a current blocker repair.

## CIBES

Future constrained information-efficient experiment design after the current frozen Phase-0 science closes. It is not permitted inside the current randomized pilot because adaptive excitation would change the experiment.

## World-Model uncertainty ladder

```text
residual quantiles
→ heteroscedastic variance
→ ensemble/bootstrap
→ conformal error bounds
→ set-membership uncertainty
→ tractable outer approximation if justified
```

Retain advanced uncertainty only if it improves calibration/OOD behavior and downstream WISE/AEGIS utility over simpler baselines.

## AURA detector challengers

Run shadow bake-offs only after core pipeline scientific closure and latency instrumentation are stable enough to compare detector alternatives without changing authority implicitly.

## WISE planning

Start with bounded candidate enumeration/library/search. Escalate to heavier MPC/Koopman/RTI approaches only if measured planning benefit justifies runtime cost.

## AEGIS runtime assurance

Long-term direction includes explicit safety-envelope specification and potentially Lyapunov/CLF/CBF-style projection or other formal runtime-assurance mechanisms, but only after the action/prediction path is scientifically and operationally justified.

---

# 15. Long-term experiment sequence

```text
P0
native-event lifecycle closure
→ complete randomized G_action pilot
→ causal dataset acceptance
→ G_action identification

C0
CALE causal graph + estimand / IV formalization

C1
CALE-RIDGE baseline
causal-admission vs unrestricted-update ablation

C2
CALE backend benchmark
RLS / OBF / DMDc / SINDy / Koopman / tiny model

E1
end-to-end latency + FFT/FRF

E1B
AoI + age-at-application + remaining-delay envelope

E1C
CTEE-F shadow benchmark

E2
AURA detector shadow bake-off

E3/E4
F_nominal + G_action model ladder

E5/E6
history + T_D→T_A delay model

E7
uncertainty calibration

E8+
WISE / event-triggered planning / lightweight MPC

optional under a new experiment contract
CIBES constrained information-efficient excitation

then
low-dimensional adaptation
→ formal AEGIS safety/runtime assurance
```

---

# 16. Go/no-go principles

Every new algorithm should answer a measured problem.

```text
measure
→ identify a real bottleneck/failure class
→ introduce the smallest credible mechanism
→ compare against a simpler baseline
→ validate causality/timing/runtime cost
→ retain only if measurable benefit is real
```

Do not optimize for algorithm count or novelty language.

---

# 17. Hard authority boundaries

This roadmap does not authorize changes to:

```text
current G_action estimand
current randomized manifest
AURA/FAST/T1/C1 semantics
PX4 authority
Direct Guard
M_STABLE_US
W_MAX_US
T_D/T_A
H1000
SEALED
production authority
```

Any material change to those boundaries requires explicit owner review, a versioned scientific/control contract, implementation evidence and qualification.
