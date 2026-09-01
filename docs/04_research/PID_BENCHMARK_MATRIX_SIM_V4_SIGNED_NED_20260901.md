# PID Benchmark Matrix — Simulation v4 Signed NED — 2026-09-01

This file supersedes the positive-only velocity examples in `PID_BENCHMARK_MATRIX_SIM_V2_20260901.md` and is the active simulation matrix for signed NED velocity testing.

## 1. Controller arms

```text
P0 FIXED-2DOF-PID
P1 GA-LOCAL-2DOF-PID
P2 RGS-2DOF-PID
P3 RGS-2DOF-PID-FF
P4 RGS-2DOF-PID-INDI
P5 RGS-2DOF-PID-INDI-DOB      conditional
P6 RGS-2DOF-PID-INDI-NN-SCHED conditional

C_FAST exact current qualified AURA + FAST/T1/C1 revision
C_FULL future qualified predictive pipeline revision; not assumed available now
```

Mandatory ablation:

```text
P0 → P1 → P2 → P3 → P4
```

## 2. Coordinate contract

All controller/model/metric variables use NED:

```text
N = +x
E = +y
D = +z
```

Sign semantics:

```text
+N north, -N south
+E east,  -E west
+D down,  -D up
```

## 3. Signed velocity envelope

Primary setpoint magnitudes:

```text
|v_N| = 3.0 m/s
|v_E| = 2.5 m/s
|v_D| = 0.8 m/s
```

All signs are required:

\[
v_N\in\{-3,+3\},\quad
v_E\in\{-2.5,+2.5\},\quad
v_D\in\{-0.8,+0.8\}\;m/s
\]

Primary 8-corner matrix:

```text
(+3.0,+2.5,+0.8)
(+3.0,+2.5,-0.8)
(+3.0,-2.5,+0.8)
(+3.0,-2.5,-0.8)
(-3.0,+2.5,+0.8)
(-3.0,+2.5,-0.8)
(-3.0,-2.5,+0.8)
(-3.0,-2.5,-0.8)
```

All vectors are `[v_N,v_E,v_D]` m/s.

Mandatory isolated reversals:

```text
(+3,0,0) ↔ (-3,0,0)
(0,+2.5,0) ↔ (0,-2.5,0)
(0,0,+0.8) ↔ (0,0,-0.8)
```

Mandatory full-vector reversals:

```text
(+3,+2.5,+0.8) ↔ (-3,-2.5,-0.8)
(+3,+2.5,-0.8) ↔ (-3,-2.5,+0.8)
```

## 4. Wind-force contract

Primary horizontal disturbance:

```text
0 N <= ||F_w,NE|| <= 15 N
F_D = 0 for primary wind campaign
```

Recommended magnitudes:

```text
0, 3, 6, 9, 12, 15 N
```

Reference physical ceiling from prior measurement:

```text
MEASURED_NO_DRIFT_REFERENCE_LIMIT = 20 N
ACTIVE_MAX = 15 N
REFERENCE_MARGIN = 5 N
```

For every signed horizontal setpoint direction, test:

```text
headwind
tailwind
left crosswind
right crosswind
±N
±E
diagonal NE/NW/SE/SW forces where non-redundant
```

## 5. Yaw exclusion

```text
YAW_OBJECTIVE=false
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
```

Yaw may be used only for a necessary body→NED transform. Equivalent NED cases must be repeated over multiple yaw headings to verify yaw invariance.

Fail the yaw-independence assumption if NED model residuals, coupling, or tracking metrics shift materially with yaw.

## 6. Scheduling-state policy

Initial candidate state:

\[
\rho_0=[v_N,v_E,v_D,\sqrt{v_N^2+v_E^2}]
\]

A simpler state is allowed only after ablation proves no material loss.

Do not replace signed components by absolute values unless positive/negative dynamics symmetry is empirically demonstrated.

Promote only if needed:

```text
NED acceleration / desired acceleration
tilt magnitude
motor/control-allocation headroom
control-effectiveness estimate
```

Battery remains excluded in simulation.

## 7. Identification gate

Before GA tuning require:

```text
closed-loop identification audit
MIMO VARX held-out prediction check
free-run simulation check
residual whiteness / input-residual correlation
N/E/D coupling assessment
positive-vs-negative directional residual audit
delay estimate
actuator-dynamics assessment
yaw-invariance audit or explicit pending state
```

Preferred model ladder:

```text
local SISO baseline
→ MIMO VARX
→ regularized/order-selected MIMO VARX
→ reduced MIMO state-space
→ local/LPV family if required
```

## 8. GA / synthesis gate

GA/NSGA-II must optimize tracking and smoothness while hard-rejecting unsafe candidates.

At minimum optimize/measure:

```text
N/E/D velocity RMSE
position RMSE / path cross-track error
ITAE
overshoot
settling time
control effort
command total variation
jerk
saturation exposure
peak tilt
headroom
frequency-domain robustness metrics
```

Hard rejection includes:

```text
unstable closed loop in plant ensemble
excessive tilt/thrust/headroom violation
unrecoverable saturation
integrator runaway
pathological gain transition
missed runtime deadline
failure to recover inside declared 0–15 N wind envelope
```

Lyapunov/closed-loop stability verification remains required after GA selection.

## 9. Frequency-domain gate

Use FFT/PSD/cross-spectrum/coherence and MIMO FRF before final gain freeze.

Require assessment of:

```text
useful bandwidth
resonance bands
phase/gain margins
sensitivity peak M_s
N/E/D frequency coupling
high-frequency command energy
INDI/filter phase delay
```

GA must not be rewarded for reducing time-domain error by exciting measured resonance/high-frequency regions.

## 10. Scenario groups

### S0 Signed nominal tracking

All 8 signed endpoint combinations.

### S1 Axis reversals

N, E and D isolated sign reversals.

### S2 Coupled reversals

Full-vector and horizontal-vector reversals.

### S3 Disturbance

0–15 N horizontal forces with head/tail/cross/diagonal direction relative to each signed horizontal command.

### S4 Model uncertainty

```text
mass/payload
control effectiveness
actuator lag/bandwidth
drag/damping
sensor/filter variation
delay
N/E/D coupling
```

### S5 Constraints

```text
saturation
headroom reduction
tilt-limit approach
15 N wind recovery
high-acceleration reversals
```

### S6 Yaw invariance

Equivalent NED objectives at multiple yaw headings.

## 11. Decision rule

Let `P*` be the simplest PID-centered arm on the Pareto frontier.

`PID_REPLACEMENT_CANDIDATE` requires:

```text
P* accuracy >= C_FAST over signed NED envelope
P* disturbance rejection >= C_FAST for 0–15 N horizontal wind
P* p99 T_accept < C_FAST
P* engineering/runtime complexity materially lower
no safety/authority regression
yaw-invariance gate PASS
all signed-axis acceptance tests PASS
```

Otherwise classify as mission-specific alternative or simplicity baseline only.

## 12. Authority

```text
ACTIVE_SIM_MATRIX_VERSION=v4_SIGNED_NED
ALL_AXIS_SIGNS_REQUIRED=true
VELOCITY_MAGNITUDES_NED=[3.0,2.5,0.8]_mps
WIND_FORCE_RANGE_HORIZONTAL=0_TO_15_N
MEASURED_NO_DRIFT_REFERENCE_LIMIT=20_N
BATTERY_MODELING_REQUIRED=false
YAW_MODEL_FEATURE=false
CANONICAL_PIPELINE_REPLACEMENT=false
PRODUCTION_AUTHORITY=false
CURRENT_PHASE0_GATE_UNCHANGED=true
```
