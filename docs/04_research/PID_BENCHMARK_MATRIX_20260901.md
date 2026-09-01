# PID Benchmark Matrix — 2026-09-01

Companion to `PID_BENCHMARK_BRANCH_20260901.md` on branch `research/pid-benchmark-20260901`.

## 1. Arms

```text
P0 FIXED-2DOF-PID
   robust globally tuned 2-DOF PID

P1 GA-LOCAL-2DOF-PID
   local robust GA gains; no runtime interpolation claim yet

P2 RGS-2DOF-PID
   bounded gain scheduling + bumpless transfer

P3 RGS-2DOF-PID-FF
   P2 + bounded model/physics feedforward

P4 RGS-2DOF-PID-INDI
   P3 + measured-response incremental correction

P5 RGS-2DOF-PID-INDI-DOB
   P4 + low-order observer only if P4 leaves a measured disturbance gap

P6 RGS-2DOF-PID-INDI-NN-SCHED
   P4/P5 + ANN/RBF gain-surface compression only if simple scheduling is inadequate

C_FAST
   exact current qualified AURA + FAST/T1/C1 revision at comparison time

C_FULL
   future qualified full predictive pipeline revision; NOT assumed available now
```

## 2. Mandatory ablation order

```text
P0 → P1 → P2 → P3 → P4
```

P5 and P6 are conditional.

Do not skip directly to P6.

## 3. Scenario groups

### S0 Nominal

```text
hover
constant velocities
N/E/diagonal
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
payload
battery/thrust effectiveness
actuator bandwidth
sensor noise
```

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
T_compute   source → controller output
T_accept    source → qualified PX4 accepted boundary
T_effect    physical event → first useful correction
T_recover   physical event → recovery threshold
```

For all latency metrics:

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

## 5. Required fairness invariants

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

No benchmark arm may receive native future disturbance truth unavailable to the comparator.

## 6. Tuning partition

Never GA-tune on the same sessions used for final comparison.

Use:

```text
IDENTIFICATION
GA_TUNING / DEVELOPMENT
HELD_OUT_VALIDATION
FINAL_BENCHMARK
```

A gain table selected using FINAL_BENCHMARK data invalidates the comparison.

## 7. Model validation gate

Before GA tuning, each local/LPV model must pass:

```text
held-out prediction/residual check
closed-loop identification audit
source/action timing audit
N/E coupling assessment
delay estimate
actuator-dynamics assessment
```

If a local SISO model cannot represent cross-axis behavior adequately, promote to MIMO rather than compensating with more aggressive PID gains.

## 8. PID implementation gate

Before flight/runtime comparison:

```text
2-DOF weighting frozen
D-on-measurement behavior verified
derivative filter frozen
anti-windup frozen
output limits frozen
integrator behavior verified
bumpless gain transition PASS
gain-rate limits PASS
no dynamic allocation in update loop
```

## 9. GA acceptance

A GA result is not accepted only because it has the smallest simulated objective.

Require:

```text
stable across plant ensemble
held-out trajectory PASS
held-out disturbance PASS
constraint/saturation PASS
no pathological gain discontinuity
repeatable optimization / retained seed provenance
```

## 10. Scheduler acceptance

```text
GS0 lookup
→ GS1 interpolation
→ GS2 polynomial/spline
→ GS3 LPV-qualified
```

Promote complexity only when the lower rung fails a measured criterion.

ANN/RBF scheduler is eligible only after GS0–GS3 evidence shows a real representation gap.

## 11. INDI acceptance

P4 is retained over P3 only if it improves at least one disturbance metric materially without unacceptable penalties in:

```text
noise sensitivity
control effort
latency
headroom
complexity
```

## 12. DOB acceptance

P5 is allowed only if P4 leaves a repeatable residual disturbance mode.

If P5 merely improves nominal plots while adding filtering delay or duplicated disturbance estimation, reject P5.

## 13. ANN acceptance

P6 must beat the simple schedule on held-out operating regions.

Require:

```text
gain bound PASS
gain-rate bound PASS
OOD fallback PASS
no worse p99 latency
no worse stability/constraint behavior
material gain-map approximation benefit
```

## 14. Current-pipeline benchmark decision

Let `P*` be the simplest PID arm on the Pareto frontier.

### PID_REPLACEMENT_CANDIDATE

Require:

```text
P* accuracy >= C_FAST
P* disturbance rejection >= C_FAST over declared envelope
P* p99 T_accept < C_FAST
P* engineering/runtime complexity materially lower
no authority/safety regression
```

### PID_MISSION_SPECIFIC_ALTERNATIVE

Use when:

```text
P* slightly loses one robustness dimension
but strongly wins latency/complexity for a narrower mission envelope
```

### PID_SIMPLICITY_BASELINE_ONLY

Use when current pipeline materially wins disturbance/OOD performance.

## 15. Expected first champion hypothesis

```text
EXPECTED_CHAMPION_BEFORE_DATA=P4_RGS_2DOF_PID_INDI
```

This is a hypothesis only.

## 16. Authority

```text
BENCHMARK_BRANCH_ONLY=true
CANONICAL_PIPELINE_REPLACEMENT=false
PRODUCTION_AUTHORITY=false
CURRENT_PHASE0_GATE_UNCHANGED=true
```
