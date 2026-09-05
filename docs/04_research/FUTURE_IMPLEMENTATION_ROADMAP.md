# AURA–WISE–WORLD MODEL–AEGIS vNext
## Active Implementation and Research Roadmap

**Canonical roadmap:** `v8 — fresh_35 forensic / FAST shadow benchmark / WM baseline review`  
**Scope:** Moving Mode only  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9`

This file contains **only the active future direction**. Superseded or rejected research is intentionally absent and remains recoverable through Git history.

Current runtime/scientific authority:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

---

# 1. Current state

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

`fresh_35` passed frozen preflight but stopped in the first CALM row before any GUST, candidate, `T_D`, ACK or exposure.

A valid same-lineage native status exists inside the current source-match budget. The present evidence does not prove whether the status failed to reach the observer callback, was lost from bounded retention after receipt, or remained retained but unselectable by matcher/indexing logic.

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

Current FAST/T1/C1 semantics remain frozen during Phase-0.

A material FAST change changes baseline `B`; therefore action-conditioned data/model validity must be reviewed under the newly frozen baseline.

---

# 4. Active research track A — FAST improvement on the current simulator

Research question:

> Can the current simulated vehicle reject wind faster and more accurately than the existing `-d_hat + T1/C1` path while preserving PX4 firmware control semantics?

This work is shadow/replay/separate non-scientific research only until Phase-0 closes.

## 4.1 Baseline

```text
F0 = current AURA + FAST(-d_hat) + T1/C1 + PX4
```

## 4.2 Research order

Do not choose a replacement algorithm before the measured limitation is known.

```text
1. characterize source age and end-to-end latency
2. separate ONSET / SUSTAINED GUST / CLEAR-RECOVERY failure modes
3. identify whether the dominant limitation is estimation, timing, scaling or plant response
4. introduce the smallest credible challenger for that measured limitation
5. compare against F0 under identical simulator conditions
6. retain only repeat-supported improvement with no important robustness regression
```

## 4.3 Required timing instrumentation

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

## 4.4 Promotion rule

```text
if challenger does not clearly beat F0
→ retain current FAST

if challenger clearly beats F0 with no important robustness/latency regression
→ freeze a new versioned FAST baseline after Phase-0
→ revalidate action-conditioned G_action under that baseline before production WM use
```

---

# 5. Active research track B — World Model / WISE

Canonical model structure:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

where:

```text
F_nominal = future evolution of the active closed-loop baseline
G_action  = incremental candidate effect relative to ZERO under that same baseline
```

Current allowed work:

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
current Phase-0 closure
+
FAST benchmark result
        ↓
BASELINE REVIEW
```

### Current FAST remains best

```text
freeze B0
→ use causal action data valid for B0
→ minimal WM identification/training
```

### A FAST challenger is materially better

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

Once causal data for the final chosen baseline is accepted:

```text
WM0 — ZERO / persistence / simple closed-loop predictor
WM1 — linear / ridge action-conditioned model
WM2 — compact structured dynamics model if WM1 is insufficient
WM3 — small nonlinear model only if measured residual structure requires it
```

Model capacity is earned by held-out improvement.

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

# 8. WISE direction

WISE is a bounded predictive candidate selector, not first-response control.

Preferred first implementation:

```text
causal StateBank state
→ bounded candidate library / enumeration
→ WM rollout
→ score tracking / effort / uncertainty / constraints
→ reject stale / uncertain / infeasible candidates
→ bounded AEGIS execution
```

Escalate planning complexity only if the simplest bounded search fails a measured requirement.

---

# 9. WM/WISE deployment go-no-go

After the best FAST baseline is established, compare:

```text
C0 = best qualified FAST baseline
C1 = C0 + WM/WISE predictive refinement
```

Retain WM/WISE in runtime only if repeat-supported incremental benefit justifies:

```text
compute cost
latency
uncertainty risk
additional failure modes
engineering complexity
```

If C1 does not materially improve the Pareto frontier, WM remains a research/benchmark path rather than a production dependency.

---

# 10. Deferred work

Later research may be opened only after the core path above exposes a specific need. It must not displace current priorities.

Current priorities are exactly:

```text
1. close fresh_35 infrastructure boundary
2. obtain a complete valid causal dataset
3. benchmark FAST on the current simulator
4. freeze the best FAST baseline
5. build the minimum useful WM for that baseline
6. prove whether WISE adds incremental closed-loop value
```

---

# 11. Integrated roadmap

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
   → measured limitation
   → smallest justified challenger
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

# 12. Go/no-go principle

```text
measure
→ identify the real bottleneck
→ introduce the smallest credible mechanism
→ compare against the simpler baseline
→ validate timing / causality / robustness
→ retain only if repeat-supported benefit is real
```

---

# 13. Hard authority boundaries

This roadmap does not authorize changes to:

```text
current G_action estimand
current randomized manifest
current AURA/FAST/T1/C1 semantics
PX4 firmware control semantics / PX4 authority
Direct Guard
M_STABLE_US
W_MAX_US
T_D/T_A
H1000
SEALED
production authority
```

Any material change requires explicit owner review, a versioned control/scientific contract, implementation evidence and qualification.
