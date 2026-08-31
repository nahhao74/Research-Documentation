# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap

**Canonical active roadmap version:** `v5 — CTEE-F / freshness / efficient identification / uncertainty expansion — 2026-08-31`  
**Scope:** Moving Mode only  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9`

The exact full local roadmap artifact is:

```text
AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v5_20260831.md
```

Detailed v5 algorithmic research note:

[`ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md`](ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md)

Current execution ladder:

[`../00_overview/CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md`](../00_overview/CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md)

Current Source Registry index:

[`../02_source_registry/CURRENT_REGISTRY_V9.md`](../02_source_registry/CURRENT_REGISTRY_V9.md)

Historical roadmap snapshots remain retained for provenance.

---

## 1. Canonical current state

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

OPTION_B_DIRECTION=PREFERRED
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

REFERENCE_STABILITY_PREDICATE=
GUST_PREOFFER_REFERENCE_STABILITY_V1

AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

EVENT_SCHEDULING_POLICY=
DELAY_RESCHEDULE_WITHIN_BLOCK
# RECOMMENDED, NOT FROZEN, NOT APPROVED

COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=NOT_IDENTIFIABLE
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

No research algorithm in v5 changes the exact next action.

---

## 2. Immediate Phase-0 execution order

```text
OWNER AUTHORIZE Q1 NOSCIENCE NO-LAUNCH RUNTIME
        ↓
Q1 bounded no-launch observation
        ↓
offline full-predicate characterization
        ↓
owner freezes:
M_STABLE_US
W_MAX_US
scheduling/fail-closed policy
source-time authority
        ↓
Tarjan static dependency preflight
        ↓
CTEE benchmark against:
- ad-hoc state machine
- conventional timed FSM
- timed-automata/runtime-enforcement baseline
        ↓
select smallest implementation that wins on measured KPI
        ↓
Option-B implementation
        ↓
deterministic regression
        ↓
bounded delayed-launch nonscientific qualification
        ↓
owner scientific review
        ↓
fresh randomized G_action science
        ↓
scientific analysis
        ↓
causal dataset acceptance
```

This remains the only valid path to World-Model action-response training.

---

## 3. CTEE — current candidate runtime primitive

CTEE is a project proposal for live causal/temporal eligibility:

```text
three-valued predicates
PASS / FAIL / UNKNOWN
        ↓
source/provenance-bound max-plus eligibility frontier
        ↓
readiness potential / critical blocker
        ↓
atomic session/reset/generation recheck
        ↓
COMMIT / RECOMPUTE / FAIL CLOSED
```

CTEE is not a flight controller and does not replace AURA, FAST/T1/C1, WM, WISE, AEGIS or PX4.

Current status:

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

`SRC-150` strengthens the prior-art baseline: timed runtime enforcement already supports delaying/suppressing timed events. Therefore CTEE must not claim novelty merely for waiting until a timing predicate becomes true.

Potential contribution remains narrower:

```text
pre-treatment causal eligibility
+
source-time/session/reset/generation provenance
+
arm-neutral randomized intervention semantics
+
max-plus earliest admissible frontier
+
atomic generation-safe commit
+
bounded fail-closed execution
```

Retain CTEE only if it materially improves correctness, tail ready→commit timing/jitter, or engineering/forensic complexity relative to simpler timed solutions.

---

## 4. Supporting algorithm roles

Keep roles separate:

```text
Tarjan SCC
= static dependency-cycle validation before runtime

CTEE
= candidate live causal/temporal eligibility

Sweep Line
= offline scientific interval/overlap/carryover validation

MCMF / network flow
= future conditional allocation only when a real constrained-allocation problem exists
```

Do not collapse these into one super-algorithm.

---

## 5. New v5 research track — CTEE-F

### 5.1 Motivation

CTEE answers:

> Are all causal prerequisites admissible now?

It does not fully answer:

> Will the state/plan still be fresh enough when PX4 actually consumes it?

The predictive chain can age information substantially:

```text
StateBank snapshot
→ World Model
→ WISE
→ AEGIS
→ DDS / PX4 scheduling
→ actual PX4 acceptance
```

### 5.2 CTEE-F definition

**CTEE-F = Causal Temporal Eligibility and Freshness Engine**

Let:

```text
A_i       = current age of required information item i
D_rem_wc  = justified remaining-delay envelope to its consumer
A_i_max   = frozen freshness budget
T_E       = CTEE causal eligibility frontier
```

Require:

\[
T_{now}\ge T_E
\]

and:

\[
A_i + D_{rem,wc} \le A_{i,max}
\]

for all critical inputs/plans, plus the normal source/session/reset/generation atomic recheck.

### 5.3 Age of Information

Research basis: `SRC-151`.

Potential measurements:

```text
state_source_age_us
reference_age_us
AURA_age_us
WM_input_age_us
WM_prediction_age_us
WISE_plan_age_us
AEGIS_candidate_age_us
age_at_PX4_acceptance
```

AoI is a freshness metric; it does not itself define project-specific freshness budgets.

### 5.4 Network Calculus

Research basis: `SRC-152`.

Network Calculus is a candidate method for deterministic remaining-delay/backlog bounds only after local assumptions are verified.

Required order:

```text
measure ROS2/DDS/PX4 arrivals/service/scheduling
→ verify model assumptions
→ construct candidate bound
→ validate bound against retained traces
→ only then expose a D_remaining envelope to CTEE-F
```

Do not substitute a theoretical bound for local timing evidence.

### 5.5 CTEE-F KPI

Potential improvements:

```text
stale-plan execution ↓
prediction applied to wrong state ↓
deadline failures ↓
tail age-at-application ↓
unnecessary stale-plan reuse ↓
effective tracking/planning utility ↑ indirectly
```

### 5.6 CTEE-F gate

CTEE-F is not a Phase-0 dependency.

Compare after Phase-1 timing data exists:

```text
TTL-only gate
vs
measured-age gate
vs
current-age + empirical p99 remaining delay
vs
CTEE-F + justified remaining-delay envelope
```

Retain only if measurable stale/deadline/closed-loop benefit exceeds added complexity.

```text
CTEE_F_STATUS=RESEARCH_HYPOTHESIS
CTEE_F_CURRENT_PHASE0_DEPENDENCY=false
CTEE_F_DOES_NOT_UNBLOCK_Q1=true
```

---

## 6. Phase 1 — latency, freshness and frequency-domain characterization

After Phase-0 causal dataset acceptance, characterize the full chain:

```text
physical/source event
→ PX4 source publication
→ uXRCE serialization
→ Agent receive
→ ROS callback ready
→ ROS callback scheduled
→ AURA
→ AEGIS
→ DDS transmit
→ PX4 receive
→ PX4 accepted control cycle
→ actuator
→ plant response
```

Measure:

```text
p50 / p95 / p99 / max
jitter
deadline misses
callback waiting vs compute
source age
queue depth
drop/overwrite counts
CPU load
```

Also perform bounded, authorized frequency-domain characterization:

```text
FFT / PSD
cross-spectrum
coherence
closed-loop FRF
MIMO FRF
bounded multisine
phase lag
N↔E coupling
resonance / oscillation
```

New v5 Phase-1 output:

```text
AoI at StateBank snapshot
AoI at WM start/end
AoI at WISE plan creation
AoI at AEGIS decision
AoI at PX4 acceptance

D_remaining(context, load, executor state)
```

This evidence supports later CTEE-F shadow benchmarking.

---

## 7. Phase 2 — AURA detector challengers

Keep current AURA/W20 as baseline until a challenger wins on the same causal traces.

Candidate families:

```text
W20 baseline
CUSUM / QCD
Random Fourier Feature sequential detector
multivariate online changepoint detector
multi-stream QCD variants
```

Required metrics:

```text
onset delay
direction-change delay
clear delay
false-positive / false-negative
amplitude/direction error
phase delay
CPU/sample
memory
tail execution time
```

Do not choose a detector because it is newer.

---

## 8. Post-Phase-0 research — CIBES

**CIBES = Causal Information-Budgeted Excitation Scheduler**

This is explicitly **not** allowed inside the current frozen randomized pilot.

After Phase-0 closure, CIBES may investigate safe probing actions that maximize information:

\[
U^* = \arg\max_U \operatorname{ExpectedInformationGain}(U)
\]

subject to:

```text
candidate authority limits
operational state constraints
CTEE admissibility
carryover/refractory rules
context support
newly frozen randomization/design rule
PX4 authority
```

Research basis: `SRC-153`.

Potential KPI:

```text
treatment SNR ↑
G_action identifiability ↑
information per independent flight/session ↑
required sessions ↓
low-information exposures ↓
```

Critical boundary: adaptive action selection changes the experiment design. Any CIBES campaign requires a new causal/randomization contract including selection policy, probabilities, availability, logging and estimand/analysis adjustments.

```text
CIBES_STATUS=POST_PHASE0_RESEARCH_HYPOTHESIS
CURRENT_FROZEN_SCIENCE_CHANGE_AUTHORIZED=false
```

---

## 9. Phase 3 — World Model

Keep the minimum-complexity model ladder:

```text
M0 ZERO / constant
M1 linear / ridge
M2 local linear / LPV
M3 SINDY
M4 Koopman
M5 tiny residual MLP
M6 TCN only if history actually helps
```

Target decomposition remains:

\[
Y_{future}=F_B(X,h)+G_{action}(X,U,h)
\]

with:

```text
B = active PX4 + AURA + FAST/T1/C1
```

Also retain:

```text
StateBank history ablation
T_D → T_A delay-aware state prediction
uncertainty-aware prediction
```

---

## 10. World-Model uncertainty ladder

Do not jump directly to one uncertainty method.

```text
U0 residual empirical quantiles
U1 heteroscedastic mean/variance
U2 ensemble/bootstrap
U3 conformal prediction/error bounds
U4 set-membership uncertainty
U5 tractable ellipsoidal outer approximation
```

### Conformal track

Research basis: `SRC-154`.

Potential benefits:

```text
empirical coverage calibration ↑
OOD overconfidence ↓
risk-aware WISE scoring ↑
```

The cited work includes a 12D quadcopter benchmark, but it does not authorize copying its controller architecture into this project.

### Set-membership track

Research basis: `SRC-155`, `SRC-156`.

Potential benefits:

```text
explicit model-error/dynamics uncertainty set
robust-optimization compatibility
clear bounded-error interpretation under assumptions
```

Risks:

```text
conservatism
nonlinear-set cost
noise-bound misspecification
```

If exact sets are too expensive, test a conservative tractable outer approximation.

Required evaluation:

```text
coverage
conditional coverage by context
OOD coverage
set width / conservatism
G_action prediction utility
WISE closed-loop utility
constraint violations
runtime
```

---

## 11. Phase 4 — WISE planner ladder

```text
WISE-0 bounded candidate enumeration
↓
WISE-1 small QP / TinyMPC
↓
WISE-2 linear / Koopman MPC
↓
WISE-3 acados + RTI
↓
WISE-4 NMPC / MPPI only if earlier approaches are insufficient
```

World Model/WISE remain predictive refinement only; they never become a first-response prerequisite.

Event-triggered reuse remains preferred when it reduces predictive-path contention without creating stale-plan execution.

---

## 12. Phase 5 — bounded online adaptation

Prefer:

```text
frozen representation
+
small adaptive coefficient/state vector
+
bounded update law
+
confidence / rollback
```

Do not default to unrestricted in-flight neural-network retraining.

A Lyapunov-bounded adaptation challenger may be tested later if it improves nonstationary behavior while preserving bounded/fail-closed semantics.

---

## 13. Phase 6 — AEGIS runtime assurance

Future AEGIS research may add:

```text
uncertainty-aware projection
Lyapunov analysis
CLF
CBF
runtime-assurance switching/projection
```

while preserving PX4 inner-loop authority.

Conceptually:

\[
U_{safe}=\arg\min_U\|U-U_{WISE}\|^2
\]

subject to qualified safety/authority constraints.

---

## 14. Updated experiment sequence

```text
P0-Q1
NO-LAUNCH SHADOW CHARACTERIZATION
        ↓
P0-OWNER
M_STABLE / W_MAX / POLICY FREEZE
        ↓
P0-PREFLIGHT
TARJAN + CTEE/TIMED-ENFORCER BENCHMARK
        ↓
P0-NOSCIENCE
DELAYED-LAUNCH QUALIFICATION
        ↓
P0-SCIENCE
RANDOMIZED G_action PILOT + DATASET DECISION
        ↓
B2-OPTIONAL
CIBES RESEARCH UNDER A NEW EXPERIMENT CONTRACT
        ↓
E1
END-TO-END LATENCY + FFT/FRF
        ↓
E1B
AoI + AGE-AT-APPLICATION + REMAINING-DELAY ENVELOPE
        ↓
E1C
CTEE-F SHADOW / REPLAY BENCHMARK
        ↓
E2
AURA DETECTOR SHADOW BAKE-OFF
        ↓
E3
F_nominal MODEL LADDER
        ↓
E4
G_action MODEL LADDER
        ↓
E5
HISTORY ABLATION
        ↓
E6
T_D→T_A DELAY MODEL
        ↓
E7
UNCERTAINTY CALIBRATION
quantile / ensemble / conformal / set-membership
        ↓
E8
WISE-0 CANDIDATE ENUMERATION
        ↓
E9
EVENT-TRIGGERED WISE
        ↓
E10
TinyMPC / Koopman / RTI benchmark
        ↓
E11
LOW-DIMENSIONAL ONLINE ADAPTATION
        ↓
E12
FORMAL AEGIS SAFETY FILTER
```

---

## 15. Go/no-go rules added in v5

### CTEE

Go only if it beats simpler timed alternatives on at least one meaningful axis without semantic regression:

```text
correctness
or tail latency/jitter
or engineering/forensic complexity
```

### CTEE-F

Go only if:

```text
freshness/remaining-delay modeling predicts real stale-at-application cases
AND CTEE-F reduces stale/deadline failures or improves closed-loop utility
AND delay-bound assumptions are validated locally
AND runtime overhead remains bounded
```

### CIBES

Go only if:

```text
information per independent session improves materially
AND causal/randomization validity remains explicit
AND operational constraints remain satisfied
AND it is introduced only under a newly frozen post-Phase-0 experiment contract
```

### Conformal / set-membership uncertainty

Go only if:

```text
coverage/calibration improves on held-out sessions/contexts
AND OOD behavior beats simpler residual/ensemble baselines
AND the resulting uncertainty is useful to WISE/AEGIS
AND runtime/optimization cost is acceptable
```

---

## 16. v9 source mapping for new tracks

```text
SRC-150
→ timed runtime enforcement competitor/baseline for CTEE

SRC-151
→ Age of Information / freshness

SRC-152
→ deterministic Network Calculus methodology

SRC-153
→ adaptive constrained experiment design / CIBES research

SRC-154
→ conformalized robust OOD MPC / quadcopter benchmark

SRC-155
→ set-membership uncertainty learning

SRC-156
→ tractable ellipsoidal set-membership approximation
```

---

## 17. Final design principle

```text
measure
→ identify the real bottleneck
→ introduce the smallest credible method
→ compare against the simpler baseline
→ validate causality / timing / runtime cost
→ retain only if benefit is real
```

The project should optimize for **measurable closed-loop value**, not algorithm count.

The current next action remains Q1 authorization/execution; CTEE-F, CIBES and advanced uncertainty methods do not change that boundary.
