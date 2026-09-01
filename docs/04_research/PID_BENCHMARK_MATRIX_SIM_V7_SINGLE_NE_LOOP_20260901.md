# PID Benchmark Matrix — Simulation v7 — Single N/E Velocity Loop — 2026-09-01

Branch: `research/pid-benchmark-20260901`

This matrix supersedes the earlier PID simulation matrices for the next implementation stage.

## 1. Core architecture

```text
MISSION / N-E PATH
      ↓
bounded path / cross-track guidance
      ↓
N/E velocity setpoint
      ↓
ONE external N/E velocity controller
      ↓
a_N / a_E setpoint
      ↓
PX4 mixed-axis boundary
      ├─ PX4 D-axis altitude controller ACTIVE
      ├─ PX4 acceleration→thrust/attitude ACTIVE
      ├─ PX4 attitude controller ACTIVE
      ├─ PX4 rate controller ACTIVE
      └─ PX4 control allocation ACTIVE
      ↓
vehicle
```

Hard invariant:

```text
PX4_NE_POSITION_CONTROLLER = BYPASSED
PX4_NE_VELOCITY_CONTROLLER = BYPASSED
DOUBLE_NE_VELOCITY_PID = PROHIBITED
```

## 2. NED mission envelope

Primary control dimensions:

\[
v_N\in[-3,+3]\;m/s
\]

\[
v_E\in[-2.5,+2.5]\;m/s.
\]

D-axis is not a primary model/control dimension. PX4 holds altitude and D-axis quantities are safety/coupling monitors.

Yaw is not a model feature. Primary campaign uses a fixed explicit yaw reference.

## 3. PX4-facing setpoint contract

When an experimental N/E controller is active:

```text
position_N     = NaN
position_E     = NaN
velocity_N     = NaN
velocity_E     = NaN
acceleration_N = finite external command
acceleration_E = finite external command
```

Preferred D-axis contract:

```text
position_D     = fixed altitude/D reference
velocity_D     = NaN unless exact runtime semantics require otherwise
acceleration_D = NaN external
```

Forbidden:

```text
finite velocity_N/E
+
finite external PID acceleration_N/E
```

because that would retain PX4 N/E velocity feedback while adding another external N/E controller.

## 4. Experimental plant

Preferred identification target:

\[
G_{NE}:[a_N,a_E]\rightarrow[v_N,v_E].
\]

Initial model family:

```text
2×2 MIMO FRF
→ high-order 2×2 VARX
→ PBSID/subspace/SVD reduction
→ compact state-space / local LPV family
```

D-axis, tilt and headroom are retained as validation/safety channels and may be promoted to scheduling/model variables only if held-out evidence shows material explanatory value.

## 5. G0 — exact-local mixed-axis smoke

Status before runtime:

```text
PUBLIC_PX4_V1_15_4_SOURCE_SUPPORT=YES
EXACT_LOCAL_RUNTIME_QUALIFICATION=NOT_RUN
```

### G0-A external acceleration smoke

Run a small bounded N/E acceleration excitation while altitude and yaw are fixed.

Require:

```text
accepted external N/E acceleration identity
N/E position setpoint inactive
N/E velocity setpoint inactive
no PX4 N/E velocity PID contribution
no hidden N/E integrator accumulation
PX4 D-axis control active
attitude/rate/allocation active
expected vehicle N/E response
```

### G0-B forensic native PX4 velocity case

Run separately for comparison only.

### G0 acceptance

```text
G0_PASS only if the runtime boundary is causally proven to contain one and only one N/E velocity loop.
```

## 6. Path/cross-track guidance

Path tracking is mandatory because velocity recovery alone does not guarantee return to the original path after a gust.

Preferred first implementation:

\[
e_\perp=(I-tt^T)(p-p_{line})
\]

\[
v_{ct}=-v_{ct,max}\tanh(e_\perp/e_0)
\]

\[
v_{sp}=v_{nominal}+v_{ct}.
\]

The guidance block must remain bounded and is not a second velocity PID.

## 7. Wind / disturbance campaigns

### W-1 CALM

Zero wind / zero direct force.

Must cover nominal N/E tracking, entries, reversals, diagonals and path recovery.

### W0 DIRECT_FORCE

Horizontal external force in NED:

```text
0, 3, 6, 9, 12, 15 N
```

Directions:

```text
±N
±E
NE / NW / SE / SW
headwind / tailwind / crosswind relative to commanded path
```

Previously measured `20 N` remains only a reference force limit, not a guaranteed moving-flight safety margin.

### W1 STEADY_AERODYNAMIC_WIND

Constant aerodynamic wind with relative-airflow effects.

### W2 GUST

Step and smooth gust pulses.

### W3 CHANGE_CLEAR

Wind increase/decrease/clear sequences to expose integral windup, estimator persistence and opposite-side overshoot.

### W4 DIRECTION_CHANGE

Wind direction switches/rotations to expose N/E coupling.

### W5 TURBULENCE

Dryden or equivalent validated stochastic turbulence for broadband robustness.

### W7 MANEUVER_PLUS_WIND

Held-out stress tests where wind occurs during velocity entry, reversal or other high-authority maneuvers.

W6 shear and W8 vertical wind are later robustness extensions.

## 8. G2 — 0–15 N moving-flight feasibility

G2 remains the highest-priority empirical gate.

Initial operating points:

```text
v_N ∈ {-3, 0, +3} m/s
v_E ∈ {-2.5, 0, +2.5} m/s
```

At each point, increase disturbance through the declared force ladder and refine near the pass/fail boundary only when needed.

Mandatory feasibility observables:

```text
peak tilt
thrust
motor/control-allocation headroom
saturation count/duration
D-axis position error
v_D drift
N/E cross-track error
N/E velocity error
recovery time
```

`15 N` is not declared feasible until these metrics pass the frozen acceptance criteria across the declared operating subset.

## 9. Identification excitation

Preferred excitation:

```text
orthogonal N/E multisine
```

Analysis:

```text
FFT
PSD / cross-spectrum
coherence
MIMO FRF
closed-loop VARX/PBSID
residual whiteness
input-residual correlation
held-out free-run validation
```

Treat the dataset conservatively as closed-loop identification data whenever a holding/reference controller influences the accepted excitation.

## 10. Controller arms

### Baselines

```text
C0 exact PX4/native baseline
C_FAST exact current qualified AURA + FAST/T1/C1 comparator at benchmark time
```

### PID family

```text
P0 fixed 2DOF PID
P1 robust-optimized 2DOF PID
P2 gain-scheduled 2DOF PID
P3 P2 + feedforward
P4a P3 + classical INDI
P4b P3 + actuator/control-effectiveness-aware INDI
```

### Challengers

```text
H0 low-order structured H-infinity
H1 low-order structured H-infinity + INDI
E0 2DOF PID + ESO/DOB
```

All arms must use the exact same single-loop N/E PX4 boundary.

## 11. Controller interaction rule

INDI is not a second velocity PID. It may only provide a bounded incremental correction at the acceleration/virtual-control level.

Require ablation:

```text
P3 vs P4a vs P4b
```

and spectral analysis of PID/INDI command contributions so duplicate/high-frequency fighting is detectable.

## 12. Frequency-domain requirements

For validated linear/local models report:

```text
MIMO FRF
phase margin / gain margin where meaningful
maximum sensitivity M_s
closed-loop bandwidth
disturbance transfer T_w→v
command high-frequency energy
resonance-band energy
```

Frequency-domain constraints complement, not replace, Lyapunov/state-space stability checks.

## 13. Stability / constraint requirements

Candidate controllers must pass, as applicable:

```text
local/LPV closed-loop eigenvalue checks
Lyapunov certificate attempts
robust plant-ensemble simulation
hard tilt/thrust/headroom constraints
anti-windup
bumpless gain transition
gain-rate limits
no pathological saturation
```

Failure to find one common quadratic Lyapunov function is not alone proof of instability; use appropriately scoped local/parameter-dependent and robust evidence.

## 14. CALM non-degradation gate

A wind-focused controller cannot be promoted if it materially degrades zero-wind behavior.

Mandatory CALM metrics:

```text
velocity RMSE N/E
position/path cross-track error
overshoot
settling time
jerk
TV(command)
control effort
saturation
p99 accepted-action latency
```

## 15. Wind improvement gate

A candidate is not a wind improvement merely because average RMSE falls.

Require held-out improvement or declared non-inferiority across the frozen mission criteria, while preserving hard safety constraints.

Report at least:

```text
peak cross-track deviation
peak velocity error N/E
onset→recovery time
clear→recovery time
integrated disturbance error
jerk
control effort
saturation
headroom
vertical drift / altitude error
p99 T_accept
```

## 16. Decision outcomes

```text
LIGHTWEIGHT_REPLACEMENT_SUPPORTED
MISSION_SPECIFIC_ALTERNATIVE
CURRENT_PIPELINE_JUSTIFIED
```

Stop rule:

If the bounded PID/INDI/H-infinity/ESO challenger set does not produce a Pareto improvement on held-out CALM + wind campaigns, do not keep adding ANN/MPC/RL layers merely to force a win. Retain the result as evidence for the limits of the lightweight branch.

## 17. Current readiness

```text
SINGLE_NE_VELOCITY_LOOP=FROZEN_FOR_G0
G0_EXACT_LOCAL_MIXED_AXIS_SMOKE=NOT_RUN
G1_PATH_GUIDANCE=READY_FOR_IMPLEMENTATION
G2_0_TO_15N_MOVING_FEASIBILITY=UNRESOLVED
G3_2X2_MIMO_ID=METHODOLOGY_READY
G4_PID_INDI=CONDITIONAL_ABLATION_REQUIRED
FULL_CONTROLLER_BUILD=BLOCKED_ON_G0_AND_G2
MAIN_CANONICAL_PIPELINE_CHANGED=false
PRODUCTION_AUTHORITY=false
```
