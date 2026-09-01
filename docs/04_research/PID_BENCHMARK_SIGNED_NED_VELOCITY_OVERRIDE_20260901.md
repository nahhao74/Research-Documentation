# PID Benchmark — Signed NED Velocity Override — 2026-09-01

This file supersedes the earlier positive-only velocity examples in the PID simulation benchmark branch.

## Active velocity contract

All commanded/modelled velocities are expressed only in NED:

```text
x = North
 y = East
 z = Down
```

The benchmark velocity envelope is signed on all three axes:

```text
v_N_sp ∈ {-3.0, +3.0} m/s
v_E_sp ∈ {-2.5, +2.5} m/s
v_D_sp ∈ {-0.8, +0.8} m/s
```

Equivalently:

\[
v_N \in [-3,3]\;m/s,\qquad
v_E \in [-2.5,2.5]\;m/s,\qquad
v_D \in [-0.8,0.8]\;m/s
\]

with the benchmark endpoints above used as the primary signed setpoints.

NED sign semantics are mandatory:

```text
+v_N = North
-v_N = South
+v_E = East
-v_E = West
+v_D = Down
-v_D = Up
```

No ENU/body-frame reinterpretation is allowed inside identification, tuning, scheduling, controller scoring, or benchmark metrics.

## Required signed combinations

The benchmark must not validate only one octant.

At minimum include all 8 endpoint sign combinations:

```text
(+3.0, +2.5, +0.8)
(+3.0, +2.5, -0.8)
(+3.0, -2.5, +0.8)
(+3.0, -2.5, -0.8)
(-3.0, +2.5, +0.8)
(-3.0, +2.5, -0.8)
(-3.0, -2.5, +0.8)
(-3.0, -2.5, -0.8)
```

All components are `[v_N, v_E, v_D]` in m/s.

Also include axis-isolation and sign-reversal cases:

```text
(+3, 0, 0) ↔ (-3, 0, 0)
(0, +2.5, 0) ↔ (0, -2.5, 0)
(0, 0, +0.8) ↔ (0, 0, -0.8)
```

and representative coupled reversals:

```text
(+3,+2.5,+0.8) ↔ (-3,-2.5,-0.8)
(+3,+2.5,-0.8) ↔ (-3,-2.5,+0.8)
```

Reference transitions must be jerk/acceleration limited where the controller architecture requires it; the benchmark must separately preserve true reversal stress tests.

## Identification requirement

MIMO VARX / reduced-state / LPV identification must be trained and validated with signed excitation on N, E and D.

A model that is accurate only for positive velocity is not accepted.

Required audits:

```text
positive-vs-negative N residual symmetry/asymmetry
positive-vs-negative E residual symmetry/asymmetry
positive-vs-negative D residual symmetry/asymmetry
N/E/D cross-axis coupling under sign changes
control-effectiveness sign consistency
actuator/thrust asymmetry, especially +D versus -D
```

Do not force symmetric dynamics if the simulated vehicle exhibits real directional asymmetry.

## Gain-scheduling requirement

The scheduler must cover the signed operating envelope.

Initial scheduling state remains NED-native and may use:

\[
\rho_0=[v_N,v_E,v_D,\|v_{NE}\|]
\]

or a lower-dimensional form only if held-out evidence demonstrates that D-axis scheduling and/or signed-axis dependence is unnecessary.

The scheduler must not collapse `+v` and `-v` merely through absolute velocity unless directional symmetry is empirically established.

## Wind contract remains unchanged

Primary external-force benchmark remains:

```text
0 N <= ||F_w,NE|| <= 15 N
primary F_D = 0
measured no-drift reference limit = 20 N
```

Wind directions must be tested relative to each signed commanded horizontal velocity vector, including headwind, tailwind and both crosswind directions.

## Yaw contract remains unchanged

```text
YAW_OBJECTIVE=false
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
```

Yaw may participate only in a required body→NED coordinate transform. Yaw-invariance must still be demonstrated; yaw-induced physical coupling must not be hidden by omission.

## Authority

```text
SIGNED_NED_VELOCITY_OVERRIDE=true
ACTIVE_PRIMARY_SETPOINT_MAGNITUDES=[3.0,2.5,0.8]_mps
ALL_AXIS_SIGNS_REQUIRED=true
COORDINATE_FRAME=NED
CANONICAL_PIPELINE_CHANGED=false
CURRENT_PHASE0_GATE_UNCHANGED=true
PRODUCTION_AUTHORITY=false
```
