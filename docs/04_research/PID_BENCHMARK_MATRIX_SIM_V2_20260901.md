# PID Benchmark Matrix — Simulation v3 — 2026-09-01

Companion to `PID_BENCHMARK_BRANCH_20260901.md` and `PID_BENCHMARK_SIMULATION_SCOPE_20260901.md` on branch `research/pid-benchmark-20260901`.

This is the active benchmark matrix for the current simulation-only phase.

## 1. Arms

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

## 2. Coordinate and mission contract

All benchmark models, controller quantities, logs, metrics and disturbance vectors use NED:

```text
x = North
 y = East
 z = Down
```

Fixed velocity setpoint:

```text
v_N_sp = +3.0 m/s
v_E_sp = +2.5 m/s
v_D_sp = +0.8 m/s
```

`v_D_sp=+0.8 m/s` is downward in NED.

Yaw contract:

```text
YAW_OBJECTIVE=false
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
```

Yaw may be used only as part of a required body→NED coordinate transform. Yaw-independence must pass a dedicated invariance audit before yaw is declared irrelevant.

## 3. Active scheduling state

Start:

\[
\rho_0=[v_N,v_E,\sqrt{v_N^2+v_E^2}]
\]

Promote only when held-out evidence requires it:

```text
NED acceleration / desired acceleration
tilt magnitude
motor/control-allocation headroom
control-effectiveness estimate
```

Current simulation rules:

```text
BATTERY_SCHEDULING_VARIABLE=false
YAW_SCHEDULING_VARIABLE=false
```

## 4. Wind-force contract

Wind/external disturbance is represented in NED:

\[
F_w^{NED}=[F_N,F_E,F_D]^T
\]

Primary campaign:

```text
0 N <= ||F_w,NE|| <= 15 N
F_D = 0
```

Reference physical limit:

```text
MEASURED_NO_DRIFT_REFERENCE_LIMIT = 20 N
ACTIVE_BENCHMARK_MAX = 15 N
REFERENCE_FORCE_MARGIN = 5 N
REFERENCE_FORCE_MARGIN_FRACTION = 25 percent
```

Recommended force levels:

```text
0, 3, 6, 9, 12, 15 N
```

Directions:

```text
+N
-N
+E
-E
NE
NW
SE
SW
headwind relative to commanded horizontal NED path
tailwind relative to commanded horizontal NED path
left/right crosswind relative to commanded horizontal NED path
```

The 20 N measurement is a reference ceiling, not a tuning point and not an authorization to test beyond the declared 15 N benchmark envelope.

## 5. Scenario groups

### S0 Nominal NED tracking

```text
hold [3.0, 2.5, 0.8] m/s NED
path tracking at that commanded NED velocity
steady-state tracking
entry to the commanded velocity
recovery to the commanded velocity
```

### S1 Reference transients

```text
bounded entry trajectory toward [3.0, 2.5, 0.8] m/s
bounded recovery after disturbance
ramps / sine / chirp used only where required for identification/FRF
```

### S2 Disturbance

```text
horizontal NED force 0–15 N
GUST ±N/±E
diagonal gusts
headwind
tailwind
crosswind
continuous wind
wind onset/change/clear
wind during reference transition
```

### S3 Model uncertainty

```text
payload / mass variation
thrust/control-effectiveness variation
actuator bandwidth / lag
aerodynamic drag variation
sensor noise/filter variation
N/E coupling
```

Battery variation is excluded in the current simulation phase.

### S4 Timing

```text
callback/executor delay injection
transport delay variation
source-age variation
```

### S5 Constraints

```text
saturation
motor/control-allocation headroom reduction
tilt-limit approach
15 N wind recovery
```

### S6 Yaw-invariance audit

Run equivalent NED setpoint/disturbance cases under multiple yaw headings while preserving the same NED physical objective.

Require:

```text
same NED command semantics
same NED force semantics
no material yaw-dependent shift in identified NED dynamics
no material yaw-dependent shift in NED tracking metrics
no unexplained residual correlation with yaw
```

If this gate fails, do not hide the effect by omitting yaw; audit coordinate conversion, vehicle/aerodynamic anisotropy and actuator coupling first.

## 6. Metrics

### NED tracking accuracy

```text
velocity_RMSE_N
velocity_RMSE_E
velocity_RMSE_D
position_RMSE_N
position_RMSE_E
position_RMSE_D
horizontal_cross_track_error
along_track_velocity_error
IAE
ITAE
peak_error
overshoot
settling_time
endpoint_error
```

### Disturbance rejection

```text
onset_to_peak_deviation
peak_cross_track_deviation
peak_velocity_error_NED
onset_to_recovery
post_event_residual
integrated_disturbance_error
```

### Control quality

```text
control_effort
TV_command
jerk
saturation_count
saturation_duration
headroom_min
peak_tilt
```

### Latency

```text
T_compute source → controller output
T_accept  source → qualified PX4 accepted boundary
T_effect  physical event → first useful correction
T_recover physical event → recovery threshold
```

For each latency metric record:

```text
p50
p95
p99
max
jitter
```

### Runtime

```text
CPU/update
CPU total
memory
allocations
callbacks
DDS traffic
deadline misses
max source age
```

### Complexity

```text
LOC
runtime modules
state machines
tunable parameters
callback boundaries
branch complexity
failure modes
forensic/debug time
```

## 7. Fairness invariants

```text
same NED coordinate semantics
same vehicle/model
same PX4 base identity
same initial-condition distribution
same NED velocity target [3.0, 2.5, 0.8] m/s
same NED wind-force realization
same sensor data availability
same control constraints
same timing source
same authority boundary
same safety envelope
```

No arm may receive future/native disturbance truth unavailable to its comparator.

## 8. Tuning partition

```text
IDENTIFICATION
GA_TUNING / DEVELOPMENT
HELD_OUT_VALIDATION
FINAL_BENCHMARK
```

No tuning on FINAL_BENCHMARK data.

Wind-force levels/directions must be partitioned so the final benchmark contains held-out combinations, not only repetitions of the exact GA-development cases.

## 9. Model validation gate

Before GA tuning require:

```text
held-out prediction/residual check
closed-loop identification audit
source/action timing audit
N/E coupling assessment
D-axis assessment
delay estimate
actuator-dynamics assessment
yaw-invariance audit or explicit pending status
```

If local SISO is inadequate, promote to MIMO instead of compensating with aggressive PID gains.

## 10. PID implementation gate

```text
2-DOF weighting frozen
D-on-measurement verified
derivative filter frozen
anti-windup frozen
output limits frozen
integrator behavior verified
bumpless gain transition PASS
gain-rate limits PASS
no dynamic allocation in update loop
NED command sign/unit audit PASS
```

## 11. GA acceptance

Require:

```text
stable across plant ensemble
held-out NED trajectory PASS
held-out 0–15 N disturbance PASS
constraint/saturation PASS
no pathological gain discontinuity
repeatable optimization and retained seed provenance
```

The simulation plant ensemble excludes battery and includes:

```text
ID uncertainty
mass/payload
control effectiveness
actuator dynamics
drag
delay
sensor/filter variation
N/E coupling
```

GA must not be rewarded for violating force/tilt/thrust/headroom constraints to reduce tracking error.

## 12. Scheduler acceptance

```text
GS0 lookup
→ GS1 interpolation
→ GS2 polynomial/spline
→ GS3 LPV-qualified
```

ANN/RBF only after evidence that GS0–GS3 are insufficient.

## 13. INDI acceptance

P4 is retained over P3 only if disturbance rejection improves materially over the 0–15 N NED force envelope without unacceptable penalties in:

```text
noise sensitivity
control effort
latency
headroom
peak tilt
complexity
```

## 14. DOB acceptance

P5 is allowed only if P4 leaves a repeatable residual disturbance mode.

## 15. ANN acceptance

P6 must beat simple scheduling on held-out operating regions and pass:

```text
gain bounds
gain-rate bounds
OOD fallback
p99 latency
stability/constraint behavior
material gain-map approximation benefit
```

## 16. Current-pipeline decision

Let `P*` be the simplest PID arm on the Pareto frontier.

### PID_REPLACEMENT_CANDIDATE

Require:

```text
P* accuracy >= C_FAST
P* disturbance rejection >= C_FAST over declared 0–15 N NED envelope
P* p99 T_accept < C_FAST
P* engineering/runtime complexity materially lower
no authority/safety regression
yaw-invariance gate PASS
```

### PID_MISSION_SPECIFIC_ALTERNATIVE

Use if P* strongly wins latency/complexity but loses a robustness dimension outside the narrower declared mission envelope.

### PID_SIMPLICITY_BASELINE_ONLY

Use if the current pipeline materially wins disturbance/OOD behavior.

## 17. Expected champion hypothesis

```text
EXPECTED_CHAMPION_BEFORE_DATA=P4_RGS_2DOF_PID_INDI
```

Hypothesis only.

## 18. Authority

```text
BENCHMARK_BRANCH_ONLY=true
SIMULATION_SCOPE=true
COORDINATE_FRAME=NED
VELOCITY_SETPOINT_NED=[3.0,2.5,0.8]_mps
WIND_FORCE_RANGE_NED_HORIZONTAL=0_TO_15_N
MEASURED_NO_DRIFT_REFERENCE_LIMIT=20_N
BATTERY_MODELING_REQUIRED=false
YAW_MODEL_FEATURE=false
CANONICAL_PIPELINE_REPLACEMENT=false
PRODUCTION_AUTHORITY=false
CURRENT_PHASE0_GATE_UNCHANGED=true
```
