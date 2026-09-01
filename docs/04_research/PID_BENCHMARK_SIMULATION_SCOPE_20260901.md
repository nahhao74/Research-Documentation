# PID Benchmark — Simulation Scope Override — 2026-09-01

This file narrows the active scope of `research/pid-benchmark-20260901` for the current simulation-only phase.

## Active rule

```text
CURRENT_ENVIRONMENT=SIMULATION
COORDINATE_FRAME=NED
NED_X=NORTH
NED_Y=EAST
NED_Z=DOWN

BATTERY_MODELING_REQUIRED=false
BATTERY_SCHEDULING_VARIABLE=false
BATTERY_UNCERTAINTY_SWEEP=false
BATTERY_BENCHMARK_SCENARIO=false
```

Battery-related variables mentioned in the parent research note are **deferred real-flight variables**, not active simulation inputs.

## Fixed benchmark velocity setpoint

All model, identification, tuning, runtime, and benchmark quantities must use NED semantics.

```text
v_N_sp = +3.0 m/s
v_E_sp = +2.5 m/s
v_D_sp = +0.8 m/s
```

Because NED uses positive Down, `v_D_sp=+0.8 m/s` means a downward velocity of 0.8 m/s. If a later mission intends upward 0.8 m/s, the correct NED command is `v_D_sp=-0.8 m/s`; do not silently reinterpret the sign.

The nominal horizontal path direction is defined by `[v_N_sp, v_E_sp]`. Path/cross-track metrics must be evaluated in NED rather than a body-fixed frame.

## Wind-force benchmark contract

Represent external wind/disturbance force as:

\[
F_w^{NED}=[F_N,F_E,F_D]^T
\]

Primary simulation wind benchmark:

```text
0 N <= ||F_w,NE|| <= 15 N
F_D = 0 for the primary horizontal-wind campaign
```

The previously measured no-drift force limit is retained as a reference ceiling:

```text
MEASURED_NO_DRIFT_REFERENCE_LIMIT = 20 N
ACTIVE_BENCHMARK_MAX_WIND_FORCE = 15 N
NOMINAL_FORCE_RESERVE = 5 N
NOMINAL_FORCE_RESERVE_FRACTION = 25 percent of the measured 20 N reference
```

The 20 N value is a physical/reference ceiling from prior measurement, **not** an active tuning target. The benchmark must not tune directly at 20 N and then claim 0–15 N robustness from interpolation alone.

Recommended disturbance magnitudes:

```text
0, 3, 6, 9, 12, 15 N
```

Recommended horizontal directions in NED:

```text
+N
-N
+E
-E
NE diagonal
NW diagonal
SE diagonal
SW diagonal
headwind relative to nominal horizontal path
tailwind relative to nominal horizontal path
crosswind left/right relative to nominal horizontal path
```

Every compared arm must receive the exact same force realization and source-time schedule.

## Yaw exclusion contract

Yaw is not part of the benchmark control objective and must not be used as a scheduling variable, model feature, or tuning dimension.

```text
YAW_OBJECTIVE=false
YAW_SCHEDULING_VARIABLE=false
YAW_MODEL_FEATURE=false
```

Preferred implementation:

```text
use NED-native position, velocity, acceleration, setpoint, and accepted-action quantities directly
```

If any required measurement/action exists only in a body-fixed frame, a coordinate transformation to NED may use the full attitude including yaw **only as a coordinate transformation**. The transformed NED signal is the model input/output; yaw itself is not exposed as a predictive feature.

Yaw exclusion is accepted only after a yaw-invariance audit:

```text
repeat equivalent NED trajectories/forces under multiple yaw headings
compare NED identified dynamics / residuals / controller response
check residual correlation with yaw
check N/E coupling versus yaw heading
```

Acceptance rule:

```text
if yaw changes materially alter the NED model or tracking metrics,
yaw cannot be declared irrelevant;
first audit frame conversion / aerodynamics / actuator coupling,
and only then decide whether model structure must change.
```

Do not hide real yaw-induced coupling by simply omitting yaw from the dataset.

## Active scheduling vector

Start with the smallest NED scheduling state:

\[
\rho_0=[v_N,v_E,\sqrt{v_N^2+v_E^2}]
\]

Do not schedule on yaw.

Only promote dimensions when held-out evidence shows a material dynamics variation that the smaller schedule cannot represent.

Candidate promoted variables, in preferred order, are:

```text
NED acceleration / desired acceleration
tilt magnitude or qualified attitude-derived quantity
motor/control-allocation headroom
control-effectiveness estimate
```

Battery and yaw are excluded from the current promotion ladder.

## Active identified-model uncertainty ensemble

For each operating region, model uncertainty should cover only variables relevant in simulation:

```text
identification uncertainty
mass / payload variation
thrust / control-effectiveness variation
aerodynamic drag / damping variation
actuator bandwidth / lag
transport/controller delay
sensor/filter variation
N/E coupling
```

Do not add battery-voltage or battery-state variation to the current ensemble.

## Active scenario matrix

### Nominal

```text
NED velocity hold at [3.0, 2.5, 0.8] m/s
NED path hold at the same commanded velocity vector
reference entry into the commanded velocity
reference recovery back to the commanded velocity after disturbance
```

### Disturbance

```text
horizontal NED wind force 0–15 N
GUST ±N / ±E
diagonal horizontal gusts
headwind / tailwind / crosswind relative to commanded NED path
continuous wind
wind onset/change/clear
wind during reference transition
```

### Model variation

```text
payload / mass variation
thrust/control-effectiveness reduction
actuator bandwidth variation
aerodynamic drag variation
sensor noise/filter variation
```

### Timing

```text
callback/executor delay injection
transport delay variation
source-age variation
```

### Constraint region

```text
saturation
motor/control-allocation headroom reduction
tilt-limit approach
recovery under 15 N wind
```

## Why battery is excluded

In the current simulator, battery is not needed to answer the main benchmark question:

> Can the PID-centered branch match or beat the current AURA/FAST/T1/C1 path on NED tracking accuracy, disturbance rejection under 0–15 N wind force, accepted-action latency, runtime, and implementation complexity?

Keeping a battery dimension would enlarge the identification and gain-scheduling state space without adding useful discriminatory power unless the simulator explicitly models battery-dependent thrust/control effectiveness.

If a later simulation introduces a validated battery-to-thrust/actuator model, battery may be reintroduced only if an ablation shows that direct battery scheduling explains dynamics variation not already captured by measured control effectiveness or motor headroom.

## Real-flight future

Battery becomes relevant again for hardware flight because voltage sag and state-of-charge may change actuator authority and thrust response.

Preferred real-flight policy:

```text
first measure whether battery materially changes control effectiveness
then decide whether to schedule directly on battery
or indirectly on measured thrust/control effectiveness / motor headroom
```

Direct battery scheduling is not automatically preferred.

## Authority

```text
SIMULATION_SCOPE_OVERRIDE=true
COORDINATE_FRAME=NED
WIND_FORCE_BENCHMARK_MAX=15N
MEASURED_NO_DRIFT_REFERENCE_LIMIT=20N
YAW_EXCLUDED_FROM_MODEL=true
CANONICAL_PIPELINE_CHANGED=false
CURRENT_PHASE0_GATE_UNCHANGED=true
PRODUCTION_AUTHORITY=false
```
