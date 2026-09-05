# AURA–WISE–WORLD MODEL–AEGIS vNext
## Active Implementation and Research Roadmap

**Canonical roadmap:** `v8 — fresh_35 forensic / FAST shadow benchmark / WM baseline review`  
**Scope:** Moving Mode only  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9`

This file defines the **active future direction**. It does not grant runtime or scientific authority.

Current authority:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

---

# 1. Current project state

Latest randomized root:

```text
LATEST_FAILED_SCIENTIFIC_ROOT=fresh_35
FRESH35_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN

G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

`fresh_35` passed preflight but stopped in the first CALM row before any GUST, candidate, `T_D`, ACK or exposure.

A valid same-lineage native status exists inside the current source-match budget. The present evidence does not prove whether the status:

```text
never reached the observer callback
was received and later removed from bounded retention
or remained retained but was not selectable by matcher/indexing logic
```

The next main-pipeline task is therefore the accepted-cycle callback visibility/retention forensic.

---

# 2. Phase-0 execution path

```text
accepted-cycle callback visibility / retention forensic
        ↓
minimal implementation-preserving repair if a concrete defect is proven
        ↓
bounded non-scientific qualification
        ↓
canonical reverse index → graph → Tarjan → peeling
        ↓
owner review
        ↓
new immutable randomized root only if separately authorized
        ↓
complete-root causal dataset admission
        ↓
minimal G_action identification
```

Hard rules:

```text
no timeout increase merely to obtain PASS
no QoS change merely to obtain PASS
no source/session/reset semantic relaxation
no patch-and-continue inside a scientific root
no partial-root pooling
no WM training from incomplete/invalid roots
```

---

# 3. Current scientific baseline

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1 baseline
```

The current FAST/T1/C1 semantics remain frozen for Phase-0.

A material FAST change changes baseline `B`; therefore the action-conditioned model must be re-evaluated under the newly frozen baseline before production use.

---

# 4. Active research track A — FAST improvement on the current simulator

This is the primary control research track now.

Research question:

> Can the current simulated vehicle reject wind faster and more accurately than the existing `-d_hat + T1/C1` path while preserving PX4 firmware control semantics?

This work is shadow/replay/separate non-scientific research only until Phase-0 closes.

## 4.1 Baseline

```text
F0 = current AURA + FAST(-d_hat) + T1/C1 + PX4
```

## 4.2 Research order

Do not choose an algorithm before identifying the measured limitation.

```text
F1 — timing / freshness / source-age alignment
     preserve the current control law; remove avoidable latency/age first

F2 — bounded static shaping / normalization
     only if repeated evidence shows systematic under/over-correction

F3 — synchronized incremental / INDI-like challenger
     only if measured plant-response dynamics remain the dominant limitation

F4 — combined mechanism
     only if ablation proves complementary benefit
```

No new PI/PID tracking loop is part of the active FAST roadmap.

No online adaptive gain tuning, cross-airframe adaptation or commercial fleet commissioning mechanism is part of the current simulator phase.

## 4.3 FAST instrumentation and metrics

Measure at minimum:

```text
F0 sensor/source frontier
F1 AURA input accepted
F2 disturbance estimate ready
F3 FAST correction ready
F4 PX4 correction/setpoint accepted
F5 first measurable plant response
```

Report:

```text
segment p50 / p95 / p99 / max latency
source age at application
T_effect
T_recover
peak velocity deviation
peak position deviation
velocity RMSE
cross-track RMSE
sustained-GUST error
post-CLEAR overshoot / settling
control effort
command variation / jerk
projection / saturation / headroom
```

Always separate:

```text
ONSET
SUSTAINED GUST
CLEAR / RECOVERY
```

## 4.4 FAST promotion rule

```text
if challenger does not clearly beat F0
→ retain current FAST

if challenger clearly beats F0 with no important robustness/latency regression
→ freeze a new versioned FAST baseline after Phase-0
→ reacquire/revalidate G_action under that baseline before production WM use
```

---

# 5. Active research track B — World Model / WISE

Canonical structure remains:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

where:

```text
F_nominal = future evolution of the active closed-loop baseline
G_action  = incremental candidate effect relative to ZERO under that same baseline
```

Current allowed WM work:

```text
StateBank / causal state representation
model interfaces
training/evaluation pipeline implementation
serving contracts
latency instrumentation
uncertainty output interface
small baseline-model tooling
```

Blocked now:

```text
final G_action training
final action-conditioned model selection
WISE efficacy claims
production model promotion
```

---

# 6. Baseline review before final production WM

After Phase-0 closes and FAST shadow research is complete:

```text
BASELINE REVIEW
```

### Outcome A — current FAST remains best

```text
freeze B0
→ use causal action data valid for B0
→ minimal WM identification/training
```

### Outcome B — a FAST challenger is materially better

```text
freeze B1
→ version control/scientific contract
→ reacquire/revalidate action-conditioned G_action under B1
→ train final production WM under B1
```

Do not assume:

```text
G_action^(B0) == G_action^(B1)
```

without evidence.

---

# 7. Minimal World Model ladder

Once causal data for the final chosen baseline is accepted, use the smallest auditable model first:

```text
WM0 — ZERO / persistence / simple closed-loop predictor
WM1 — linear / ridge action-conditioned model
WM2 — compact structured dynamics model if WM1 is insufficient
WM3 — small nonlinear model only if measured residual structure requires it
```

Model capacity is earned by held-out improvement, not by architecture complexity.

Evaluate separately:

```text
prediction quality
G_action incremental-response quality
H0/H20/H40/H80 behavior
uncertainty calibration
serving latency / deadline misses
planning utility
incremental benefit over best FAST baseline
```

---

# 8. WISE planning direction

WISE remains a bounded predictive candidate selector, not first-response control.

Preferred first implementation:

```text
causal StateBank state
→ bounded candidate library / enumeration
→ WM rollout
→ score tracking / effort / uncertainty / constraints
→ reject stale / uncertain / infeasible candidates
→ bounded AEGIS execution
```

Do not introduce heavier MPC/Koopman/RTI planning unless simple bounded enumeration fails a measured requirement.

---

# 9. WM/WISE deployment go-no-go

WM/WISE is not automatically required in the final runtime architecture.

After the best FAST baseline is established, compare:

```text
C0 = best qualified FAST baseline
C1 = C0 + WM/WISE predictive refinement
```

Retain WM/WISE in runtime only if repeat-supported incremental benefit justifies:

```text
compute cost
latency
uncertainty / OOD risk
additional failure modes
engineering complexity
```

If C1 does not materially improve the Pareto frontier, WM remains a research/benchmark path rather than production dependency.

---

# 10. Deferred research

These topics are retained only as later research and are **not active implementation work**:

```text
CALE causal learning-admission formalization
CTEE-F / AoI / age-at-application gating
advanced uncertainty calibration
stronger AEGIS runtime assurance
AURA detector challengers
```

They must not displace the current priorities.

---

# 11. Explicitly rejected / inactive directions

The following are removed from the active roadmap:

```text
residual PI / 2-DOF PI as the primary FAST direction
manual per-airframe PI tuning
online gain adaptation in the current simulator phase
cross-airframe adaptive controller / fleet commissioning architecture
online neural-network control adaptation
adding algorithms because literature contains them without a measured failure class
```

Do not reintroduce these as active candidates unless the owner explicitly reopens them after new evidence.

Git history preserves prior discussion.

---

# 12. Integrated roadmap

```text
NOW
├─ MAIN PIPELINE
│  accepted-cycle visibility/retention forensic
│  → minimal repair if proven
│  → bounded non-scientific qualification
│  → owner review
│  → later fresh randomized root if authorized
│  → causal dataset acceptance
│
└─ PARALLEL SHADOW RESEARCH
   current FAST F0
   → latency/source-age characterization
   → smallest justified FAST challengers
   → repeat-supported benchmark

AFTER PHASE-0
baseline review: current FAST vs best challenger
        ↓
freeze final baseline B*
        ↓
ensure G_action causal data is valid for B*
        ↓
minimal F_nominal + G_action WM
        ↓
WISE bounded-candidate benchmark
        ↓
C0 best FAST vs C1 best FAST + WM/WISE
        ↓
retain only components with measurable incremental value
```

---

# 13. Go/no-go principle

```text
measure
→ identify the real bottleneck
→ introduce the smallest credible mechanism
→ compare against the simpler baseline
→ validate timing / causality / robustness
→ retain only if repeat-supported benefit is real
```

Do not optimize for algorithm count or novelty.

---

# 14. Hard authority boundaries

This roadmap does not authorize changes to:

```text
current G_action estimand
current randomized manifest
current AURA/FAST/T1/C1 semantics
PX4 firmware PID / PX4 authority
Direct Guard
M_STABLE_US
W_MAX_US
T_D/T_A
H1000
SEALED
production authority
```

Any material change requires explicit owner review, versioned control/scientific contract, implementation evidence and qualification.
