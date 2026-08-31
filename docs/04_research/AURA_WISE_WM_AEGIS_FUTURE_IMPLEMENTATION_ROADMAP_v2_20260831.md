# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap
### Moving-Mode UAV Detect & Response Pipeline

**Status:** Canonical working roadmap / design & research roadmap  
**Scope:** Moving Mode only  
**Updated:** 2026-08-31  
**Research registry reference:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7`  
**Important:** This document is a roadmap. It does **not** authorize changes to the current frozen randomized-identification, timing, control-authority, safety, or production contract.

---


# 0. Canonical current state — 2026-08-31

This section supersedes older roadmap-state summaries. It records where the project actually stands now; it does **not** change the scientific contract.

Authoritative current research registry:

```text
CURRENT_RESEARCH_REGISTRY_VERSION=
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7

ARTIFACT=
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7.md

UPDATED=2026-08-29
SOURCES=141
RESOLVED=138
UNRESOLVED=3

UNRESOLVED_IDS=
SRC-024
SRC-040
SRC-053
```

Do not invent a v7 JSON filesystem path or SHA256 until an exact canonical JSON artifact is actually verified or created/promoted.

## 0.1 Phase-0 canonical state

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED

OPTION_B_DIRECTION=PREFERRED
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

REFERENCE_STABILITY_PREDICATE=
GUST_PREOFFER_REFERENCE_STABILITY_V1

AUTHORITATIVE_TIME_DOMAIN=
PX4_BOOT_US
# selected/proposed source-time authority
# still inside owner freeze boundary

M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

EVENT_SCHEDULING_POLICY=
DELAY_RESCHEDULE_WITHIN_BLOCK
# RECOMMENDED
# NOT_FROZEN
# NOT_APPROVED

COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED

FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=
NOT_IDENTIFIABLE

W_MAX_FULL_PREDICATE_OFFLINE_SUPPORT=
UNRESOLVED

W_MAX_RUNTIME_FEASIBILITY=
UNPROVEN

CHARACTERIZATION_STATE=
PREPARED_FOR_OWNER_NOSCIENCE_RUNTIME_AUTHORIZATION

PHASE_0B3_IMPLEMENTATION=BLOCKED
PHASE_0B4_DETERMINISTIC_REGRESSION=BLOCKED
PHASE_0B5_FRESH_NOSCIENCE_PARITY=BLOCKED
PHASE_0B6_SCIENTIFIC_PILOT_OWNER_REVIEW=BLOCKED
PHASE_0B7_FRESH_RANDOMIZED_SCIENCE=BLOCKED
PHASE_0B8_SCIENTIFIC_ANALYSIS=BLOCKED
PHASE_0B9_CAUSAL_DATASET_ACCEPTANCE=BLOCKED

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION

MANIFEST_SLOTS_CONSUMED=0
SCIENTIFIC_EXECUTION=NOT_RUN

AEGIS_AUTHORITY=0
ROUND_Z_AUTHORITY=0
production_authority=false
```

## 0.2 Closed 0B.1 causal conclusion

The current blocker must **not** be described as a candidate-treatment failure, DDS callback loss, AURA contradiction, FAST/T1/C1 failure, or World-Model problem.

Canonical root-cause class:

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

The failure occurs before candidate offer and before scientific `T_D`.

Therefore the immediate roadmap remains inside **Phase 0 scientific/causal acquisition**, not World-Model training.

## 0.3 Why historical traces are insufficient

Existing GUST traces can show reference-side stable chronology but cannot identify the true delayed-launch full-predicate counterfactual because the native GUST already occurred before the proposed stability window completed.

Current evidence supports:

```text
REFERENCE_ONLY_COUNTERFACTUAL_SUPPORT
```

but not:

```text
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT
```

Therefore neither:

```text
M_STABLE_US
```

nor:

```text
W_MAX_US
```

may be frozen from the historical trace alone.

## 0.4 Q1 — current next evidence gate

The minimum next evidence task is:

```text
Q1 — BOUNDED NON-SCIENTIFIC NO-LAUNCH SHADOW QUALIFICATION
```

Current Q1 state:

```text
Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=
PREPARED

Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false
```

Q1 must create a **true unexposed waiting trajectory** around the nominal GUST-launch opportunity:

```text
session/block ready
    ↓
nominal GUST launch opportunity identified
    ↓
record nominal_requested_launch_source_us
in PX4_BOOT_US
    ↓
DO NOT launch native GUST
    ↓
continue observing:
reference
AURA
C1
session/reset
source continuity
provenance
    ↓
evaluate GUST_PREOFFER_REFERENCE_STABILITY_V1
in SHADOW ONLY
    ↓
bounded termination
```

Q1 must not:

```text
launch native GUST
offer treatment
reach scientific T_D
consume manifest slots
access SEALED
estimate G_action
implement Option B
freeze M_STABLE_US
freeze W_MAX_US
```

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

during the Q1 observation window.

Q1 should record enough source-causal state to evaluate the characterization grid offline. The grid remains evidence-generation only:

```text
0
25 ms
50 ms
75 ms
100 ms
150 ms
200 ms
250 ms
300 ms
400 ms
500 ms
750 ms
1000 ms
```

No value in this grid is owner-approved merely because it is tested.

## 0.5 What Q1 can and cannot establish

Q1 can create evidence for:

```text
nominal_requested_launch_source_us

first_full_predicate_eligible_source_us

full_counterfactual_wait_us

FULL_PREDICATE support across candidate M_STABLE_US

candidate W_MAX offline coverage envelope
```

For identifiable opportunities:

\[
\text{full\_counterfactual\_wait\_us}
=
\text{first\_full\_predicate\_eligible\_source\_us}
-
\text{nominal\_requested\_launch\_source\_us}
\]

Q1 still **cannot** establish:

```text
W_MAX_RUNTIME_FEASIBILITY
```

because it does not execute:

```text
delayed native launch
→ AURA onset binding
→ T_D
→ offer
→ ACK / accepted exposure
→ release
→ H1000
```

Therefore after Q1:

```text
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN
```

must remain true until a separately authorized delayed-launch runtime qualification is completed.

## 0.6 Owner freeze boundary after Q1

Q1 provides a decision envelope; it does not select owner policy.

Option B may only become frozen after explicit owner decisions for:

```text
APPROVE_OPTION_B=true|false

M_STABLE_US=<numeric source-time interval>

W_MAX_US=<bounded common timeout>

APPROVE_DELAY_RESCHEDULE=true|false

APPROVE_FAIL_CLOSED_TIMEOUT=true|false

APPROVE_SOURCE_TIME_DOMAIN=PX4_BOOT_US|other
```

Until then:

```text
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false
```

## 0.7 Updated Phase-0 execution ladder

```text
PHASE 0A
mechanism / structural prerequisites
    CLOSED
      ↓
PHASE 0B.1
GUST P1 forensic / first causal divergence
    CLOSED
      ↓
PHASE 0B.2a
historical offline/reference-only characterization
    REACHED EVIDENCE LIMIT
      ↓
PHASE 0B.2b
Q1 no-launch shadow preparation
    PREPARED
      ↓
OWNER AUTHORIZATION FOR Q1 NOSCIENCE RUNTIME
      ↓
Q1 bounded no-launch shadow execution
      ↓
offline full-predicate characterization
      ↓
owner margin/policy decision
      ↓
OPTION-B CONTRACT FREEZE
      ↓
0B.3 implementation
      ↓
0B.4 deterministic regression
      ↓
0B.5 bounded delayed-launch nonscience qualification
      ↓
0B.6 owner scientific-pilot review
      ↓
0B.7 fresh randomized science
      ↓
0B.8 scientific analysis
      ↓
0B.9 causal dataset acceptance
```

No extra scientific smoke/corridor should be invented after each gate unless a concrete unresolved validity condition requires it.

---

# 1. Purpose

This document records the long-term implementation direction for the current AURA–WISE–World Model–AEGIS pipeline.

The goal is to improve:

- end-to-end disturbance-response latency;
- predictive accuracy;
- closed-loop trajectory tracking;
- robustness to disturbance/model mismatch;
- computational determinism;
- uncertainty handling;
- online adaptation;
- runtime safety;

while preserving the fundamental architecture:

```text
FAST PATH:
AURA → FAST/T1/C1 → PX4

PREDICTIVE PATH:
StateBank → World Model → WISE → bounded candidate → AEGIS → PX4
```

The predictive path must refine the existing controller, not replace the immediate response path.

---

# 2. Architectural invariants

The following properties are treated as hard architecture constraints unless explicitly redesigned and approved later.

## 2.1 FAST path must remain independently functional

The system must continue to operate when the World Model or WISE is:

- unavailable;
- stale;
- warming;
- uncertain;
- late;
- rejected;
- disabled.

Required fallback:

```text
AURA
  ↓
FAST/T1/C1
  ↓
PX4
  ↓
UAV
```

The World Model must never become a prerequisite for first disturbance response.

---

## 2.2 PX4 remains inner-loop authority

The predictive layer must not directly assume authority over:

- motors;
- raw motor mixing;
- raw thrust outside the qualified boundary;
- attitude loop;
- rate loop;
- control allocation;
- actuator safety limits.

The preferred insertion domain remains a bounded acceleration-level correction path above PX4 inner loops.

Conceptually:

```text
u_baseline
    = qualified FAST/T1/C1 contribution

u_total_requested
    = u_baseline + u_candidate
```

This equality is an exact **requested-action composition** identity.

It must not be interpreted as a physical linear-superposition assumption:

```text
Y(B + U) ≠ Y(B) + Y(U)
```

in general.

---

# 3. Scientific foundation that must be closed first

Before major predictive-control implementation, the project must establish whether the candidate action carries usable causal signal.

The scientific target is:

\[
G_{action}(X,U,h)
=
Y(B+U,h)-Y(B+ZERO,h)
\]

where:

```text
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The World Model should learn the future of the **already-controlled closed-loop system** rather than an artificial bare-airframe system with feedback disabled.

---

## 3.1 Current causal interpretation

A randomized candidate can alter vehicle state, which can then cause:

- AURA to react;
- FAST/T1/C1 to react;
- PX4 feedback loops to react;
- allocation and actuator behavior to change.

These post-treatment reactions are part of the realized closed-loop candidate effect.

Correct conceptual split:

```text
pre-treatment AURA / FAST / PX4 state
    → context / moderator / model input

post-treatment FAST / PX4 response
    → treatment-mediated closed-loop response
```

The downstream feedback response must not be automatically removed from the target.

---

## 3.2 T_D and T_A remain separate

The architecture distinguishes:

```text
T_D = causal decision frontier
T_A = actual accepted-action frontier
```

At `T_D`, the real future `T_A` is not yet known.

Therefore the online model must not use actual future `T_A` as an input.

Actual `T_A` may be used after execution for:

- supervision;
- audit;
- target alignment;
- realized-latency estimation.

This distinction becomes an important component of the future latency-aware World Model.

---

# 4. Overall future architecture

The target architecture is:

```text
                         ┌──────────── FAST PATH ─────────────┐
                         │                                    │
Sensors / PX4 ──> AURA vNext ──> FAST/T1/C1 ───────────────┐ │
      │                   │                                  │ │
      │                   └──── disturbance context          │ │
      │                                                      v v
      │                                                    AEGIS
      │                                                      │
      │                                                      v
      └──> StateBank ──> delay-aware state ──> WM ──> WISE ─> PX4
              warm            T_D → T_A        │       │        │
                                                │       │        v
                                   F_nominal + G_action  │      UAV
                                   + uncertainty         │        │
                                                        │        └─ feedback
                                                        │
                                                   bounded plan
```

The long-term design should preserve a strict separation between:

```text
current disturbance rejection
predictive future refinement
online model adaptation
safety/runtime assurance
```

---

# 5. Phase 0 — Scientific / causal data closure

Phase 0 is intentionally longer than a conventional "pilot preparation" stage because the target is not merely to prove that a candidate command can run.

The required causal chain is:

```text
randomized assignment
→ exact pre-treatment eligibility
→ exact intervention timing
→ exact accepted exposure
→ exact T_A
→ uncontaminated future target
→ valid randomized comparison
→ usable G_action dataset
```

The project must not train the action-conditioned World Model merely because runtime can execute candidate commands.

## 5.1 Current subphase map

```text
0A      structural/mechanism closure                         CLOSED
0B.1    GUST-P1 timing/design forensic                       CLOSED
0B.2a   historical/reference-only characterization           COMPLETE TO EVIDENCE LIMIT
0B.2b   Q1 no-launch shadow preparation                      PREPARED
0B.2c   Q1 live no-launch characterization                   OWNER AUTHORIZATION REQUIRED
0B.2d   owner numeric margin/policy freeze                   PENDING
0B.3    Option-B implementation                              BLOCKED
0B.4    deterministic regression                             BLOCKED
0B.5    fresh bounded nonscientific delayed-launch parity    BLOCKED
0B.6    owner scientific-pilot review                        BLOCKED
0B.7    fresh randomized science                             BLOCKED
0B.8    randomized scientific analysis                       BLOCKED
0B.9    causal dataset acceptance                            BLOCKED
```

## 5.2 Current scientific estimand remains unchanged

\[
G_{action}(X,U,h)
=
Y(B+U,h)-Y(B+ZERO,h)
\]

where:

```text
B = active PX4 + AURA + FAST/T1/C1
```

Option B is an eligibility/scheduling proposal only. It must not change:

```text
G_action definition
P1/P2 force profile
candidate magnitude/duration
FAST/T1/C1 mathematics
AURA lifecycle semantics
H1000 semantics
T_D/T_A semantics
PX4 control authority
SEALED policy
production authority
```

## 5.3 Q1 objective

Q1 must answer:

> At the point where a GUST would normally have been requested, if the native GUST is intentionally not launched yet, how do reference state, AURA, C1, session/reset identity, source continuity and provenance evolve in PX4_BOOT_US?

This is the missing causal trajectory that historical already-exposed traces cannot provide.

## 5.4 Q1 required output for owner decision support

For each valid opportunity, derive or classify:

```text
nominal_requested_launch_source_us

first_full_predicate_eligible_source_us

full_counterfactual_wait_us

predicate PASS/FAIL/UNKNOWN

context/session support

arm-neutrality support

M_STABLE_US sensitivity

candidate W_MAX offline coverage
```

Every quantity must be tagged conceptually as:

```text
OBSERVED

DERIVED_FROM_OBSERVED_PRETREATMENT_STATE

COUNTERFACTUAL_SCHEDULING_QUANTITY
```

Do not describe reference-only chronology as full-predicate support.

## 5.5 After Q1 — owner decision, not automatic implementation

If Q1 gives adequate numeric evidence, the next state is an owner decision envelope, not an automatic choice.

Only explicit owner approval may freeze:

```text
M_STABLE_US
W_MAX_US
DELAY_RESCHEDULE_WITHIN_BLOCK
FAIL_CLOSED_TIMEOUT
PX4_BOOT_US authority
```

## 5.6 After Option-B freeze

Only then:

```text
implement minimal Option-B gate
→ deterministic regression
→ bounded nonscientific delayed-launch qualification
```

The runtime qualification must prove actual operational chronology:

```text
stability predicate becomes true
→ delayed native GUST launches
→ AURA onset binds correctly
→ pre-offer eligibility
→ T_D
→ exact offer
→ exact ACK / accepted exposure
→ release
→ H1000
```

This is the first stage that can establish:

```text
W_MAX_RUNTIME_FEASIBILITY
```

## 5.7 Fresh randomized science

After owner authorization and nonscientific parity closure, run a new immutable scientific root under a newly frozen manifest/runtime identity.

The pilot remains intended to estimate output-space treatment support, not to train a high-capacity model automatically.

Primary diagnostics include:

```text
ZERO variability
P1 vs ZERO at H40/H80
P2 vs ZERO at H40/H80
session variation
CALM vs GUST_E
carryover
lagged FAST × candidate interaction
projection activity
constraint activity
saturation
accepted-exposure fidelity
```

Use the frozen assigned-arm and clustered inference contract.

## 5.8 Phase-0 branching decision

If valid randomized evidence is:

```text
CLEAR
```

and causal dataset acceptance criteria pass:

```text
→ freeze causal action-conditioned dataset
→ proceed toward model development
```

If:

```text
WEAK
```

then:

```text
→ experiment-design / excitation / horizon / observability review
```

If:

```text
NOT_RESOLVED
```

then:

```text
→ do not train an action-conditioned WM as though G_action were established
```

Do not automatically increase treatment amplitude.

---

# 6. Phase 1 — End-to-end latency / real-time / frequency-response characterization

This phase should occur before replacing major algorithms.

Primary source families:

```text
SRC-104  PX4 filter/control latency
SRC-105  PX4 uORB
SRC-106  PX4 architecture
SRC-107  ROS 2 processing-chain response time
SRC-108  PREEMPT_RT
SRC-109  cyclictest

SRC-128  DDS/UAV latency measurement
SRC-129  PiCAS chain-aware ROS 2 scheduling
SRC-130  ROS 2 Executor / WaitSet / Events Executor
SRC-141  deterministic ROS 2 execution research
```

---

## 6.1 Required latency chain

Instrument at least:

```text
physical / source event
        ↓
PX4 source publication
        ↓
uXRCE serialization
        ↓
Agent receive
        ↓
ROS callback ready
        ↓
ROS callback actually scheduled
        ↓
AURA start
        ↓
AURA end
        ↓
AEGIS start
        ↓
AEGIS end
        ↓
DDS transmit
        ↓
PX4 receive
        ↓
PX4 accepted control cycle
        ↓
actuator output
        ↓
measured plant response
```

---

## 6.2 Decompose total latency

Use:

\[
T_{total}
=
T_{sensor/source}
+
T_{transport}
+
T_{callback-wait}
+
T_{AURA-compute}
+
T_{AEGIS-compute}
+
T_{PX4}
+
T_{actuation}
+
T_{plant}
\]

Do not collapse callback latency into one number.

Explicitly separate:

\[
T_{callback}
=
T_{waiting}
+
T_{compute}
\]

because:

```text
callback waiting >> compute
```

implies scheduling/executor optimization is more valuable than algorithm micro-optimization.

---

## 6.3 Metrics

Measure at minimum:

```text
p50
p95
p99
maximum
jitter
deadline misses
callback waiting time
callback compute time
source age
CPU utilization
context switches
queue depth
drop / overwrite counts
```

---

## 6.4 Decision rule

If:

```text
callback_wait >> AURA_compute
```

prioritize:

- executor;
- callback grouping;
- scheduling;
- CPU affinity;
- realtime policy;
- transport contention.

If:

```text
AURA_compute / AURA decision delay
```

dominates, then prioritize detector redesign.

---


## 6.5 Phase 1A vs Phase 1B

Split Phase 1 into two authority classes.

### Phase 1A — read-only characterization

May overlap with late Phase 0 only when it does not alter the scientific baseline.

Allowed examples:

```text
timestamping
trace analysis
callback-wait measurement
transport-delay measurement
T_D→T_A distribution measurement
spectral diagnostics
offline FRF estimation
```

Forbidden during a frozen scientific campaign:

```text
executor change
QoS change
CPU-priority change
PREEMPT_RT policy change
filter retuning
AURA semantic change
FAST/T1/C1 timing/control-law change
```

### Phase 1B — latency optimization

Only after baseline requalification boundaries are explicit.

Any change that materially alters closed-loop timing may change:

\[
B_1 \rightarrow B_2
\]

and potentially:

\[
G^{B_1}_{action}
\neq
G^{B_2}_{action}
\]

Therefore latency optimization must not silently occur inside one causal-identification campaign.

## 6.6 Fourier / spectral / FRF characterization

Add an explicit frequency-domain characterization track.

This is distinct from Random Fourier Features used later for change detection.

Candidate analyses:

```text
FFT / PSD
cross-spectrum
coherence
closed-loop frequency response
MIMO FRF
bounded multisine probing
phase-lag characterization
N↔E coupling
resonance / oscillation detection
```

Primary questions:

```text
Which frequency bands contain candidate-observable response?

Where does FAST/PX4 attenuate or amplify candidate effects?

What is the phase lag from candidate to acceleration/velocity/position?

Is there meaningful N↔E coupling?

Does the chosen H40/H80 horizon align with useful dynamic bandwidth?

Are filters or scheduling delays introducing avoidable phase loss?
```

FRF/multisine work must remain bounded and scientifically authorized. It must not be introduced into the current frozen Phase-0 campaign without a new experiment contract.

Relevant registry families include closed-loop identification/UAS flight-data identification and PX4 latency/filter references.

# 7. Phase 2 — AURA vNext detector challengers

Current AURA/W20 should remain the baseline until a challenger demonstrates superior behavior.

Candidate source families:

```text
SRC-121  Quickest Change Detection
SRC-131  Random Fourier Feature online change detection
SRC-132  Online multivariate changepoint detection
SRC-133  Multi-stream QCD review
SRC-139  adaptive/gain-scaled disturbance observer
```

---


Random Fourier Features are a **change-detection representation/approximation tool**, not the same as FFT/FRF system identification.

AURA challenger family should therefore distinguish:

```text
W20 current baseline

CUSUM / Page-Hinkley / QCD

Random Fourier Feature sequential detector

exact multivariate likelihood-ratio changepoint detector

multi-stream QCD variants
```

Random Fourier Features should only be retained if they improve the detection-delay / false-alarm / runtime Pareto frontier on the same causal traces.

## 7.1 Do not directly replace W20

First create:

```text
AURA DETECTOR SHADOW BENCH
```

All detector candidates must process the same causal traces.

Example:

```text
same raw causal trace
        │
        ├── W20 baseline
        ├── CUSUM / QCD
        ├── RFF sequential detector
        └── multivariate detector
```

---

## 7.2 Metrics

Measure:

```text
disturbance onset delay
direction-change delay
clear delay
false-positive rate
false-negative rate
amplitude-estimation error
direction error
phase delay
CPU time / sample
memory
tail execution time
```

---

## 7.3 Selection principle

Do not choose a detector because it is newer.

Prefer candidates on the latency/accuracy Pareto frontier.

A detector is interesting only if it materially improves:

```text
detection delay
```

without unacceptable degradation of:

```text
false alarms
noise sensitivity
direction estimation
compute cost
```

---

# 8. Phase 3 — World Model v1

The initial World Model should use the minimum complexity that explains the data.

Preferred decomposition:

\[
Y_{future}
=
F_B(X,h)
+
G_{action}(X,U,h)
\]

where:

```text
F_B
    = future behavior under active baseline
      PX4 + AURA + FAST/T1/C1

G_action
    = incremental candidate-induced closed-loop response
```

---

# 9. Model ladder

Primary source families:

```text
SRC-117  PI-TCN
SRC-118  residual multirotor dynamics
SRC-119  SINDY-MPC
SRC-120  Koopman-based quadrotor MPC
SRC-122  sparse GP-MPC
SRC-124  uncertainty-aware learned residual MPC
```

The recommended model ladder is:

```text
M0  ZERO / constant predictor
 ↓
M1  linear / ridge
 ↓
M2  local linear / LPV
 ↓
M3  SINDY sparse model
 ↓
M4  Koopman / lifted reduced-order model
 ↓
M5  tiny residual MLP
 ↓
M6  temporal model / causal TCN
```

Do not start with a large neural World Model.

---

## 9.1 Selection criterion

Select models based on:

```text
prediction quality
incremental-action prediction quality
latency
tail latency
memory
sample efficiency
interpretability
uncertainty calibration
closed-loop planning utility
```

Example decision:

```text
SINDY:
95.0% useful prediction
0.1 ms

MLP:
96.0%
0.4 ms

TCN:
96.3%
2.5 ms
```

A TCN is not automatically preferable just because its point prediction is slightly better.

---

# 10. Phase 3.1 — StateBank history ablation

StateBank may retain long causal history, but serving-model history length should be determined empirically.

Test:

```text
H0      current state only
H20
H40
H80
H160
H320
H1000
```

Evaluate improvement in:

```text
future-state prediction
G_action prediction
generalization
uncertainty calibration
```

against:

```text
compute
memory
latency
model complexity
```

Important:

```text
scientific retained history
≠
required serving-model history length
```

H1000 can remain part of scientific/history semantics even if the serving model only benefits from a much shorter input window.

---

# 11. Phase 3.2 — Delay-aware World Model

This is one of the highest-priority future accuracy improvements.

Because:

\[
T_D \neq T_A
\]

a plan generated at the decision frontier may be acting on a different vehicle state when physically accepted.

Let:

\[
T_A = T_D + \tau_{apply}
\]

If the vehicle is moving rapidly:

\[
X(T_D) \neq X(T_A)
\]

Therefore ignoring action-application latency can produce an accurate model used at the wrong state.

Source family:

```text
SRC-126  latency-aware quadrotor control
```

---

## 11.1 Stage A

Predict the state at action acceptance:

\[
X(T_D),
H_t,
\hat{\tau}_{apply}
\rightarrow
\hat X(T_A)
\]

---

## 11.2 Stage B

Predict the future from the estimated accepted-frontier state:

\[
\hat X(T_A),
U
\rightarrow
Y(T_A+h)
\]

---

## 11.3 Required ablation

Compare:

```text
A. ignore application delay

B. use fixed mean delay

C. context-conditioned application-delay predictor
```

Use the simplest variant that provides meaningful closed-loop benefit.

Actual future `T_A` remains audit/training supervision and must not leak into online decision-time state.

---

# 12. Phase 3.3 — Uncertainty-aware World Model

The predictive model should eventually output more than a point estimate.

Instead of only:

\[
\hat Y
\]

prefer:

\[
(\hat Y,\sigma_Y)
\]

or a predictive set:

\[
\mathcal{Y}_h
\]

Relevant source families:

```text
SRC-098
SRC-122
SRC-124
```

---

## 12.1 Why uncertainty matters

Example:

```text
Candidate U1
predicted tracking error = 2 cm
uncertainty              = ±20 cm

Candidate U2
predicted tracking error = 4 cm
uncertainty              = ±3 cm
```

A mean-only optimizer selects `U1`.

A risk-aware planner may correctly prefer `U2`.

---

## 12.2 Possible WISE cost

\[
J(U)
=
J_{tracking}
+
\lambda_u J_{control}
+
\lambda_{\Delta u}J_{smoothness}
+
\lambda_\sigma J_{uncertainty}
+
J_{constraint-margin}
\]

Uncertainty must be calibrated before it is used as a safety proxy.

---


## 12.3 Optional Fourier-feature challenger

Fourier features may be tested inside the World Model only if history/state ablations show periodic or nonlinear structure that simple linear/SINDY/Koopman features do not capture efficiently.

Example feature map:

\[
\phi(x)
=
[
\sin(\omega_1 x),
\cos(\omega_1 x),
\ldots
]
\]

Possible uses:

```text
compact nonlinear basis for G_action
spectral summary of short StateBank history
periodic/oscillatory residual representation
```

This is an optional challenger, not a mandatory architecture component.

Decision criterion:

```text
retain only if G_action prediction or closed-loop WISE utility
improves materially after accounting for compute/latency
```

---

# 13. Phase 4 — WISE predictive planner

WISE should be interpreted as a planning/scoring role, not as a commitment to heavy NMPC.

Relevant source families:

```text
SRC-110  acados
SRC-111  RTI
SRC-112  TinyMPC
SRC-113  explicit MPC
SRC-114  MPPI
SRC-115  event-triggered MPC
SRC-120  Koopman linear MPC
SRC-123  realtime quadrotor MPC
```

---

# 14. WISE solver ladder

Recommended progression:

```text
WISE-0
bounded candidate enumeration / library search

WISE-1
small convex QP / TinyMPC

WISE-2
linear / Koopman MPC

WISE-3
acados + RTI

WISE-4
full nonlinear MPC / MPPI
only if earlier approaches are demonstrably insufficient
```

---

## 14.1 WISE-0 must be treated as a serious baseline

For an action vector such as:

\[
U=
[\Delta a_N,\Delta a_E]
\]

one can enumerate a finite candidate set:

```text
ZERO

±N small
±N medium
±N large

±E small
±E medium
±E large

selected diagonal combinations
selected short-duration temporal plans
```

For each plan:

```text
World Model rollout
        ↓
trajectory score
        ↓
constraint / uncertainty check
        ↓
best bounded candidate
```

No iterative solver is required.

If this gives comparable tracking performance with lower latency than MPC, it should remain the preferred implementation.

---

# 15. Phase 4.1 — Event-triggered WISE

A high-rate StateBank does not imply a high-rate optimizer.

Preferred runtime pattern:

```text
StateBank       always updating
AURA/FAST       always active

WISE:
compute plan
reuse
reuse
reuse
reuse
recompute on event
```

Relevant source:

```text
SRC-115
```

---

## 15.1 Candidate triggers

Possible triggers include:

\[
\|X-\hat X\| > \epsilon
\]

or:

```text
new disturbance event
reference change
prediction uncertainty increase
plan horizon expiry
cross-track error growth
constraint margin reduction
model validity loss
candidate stale/freshness boundary
```

---

## 15.2 Benefits

Event-triggering may improve:

```text
CPU utilization
tail latency
scheduler contention
predictive-path determinism
energy usage
```

while protecting FAST from competing computation.

---

# 16. Phase 5 — Online adaptation

Real UAV dynamics are nonstationary.

Potential changes include:

```text
battery state
payload
motor degradation
temperature
air density
aerodynamics
wind regime
```

Relevant source families:

```text
SRC-101
SRC-125
SRC-136  Neural-Fly
SRC-137
SRC-138
```

---

## 16.1 Avoid unrestricted online neural-network retraining

Do not default to:

```text
online backpropagation of full network weights
```

on the flight-critical runtime.

Prefer:

```text
frozen representation
+
small adaptive parameter vector
```

---

## 16.2 Example architecture

Offline:

\[
\Phi(X,U)
\]

Online:

\[
G(X,U)
=
\Phi(X,U)\alpha_t
\]

Only:

\[
\alpha_t
\]

is adapted.

Runtime state can include:

```text
base-model identity
adaptive coefficients
adaptation confidence
adaptation validity
rollback state
```

If confidence fails:

```text
adaptive residual disabled
→ frozen base model retained
```

---


## 16.3 Lyapunov-bounded adaptation candidate

If online adaptation is later introduced, include a challenger in which the small adaptive state/parameter vector is governed by an update law with an explicit Lyapunov-style boundedness/stability argument.

Conceptually:

```text
frozen offline representation
+
small adaptive coefficient vector
+
bounded update law
+
confidence / rollback
```

Do not infer that a Lyapunov adaptive law must replace the current controller. Its role is to constrain or certify a future adaptation mechanism.

Selection requires evidence that the adaptive variant improves nonstationary performance while preserving bounded/fail-closed behavior.

---

# 17. Phase 5.1 — Separate disturbance bandwidths

A useful architecture pattern from observer + learned-model literature is:

```text
fast unpredictable disturbance
        ↓
AURA / observer

predictable closed-loop dynamics
        ↓
F_nominal + G_action

slowly varying model mismatch
        ↓
small online adaptation
```

A possible future decomposition is:

\[
Y
=
F_B
+
G_{action}
+
R_{slow}
\]

while:

\[
d_{fast}
\]

remains a separate high-bandwidth disturbance estimate handled by AURA/FAST.

The World Model should not be forced to replace AURA.

---

# 18. Phase 6 — AEGIS runtime assurance / Lyapunov–CLF–CBF safety layer

Relevant sources:

```text
SRC-127  unified Safety Filter review
SRC-140  quadrotor runtime assurance
```

Current AEGIS already handles mechanisms such as:

```text
identity
generation
freshness
bounds
release
stale-plan rejection
```

A future AEGIS version may add formal safety projection.

---

## 18.1 Conceptual safety projection

Given WISE output:

\[
U_{WISE}
\]

compute:

\[
U_{safe}
=
\arg\min_U
\|U-U_{WISE}\|^2
\]

subject to:

\[
g_i(X,U)\le0
\]

Possible constraints:

```text
acceleration envelope
tilt reserve
actuator reserve
altitude envelope
geofence
trajectory envelope
stability-related margin
control-authority reserve
```

---

## 18.2 Safety-filter principle

The safety filter must:

- preserve PX4 authority;
- operate only inside qualified candidate authority;
- have a bounded runtime budget;
- fail closed;
- never become a hidden replacement controller;
- expose exact intervention/projection diagnostics.

---


## 18.3 Lyapunov / CLF / CBF research track

AEGIS may later move from purely provenance/bounds/freshness mediation toward a formally constrained runtime-assurance layer.

Candidate mathematical tools:

```text
Lyapunov stability analysis
Control Lyapunov Functions (CLF)
Control Barrier Functions (CBF)
Barrier Lyapunov functions where appropriate
runtime-assurance switching / projection
```

Conceptual future path:

```text
WISE candidate
    ↓
AEGIS provenance / identity / freshness
    ↓
uncertainty + constraint check
    ↓
Lyapunov / CLF / CBF condition
    ↓
safe bounded candidate
    ↓
PX4
```

Example stability-style condition:

\[
V(x_{next}) - V(x)
\le
-\alpha \|x\|^2
\]

or a continuous-time equivalent where justified.

Example safety projection:

\[
U_{safe}
=
\arg\min_U \|U-U_{WISE}\|^2
\]

subject to certified safe constraints.

Important:

```text
Lyapunov/CBF does not replace PX4 inner-loop authority.

A paper/reference suggesting a controller redesign does not authorize
replacing the current PX4 attitude/rate/control-allocation stack.

Formal safety work belongs after G_action identification, WM/WISE validation,
and characterization of the actual candidate authority envelope.
```

---

# 19. Priority order for latency improvements

Recommended priority:

| Priority | Improvement | Reason |
|---|---|---|
| 1 | End-to-end scheduling/transport decomposition | Can expose hidden multi-ms or tens-of-ms delay |
| 2 | AURA detector bake-off | Can reduce disturbance-detection phase delay |
| 3 | `T_D → T_A` compensation | Reduces the control impact of unavoidable application latency |
| 4 | Event-triggered WISE | Reduces contention and predictive-path load |
| 5 | TinyMPC / explicit MPC / candidate search | Reduces planning compute |
| 6 | More deterministic ROS 2 execution | Use if tail jitter remains significant |

---

# 20. Priority order for accuracy improvements

Recommended priority:

| Priority | Improvement |
|---|---|
| 1 | Valid causal `G_action` identification |
| 2 | Delay-aware state prediction |
| 3 | Structured residual model |
| 4 | History-length ablation / temporal model |
| 5 | Uncertainty-aware prediction |
| 6 | Small online adaptation |
| 7 | Improved disturbance estimation / AURA vNext |

The key principle is:

```text
better causal data
>
larger model
```

---

# 21. Explicit non-goals

The roadmap intentionally avoids the following unless future evidence strongly justifies a redesign.

## 21.1 No end-to-end neural replacement of the flight stack

Do not default to:

```text
sensor
→ giant neural model
→ neural controller
→ motors
```

---

## 21.2 No World Model in the FAST dependency path

The predictive layer must not block the first response.

---

## 21.3 No direct bypass of PX4 inner loops

WISE/WM must not directly command motors or replace qualified inner-loop authority.

---

## 21.4 No unrestricted online retraining

Do not update a large neural network in-flight without a dedicated stability/safety/rollback contract.

---

## 21.5 No heavy optimizer by default

Do not use NMPC/MPPI if:

```text
candidate enumeration
or
small convex optimization
```

provides equivalent control performance.

---

## 21.6 No detector redesign without false-alarm analysis

A faster detector is not automatically better.

---

# 22. Three system generations

## vNext-1 — Scientific system

Primary objective:

```text
demonstrate whether causal incremental action response
G_action
is identifiable under the active controller baseline
```

Includes:

```text
runtime/source qualification
randomized candidate experiment
T_D/T_A causal contract
exact accepted exposure
treatment SNR
carryover analysis
```

---

## vNext-2 — Performance system

Primary objective:

```text
improve real closed-loop trajectory behavior
without increasing first-response dependency
```

Candidate additions:

```text
end-to-end latency optimization
AURA detector challenger
F_nominal + G_action WM
history ablation
delay-aware T_D→T_A prediction
uncertainty
WISE candidate search
event-triggered planning
TinyMPC / lightweight MPC if useful
```

This is a strong candidate architecture for the main performance paper.

---

## vNext-3 — Adaptive / safety-oriented system

Primary objective:

```text
handle nonstationarity and move toward robust real-world deployment
```

Candidate additions:

```text
low-dimensional online adaptation
adaptive disturbance estimation
formal safety filter
runtime assurance
hardware stress testing
tail-latency validation
degradation / rollback testing
```

---

# 23. Research implementation policy

The source registry should be used to generate hypotheses and candidate architectures.

It must not be treated as authorization to modify the current scientific system.

Use the hierarchy:

```text
1. exact local source/build + raw runtime evidence
2. current internal scientific/architecture contract
3. version-matched official PX4 / eProsima / ROS source/docs
4. primary scholarly literature
5. secondary technical sources
6. inference / hypothesis
```

If a paper suggests a better architecture:

```text
paper
  ↓
shadow implementation
  ↓
offline/replay ablation
  ↓
non-scientific qualification
  ↓
owner scientific/control review if semantics change
  ↓
only then controlled integration
```

---

# 24. When additional research is warranted

The current source set is broad enough to design the next major experiments.

Additional literature search should be opened when a concrete evidence gap appears.

Examples:

## If temporal models remain inadequate

Research:

```text
feedback-aware temporal dynamics
hybrid state-space neural models
latent controlled dynamics
```

---

## If uncertainty calibration fails

Research:

```text
conformal prediction
Bayesian residual models
ensemble calibration
risk-sensitive MPC
```

---

## If latency remains unexplained

Research:

```text
ROS 2 executor internals
RMW-specific behavior
DDS transport tuning
CPU isolation
PREEMPT_RT
lock contention
NUMA / cache effects
```

after first confirming the exact local runtime implementation.

---

## If online adaptation is unstable

Research:

```text
bounded adaptive control
dual control
safe online identification
projection-based adaptation
runtime adaptation monitors
```

---

## If WISE planning is too slow

Research:

```text
explicit MPC
policy distillation
warm-start QP
reduced-order lifted dynamics
event-triggered planning
```

---

# 25. Recommended experiment sequence after current scientific closure

```text
P0-Q1
NO-LAUNCH SHADOW CHARACTERIZATION
        ↓
P0-OWNER
M_STABLE / W_MAX / POLICY FREEZE
        ↓
P0-NOSCIENCE
DELAYED-LAUNCH PARITY QUALIFICATION
        ↓
P0-SCIENCE
RANDOMIZED G_action PILOT + DATASET DECISION
        ↓
E1
END-TO-END LATENCY + FFT/FRF BASELINE
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

Each experiment should have a frozen baseline and explicit decision criterion.

---

# 26. Example go/no-go criteria

## AURA challenger

Go only if:

```text
latency improves materially
AND
false-alarm rate acceptable
AND
direction/amplitude estimation not degraded materially
AND
runtime cost fits deadline
```

---

## Temporal WM

Go only if:

```text
history improves DEV/generalization enough
to justify additional compute and latency
```

---

## Delay-aware WM

Go only if:

```text
T_D→T_A modeling improves future prediction
or planning utility beyond constant-delay baseline
```

---

## Uncertainty-aware WISE

Go only if:

```text
uncertainty is calibrated
AND
risk-aware scoring improves closed-loop robustness
```

---

## TinyMPC / NMPC

Go only if:

```text
control benefit exceeds candidate enumeration
by enough margin to justify compute and complexity
```

---

## Online adaptation

Go only if:

```text
adaptation improves out-of-distribution behavior
AND
has bounded/fail-closed behavior
AND
can be rolled back cleanly
```

---

# 27. Primary design principle

The pipeline should evolve according to:

```text
minimum complexity
that produces a demonstrable
closed-loop benefit
```

not:

```text
maximum model sophistication
available in the literature
```

The preferred decision order is:

```text
measure
→ identify bottleneck
→ introduce smallest credible improvement
→ shadow/replay test
→ ablate
→ validate causality and latency
→ integrate only if benefit is real
```

---

# 28. Final roadmap summary

```text
CURRENT PHASE 0B.2
Q1 NO-LAUNCH SHADOW
        │
        ▼
owner M_STABLE / W_MAX / scheduling freeze
        │
        ▼
delayed-launch nonscience qualification
        │
        ▼
fresh randomized G_action identification
        │
        ▼
END-TO-END LATENCY + FFT/FRF CHARACTERIZATION
        │
        ├───────────────┐
        │               │
        ▼               ▼
AURA detector        scheduling /
challengers          transport optimization
        │               │
        └───────┬───────┘
                ▼
           WORLD MODEL v1
                │
      F_nominal + G_action
                │
          model ladder
                │
        history ablation
                │
         delay-aware T_D→T_A
                │
           uncertainty
                │
                ▼
              WISE
                │
       candidate enumeration
                │
        event-triggered reuse
                │
       ┌────────┴─────────┐
       │                  │
   TinyMPC / QP       Koopman MPC
       │                  │
       └────────┬─────────┘
                │
        acados / RTI only
          if justified
                │
                ▼
       SMALL ONLINE ADAPTATION
                │
                ▼
         AEGIS LYAPUNOV / CLF / CBF SAFETY
                │
                ▼
               PX4
```

The intended outcome is a UAV control stack with:

```text
very fast first response
+
causal action-conditioned prediction
+
low-latency predictive refinement
+
uncertainty awareness
+
bounded online adaptation
+
formal runtime assurance
+
PX4 inner-loop authority preserved
```

This roadmap should remain evidence-driven. Each new algorithm must earn its place through a controlled comparison against the simpler baseline it is intended to replace.


# 29. Authoritative internal references for this update

The 2026-08-31 roadmap-state update is grounded in the current internal artifacts:

```text
docs/GUST_P1_OPTION_B_REFERENCE_STABILITY_CONTRACT_DECISION.md

Q1 — BOUNDED NON-SCIENTIFIC NO-LAUNCH SHADOW
QUALIFICATION PREPARATION

AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7.md
```

Current decision-package facts:

```text
PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN
```

Current Q1 facts:

```text
Q1 contract prepared
Q1 runtime not authorized
Q1 runtime not executed
scientific execution not run
manifest slots consumed = 0
```

Current registry facts:

```text
v7
updated 2026-08-29
141 sources
138 resolved
3 unresolved:
SRC-024
SRC-040
SRC-053
```

This roadmap must be re-synchronized whenever exact source/tests/raw runtime evidence supersede these facts.
