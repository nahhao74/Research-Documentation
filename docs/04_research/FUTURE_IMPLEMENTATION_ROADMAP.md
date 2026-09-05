# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap

**Canonical active roadmap version:** `v7 — Phase-0 fresh-pilot ready / FASTv2 residual-PI direction`  
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

As of 2026-09-05, Phase-0 Option-B and all currently known pre-pilot infrastructure blockers have bounded qualification:

```text
Option-B / Direct Guard
continuous-C1 replay/recovery
post-reset E8 source-causal pairing
native-event CLEAR/retirement lifecycle
next-status mirror/successor-frontier contract
WM reverse/Tarjan/peeling validity engine
```

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
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
NATIVE_EVENT_CLEAR_LIFECYCLE_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
PRE_RETRY_VALID_CAUSAL_CORE=true

LATEST_FAILED_SCIENTIFIC_ROOT=fresh_34
FRESH34_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

OWNER_FRESH_RANDOMIZED_PILOT_AUTHORIZED=true
NEW_FRESH_RANDOMIZED_PILOT_EXECUTED=false
NEXT_STATE=OWNER_AUTHORIZED_NEW_FRESH_RANDOMIZED_PILOT_READY_TO_EXECUTE
```

`fresh_34` failed in the first CALM row because valid strict-future native applied statuses were filtered out before E8 mirror publication. The canonical mirror now exposes native statuses before the existing health/authority gate, and a separate bounded qualification produced 689 strict-future successor lookups with zero timeout and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

The current next executable action is therefore the owner-authorized new immutable 8-session / 96-block randomized pilot, not another infrastructure qualification.

No CALE/CTEE-F/CIBES/FASTv2/model-capacity work is permitted to change the frozen baseline during this pilot.

---

# 2. Phase-0 completion sequence

The remaining Phase-0 sequence is now:

```text
P0-SCIENCE
owner-authorized new immutable 8-session / 96-block randomized G_action root
        ↓
P0-VALIDITY
canonical reverse index → graph → Tarjan → peeling
        ↓
P0-ADMISSION
separate causal dataset acceptance audit
        ↓
P0-ID
G_action identification
```

If the new root fails before 96/96 validity:

```text
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

No partial root is pooled or analyzed as treatment-effect evidence.

World-Model action-response training remains blocked until causal-dataset acceptance and separate owner authorization.

---

# 3. Research priority after Phase-0

The post-Phase-0 priority becomes:

```text
causal estimand + exact treatment identity
>
accepted causal dataset
>
minimal G_action identification
>
FAST/latency/tracking characterization
>
causal learning-admission invariants
>
minimal numerical backend
>
higher model/control capacity
```

The project should not introduce a more complex predictor or controller before a measured problem is identified and simpler baselines are established.

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

These mechanisms remain conceptually separate.

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

---

# 9. Assignment effect vs accepted-action effect

Future work must preserve:

```text
G_Z(X,Z,h)
= effect of randomized assignment under the frozen policy

G_U(X,U_A,h)
= response to action actually accepted by the execution boundary
```

They are not automatically the same estimand. The frozen assigned-arm/ITT-style analysis remains the scientific reference unless a later contract explicitly changes it.

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

No IV estimator is authorized merely because assignment is randomized.

---

# 11. CALE backend ladder

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
predictor/controller compute << transport/scheduling latency
```

---

# 13. CALE benchmark / go-no-go

Compare:

```text
A — frozen offline model, no online adaptation
B — ordinary error-triggered RLS/update
C — informative-only update
D — CALE causal-admission + informativeness update
```

Retain CALE only if causal admission prevents a real invalid-learning failure class or materially improves robustness/generalization at acceptable runtime cost.

Current research state:

```text
CALE_STATUS=RESEARCH_HYPOTHESIS
CALE_PUBLICATION_NOVELTY=PLAUSIBLE_NOT_PROVEN
CALE_CURRENT_PHASE0_RUNTIME_DEPENDENCY=false

CEORP_STATUS=BACKEND_CANDIDATE
CEORP_ROLE=CALE_OBF_IMPLEMENTATION_OPTION
```

---

# 14. FASTv2 — residual 2-DOF PI research track

The independent PID benchmark branch is a research source, not current main-pipeline authority. Its useful mechanisms should be reused selectively without changing the PX4 firmware PID.

Current strongest execution-feasible FASTv2 hypothesis:

```text
PX4 firmware PID and inner-loop authority unchanged
+
AURA disturbance feedforward (-d_hat)
+
T1/C1 temporal continuation
+
bounded residual 2-DOF PI at the already qualified acceleration-correction boundary
```

Conceptually:

```text
a_FASTv2 = -d_hat + a_T1/C1 + a_residual_2DOF_PI
```

The residual PI should use current realized tracking residuals, initially horizontal velocity error, and must be tuned against the **residual closed-loop plant with PX4+AURA+FAST/T1/C1 active**, not by copying numerical gains from the independent PID branch.

### Initial implementation ladder

```text
F0 — current -d_hat + T1/C1 baseline
F1 — F0 + bounded residual P
F2 — F0 + bounded residual PI
F3 — F0 + bounded residual 2-DOF PI
F4 — F3 + refined anti-windup only if saturation evidence requires it
F5A — F4 + synchronized actuator-aware INDI challenger
F5B — F4 + AURA/FAST only comparator
```

Only open combined INDI+AURA fast augmentation if repeat-supported ablation shows complementary benefit.

### PI tuning direction

Preferred process:

```text
identify residual accepted-acceleration → velocity plant
→ 2x2 N/E FRF and coupling audit
→ low-order FOPDT/IPDT reduction only if defensible
→ conservative analytic PI seed (for example SIMC-style)
→ Kp-only sweep
→ add Ki for sustained residual / low-frequency error
→ tune 2-DOF setpoint weight beta
→ anti-windup qualification
→ robust offline GA/NSGA-II refinement around the seeded region
→ plant-ensemble and delay/saturation validation
→ T1/C1/integrator overlap audit
```

Required safeguards:

```text
fixed robust gains first
explicit output bounds
conditional-integration anti-windup first
source/session/reset/lifecycle-aware integrator state
bandwidth below the main PX4 effective tracking loop
no gain scheduling until a measurable causal scheduling state z is supported
no ANN/RBF gain scheduler by default
```

Metrics must include not only tracking RMSE but also:

```text
T_effect
T_recover
peak deviation
ITAE
cross-track error
post-CLEAR overshoot/settling
control effort
command TV/jerk
saturation/headroom
phase/delay margins
PI vs FAST interaction
integral vs T1/C1 overlap
```

This FASTv2 direction is **post-Phase-0 research only**. Promotion would redefine baseline `B` and therefore requires a new versioned control/scientific contract.

---

# 15. Other active research tracks

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

## CIBES

Future constrained information-efficient experiment design after the frozen Phase-0 science closes. Adaptive excitation is not permitted inside the current randomized pilot.

## World-Model uncertainty ladder

```text
residual quantiles
→ heteroscedastic variance
→ ensemble/bootstrap
→ conformal error bounds
→ set-membership uncertainty
→ tractable outer approximation if justified
```

## AURA detector challengers

Run shadow bake-offs only after core pipeline scientific closure and latency instrumentation are stable enough for fair comparison.

## WISE planning

Start with bounded candidate enumeration/library/search. Escalate to heavier MPC/Koopman/RTI approaches only if measured planning benefit justifies runtime cost.

## AEGIS runtime assurance

Long-term direction includes explicit safety-envelope specification and potentially Lyapunov/CLF/CBF-style projection or other formal runtime-assurance mechanisms, after the action/prediction path is scientifically and operationally justified.

---

# 16. Long-term experiment sequence

```text
P0
new owner-authorized randomized G_action pilot
→ causal dataset acceptance
→ G_action identification

E0
fresh F0→F5 end-to-end latency / AoI instrumentation

F0
FASTv2 residual-plant identification
→ fixed residual P/PI/2DOF-PI ablation
→ anti-windup / T1C1 interaction audit
→ optional INDI challenger

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

# 17. Go/no-go principles

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

# 18. Hard authority boundaries

This roadmap does not authorize changes to:

```text
current G_action estimand
current randomized manifest
AURA/FAST/T1/C1 semantics
PX4 firmware PID / PX4 authority
Direct Guard
M_STABLE_US
W_MAX_US
T_D/T_A
H1000
SEALED
production authority
```

Any material change to those boundaries requires explicit owner review, a versioned scientific/control contract, implementation evidence and qualification.
