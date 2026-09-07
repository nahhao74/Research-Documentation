# AURA–WISE–World Model–AEGIS vNext
## Active Implementation and Research Roadmap

**Canonical roadmap:** `v9 — D0 V3 closure → Phase D FAST bottleneck measurement → baseline review`  
**Updated:** 2026-09-07  
**Scope:** Moving Mode only

This document contains only the active future direction. Superseded execution paths remain in Git history.

## 1. Current priority

```text
CURRENT_MAINLINE=D0_V3_CAUSAL_OBSERVABILITY_AND_READINESS
D0_CLOSED=false
READY_FOR_PHASE_D=false
CURRENT_BLOCKER=DATA0_PRECOLLECTOR_STARTUP_ATTESTATION_CONTRACT
```

Formal root `_04` completed a clean 20 s window and exact downstream correspondence but did not pass readiness.

Prospective registry replay gives:

```text
VALID=2879
EXPLAINED=1116
NOT_READY=5
UNKNOWN=1
```

The one UNKNOWN is `CAUSAL_EXPECTED_AVAILABLE=false`; the five NOT_READY events are `motor_command_stale_or_missing`.

## 2. Immediate execution path

```text
close DATA0 precollector startup/attestation contract offline
        ↓
one expected/motor provenance probe
        ↓
resolve expected-state disposition
        ↓
resolve motor-command NOT_READY root cause
        ↓
freeze final registry/evaluator
        ↓
one final fresh D0 root
        ↓
D0 CLOSED only if UNKNOWN/NOT_READY/READINESS_FAILURE/INVARIANT_VIOLATION are all zero
        ↓
freeze Phase-D campaign
```

## 3. D0 V3 qualification principle

D0 is an observability/readiness gate, not a demand for continuous valid control.

Allowed PASS-compatible state:

```text
EXPLAINED_CONTROL_UNAVAILABLE(<registry-approved exact cause>)
```

Non-PASS states:

```text
UNKNOWN_MISSING_EVIDENCE
NOT_READY_CONTROL_UNAVAILABLE
READINESS_FAILURE
INVARIANT_VIOLATION
```

`V3WindowEvaluator` remains the sole classifier. Causal precedence is earliest authoritative first-false only.

## 4. Phase D — measure before redesign

After D0 closure, characterize the current baseline:

```text
F0 = PX4 + AURA + current FAST/T1/C1
```

Separate disturbance phases:

```text
ONSET
SUSTAINED
CLEAR / RECOVERY
```

Measure:

```text
source age / AoI
F0→F4 latency
F5 diagnostic plant response
peak position / velocity deviation
RMSE
recovery
overshoot / settling
control effort
command TV / jerk
projection / saturation / headroom
validity gaps and recoveries
```

Do not select a replacement FAST algorithm before the dominant limitation is measured.

## 5. FAST challenger families — only after bottleneck identification

Possible families remain research candidates only:

```text
freshness/scheduling/AoI improvement
bounded shaping/scaling/phase compensation
estimator/feedforward/disturbance observer
acceleration-domain INDI-like correction
bounded short-horizon predictive correction
filtering/multirate/event-triggered mechanisms
```

No challenger is selected today.

Promotion requires repeat-supported improvement over F0 with no important robustness, latency, saturation or headroom regression.

## 6. Baseline dependency of World Model science

Frozen estimand:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

where `B` is the active closed-loop baseline.

If Phase D later promotes a material FAST change:

```text
B0 -> B1
```

then production action-response validity must be reviewed/reacquired under B1. Do not assume:

```text
G_action^(B0) == G_action^(B1)
```

## 7. World Model / WISE ladder

After the final baseline and causal dataset are accepted:

```text
WM0 persistence/simple closed-loop predictor
WM1 linear/ridge action-conditioned model
WM2 compact structured dynamics if justified
WM3 small nonlinear model only if residual evidence requires it
```

Canonical structure:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

WISE remains a bounded predictive candidate selector and never blocks FAST first response.

## 8. Production go/no-go

Compare:

```text
C0 = best qualified FAST baseline
C1 = C0 + WM/WISE predictive refinement
```

Retain WM/WISE only if repeat-supported incremental benefit justifies compute, latency, uncertainty and complexity.

## 9. Deferred work

```text
ADAPTIVE_FAST=FUTURE
CROSS_AIRFRAME=FUTURE
SIM_TO_REAL=FUTURE
WM_PORTABILITY=FUTURE
```

These must not displace D0 closure or measured Phase-D bottleneck work.

## 10. Integrated roadmap

```text
NOW
D0 precollector lifecycle closure
→ expected/motor provenance
→ final registry
→ final D0 qualification

NEXT
Phase-D metric freeze
→ F0 bottleneck measurement
→ smallest justified challenger
→ repeat benchmark
→ freeze best FAST baseline B*

THEN
validate/acquire G_action for B*
→ minimal WM
→ WISE benchmark
→ C0 vs C1 production decision
```

## Hard authority boundaries

```text
PX4 remains authoritative
FAST remains first-response path
WM/WISE cannot block first response
failed roots remain immutable
no semantic relaxation to obtain PASS
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
large runtime artifacts -> /media/nahhao74/KINGSTON
```
