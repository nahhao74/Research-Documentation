# PID Benchmark Matrix — Simulation v2 — 2026-09-01

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

## 2. Active scheduling state

Start:

\[
\rho_0=[v_x,v_y,|v|]
\]

Promote only when held-out evidence requires it:

```text
acceleration / desired acceleration
tilt / attitude
motor/control-allocation headroom
control-effectiveness estimate
```

Current simulation rule:

```text
BATTERY_SCHEDULING_VARIABLE=false
```

## 3. Scenario groups

### S0 Nominal

```text
hover
constant velocities
N/E/diagonal motion
```

### S1 Reference transients

```text
steps
reversals
ramps
sine
chirp
```

### S2 Disturbance

```text
GUST ±N/±E
continuous wind
wind onset/change/clear
crosswind during reference transition
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
aggressive reversal
```

## 4. Metrics

### Accuracy

```text
velocity_RMSE
position_RMSE
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
peak_deviation
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

## 5. Fairness invariants

```text
same vehicle/model
same PX4 base identity
same initial-condition distribution
same trajectory
same disturbance realization
same sensor data availability
same control constraints
same timing source
same authority boundary
same safety envelope
```

No arm may receive future/native disturbance truth unavailable to its comparator.

## 6. Tuning partition

```text
IDENTIFICATION
GA_TUNING / DEVELOPMENT
HELD_OUT_VALIDATION
FINAL_BENCHMARK
```

No tuning on FINAL_BENCHMARK data.

## 7. Model validation gate

Before GA tuning require:

```text
held-out prediction/residual check
closed-loop identification audit
source/action timing audit
N/E coupling assessment
delay estimate
actuator-dynamics assessment
```

If local SISO is inadequate, promote to MIMO instead of compensating with aggressive PID gains.

## 8. PID implementation gate

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
```

## 9. GA acceptance

Require:

```text
stable across plant ensemble
held-out trajectory PASS
held-out disturbance PASS
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
```

## 10. Scheduler acceptance

```text
GS0 lookup
→ GS1 interpolation
→ GS2 polynomial/spline
→ GS3 LPV-qualified
```

ANN/RBF only after evidence that GS0–GS3 are insufficient.

## 11. INDI acceptance

P4 is retained over P3 only if disturbance rejection improves materially without unacceptable penalties in:

```text
noise sensitivity
control effort
latency
headroom
complexity
```

## 12. DOB acceptance

P5 is allowed only if P4 leaves a repeatable residual disturbance mode.

## 13. ANN acceptance

P6 must beat simple scheduling on held-out operating regions and pass:

```text
gain bounds
gain-rate bounds
OOD fallback
p99 latency
stability/constraint behavior
material gain-map approximation benefit
```

## 14. Current-pipeline decision

Let `P*` be the simplest PID arm on the Pareto frontier.

### PID_REPLACEMENT_CANDIDATE

Require:

```text
P* accuracy >= C_FAST
P* disturbance rejection >= C_FAST over declared simulation envelope
P* p99 T_accept < C_FAST
P* engineering/runtime complexity materially lower
no authority/safety regression
```

### PID_MISSION_SPECIFIC_ALTERNATIVE

Use if P* strongly wins latency/complexity but loses a robustness dimension outside the narrower declared mission envelope.

### PID_SIMPLICITY_BASELINE_ONLY

Use if the current pipeline materially wins disturbance/OOD behavior.

## 15. Expected champion hypothesis

```text
EXPECTED_CHAMPION_BEFORE_DATA=P4_RGS_2DOF_PID_INDI
```

Hypothesis only.

## 16. Authority

```text
BENCHMARK_BRANCH_ONLY=true
SIMULATION_SCOPE=true
BATTERY_MODELING_REQUIRED=false
CANONICAL_PIPELINE_REPLACEMENT=false
PRODUCTION_AUTHORITY=false
CURRENT_PHASE0_GATE_UNCHANGED=true
```
