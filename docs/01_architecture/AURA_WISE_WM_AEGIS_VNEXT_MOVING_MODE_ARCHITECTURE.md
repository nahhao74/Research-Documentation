# AURA–WISE–World Model–AEGIS vNext — Moving Mode Architecture

## Scope

This document records the current canonical architecture required to resume the Detect and Response project without reopening retired branches. Scope is multirotor **Moving Mode only**.

## Top-level control architecture

```text
                         FAST PATH
Sensors/PX4/Reference -> AURA -> AEGIS FAST/T1/C1 -> PX4 -> UAV
        |
        +-------------> StateBank (always warm)
                              |
                              v
                      World Model / WISE
                              |
                      candidate U_plan
                              |
                              v
                       AEGIS ActivePlan
                              |
                              v
                             PX4
```

The World Model never blocks the first response. If WM is stale, uncertain, unavailable or too slow, AURA + FAST/T1/C1 + PX4 must remain operational.

## Module responsibilities

### PX4

PX4 remains authoritative for the cascaded inner control loops, attitude/rate control, control allocation and motors. AEGIS/WM inject only at qualified bounded boundaries and do not directly write motors/thrust/attitude/rate.

### AURA

AURA estimates the current deployable disturbance/equivalent external acceleration effect. Horizontal output is conceptually `d_hat_N`, `d_hat_E` plus validity, confidence, freshness and causal identity.

The fast W20 observable remains causal and onset/change sensitive. Sustained disturbance can be absorbed by the W20 baseline; this is why T1/bridge continuity remains part of the active baseline.

### AEGIS FAST/T1/C1

Immediate FAST correction uses the qualified acceleration-correction boundary before PX4 `_accelerationControl()`.

The active baseline `B` for scientific identification is:

```text
PX4 + AURA + FAST/T1/C1
```

The incremental candidate does not replace this baseline.

### StateBank

StateBank is always-warm causal memory. H1000 is a rolling causal history over native source time with no future leakage, interpolation, resampling or forward-fill.

Required bootstrap streams currently include:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

### World Model

Current decomposition:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

`F_nominal` models controlled baseline dynamics under active PX4+AURA+FAST/T1/C1. `G_action` models the incremental effect of the bounded candidate plan.

### WISE

WISE is the predictive planning/scoring layer. It evaluates candidate rollouts using trajectory error, velocity/cross-track behavior, control effort, uncertainty and constraints. It is model-predictive/model-based in function but need not be a heavy online nonlinear MPC solver.

## Scientific estimand

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

with `B` equal to the active baseline. FAST/PX4 reactions occurring after the candidate are downstream closed-loop treatment response, not pre-treatment confounding.

## Candidate exposure contract

Frozen pilot arms:

```text
ZERO: exactly 0 nonzero accepted candidate cycles
P1:   0.008 m/s^2 x exactly 5 accepted cycles
P2:   0.012 m/s^2 x exactly 7 accepted cycles
Directions: +E, -E, +N
```

Exposure is defined at the qualified requested/accepted correction boundary. No compensation cycles, post-hoc re-dose, arm relabeling or partial-root pooling are allowed.

## Temporal authority

### Native source continuity

Source continuity is defined by native PX4 source timestamp + publication generation. Native-source gap threshold remains `20,000 us`.

### Clock alignment

Cross-domain clock alignment is a separate predicate. A Timesync mapping transition must not be interpreted as native source loss.

Current versioned observational wire:

```text
SensorCombinedStampedV1
WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V2
```

It carries native source identity and exact sender mapping provenance while standard SensorCombined semantics remain unchanged.

### T_D / T_A

`T_D` is the native causal decision frontier. `T_A` is the native accepted-controller-cycle frontier. Host/mapped time is alignment provenance, not the authoritative source identity.

### H1000

H1000 remains exactly `1,000,000` native source microseconds and candidate-only for scientific refractory/history semantics.

## Randomized pilot contract

Frozen manifest SHA256:

```text
253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7
```

Campaign design:

```text
Worker A
8 sessions = 4 CALM + 4 GUST_E
12 blocks/session = 96 blocks
ZERO = 48
P1 = 24
P2 = 24
```

Pilot is for mechanism variance/carryover/SNR calibration and causal-identification readiness, not a standalone final efficacy proof. A complete valid root is required before E0/E1/E2 statistics are run.

## Fail-closed governance

- Root becomes immutable at first scientific row/block.
- First invalid scientific row stops the root.
- No patch-and-continue.
- No merging/pooling partial invalid roots.
- Mapping transition during science remains fail-closed unless exact transition certification is explicitly qualified.
- SEALED remains locked until approved final evaluation.
- `production_authority=false`.

## Storage

All substantial runtime/capture/dataset/replay/training artifacts belong under:

```text
/media/nahhao74/KINGSTON
```

Do not place large artifacts under `/home`.

## Current immediate blocker

The latest pilot attempt stopped before the first scientific `T_D`. StateBank/AURA bootstrap readiness itself passed live with all 7 required streams and an accepted snapshot ACK. The specialized randomized-pilot runner omitted `bootstrap_only=True` in the `block_index=-1` bootstrap transaction, causing fall-through into the candidate-offer/C1-frontier path.

Current immediate repair is therefore mechanical orchestration only: fix that call site, audit all `run_transaction()` call sites, and exact-preflight the specialized runner before another owner-authorized scientific root.
