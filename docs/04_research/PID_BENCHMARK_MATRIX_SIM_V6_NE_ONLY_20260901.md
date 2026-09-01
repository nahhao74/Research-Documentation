# PID Benchmark Matrix — Simulation v6 N/E-Only — 2026-09-01

Companion to `PID_NE_ONLY_RESEARCH_CLOSURE_20260901.md` on branch `research/pid-benchmark-20260901`.

This is the active benchmark matrix after N/E-only research closure.

## 1. Primary control scope

```text
COORDINATE_FRAME=NED
PRIMARY_CONTROL_AXES=N,E
PRIMARY_MODEL_AXES=N,E
D_AXIS_PRIMARY_CONTROL=false
D_AXIS_PRIMARY_MODEL=false
D_AXIS_PX4_ALTITUDE_CONTROL=true
D_AXIS_SAFETY_MONITOR=true
YAW_MODEL_FEATURE=false
YAW_PRIMARY_ID_REFERENCE=FIXED
```

Horizontal velocity envelope:

```text
v_N in [-3.0,+3.0] m/s
v_E in [-2.5,+2.5] m/s
```

Signed extrema remain mandatory stress cases, but identification must include intermediate and zero-crossing operating points.

## 2. Controller architecture

```text
N/E path
  ↓
bounded cross-track guidance
  ↓
bounded v_N_sp / v_E_sp
  ↓
2DOF PID N/E
  + feedforward
  + optional INDI by ablation
  ↓
a_N_sp / a_E_sp
  ↓
PX4 mixed-axis acceleration boundary
  ├─ Z altitude hold
  ├─ acceleration→thrust
  ├─ tilt/thrust constraints
  ├─ attitude/rate
  └─ allocation
  ↓
vehicle
```

The path objective and velocity objective are separate. A velocity controller that restores commanded speed but leaves persistent cross-track displacement is not accepted.

## 3. Arms

```text
B0 current exact qualified AURA + FAST/T1/C1 comparator
P0 fixed 2DOF PID
P1 robust-optimized 2DOF PID
P2 gain-scheduled 2DOF PID
P3 P2 + model feedforward
P4 P3 + bounded INDI

H0 compact H-infinity challenger, optional benchmark only
```

Mandatory ablation:

```text
P0 → P1 → P2 → P3 → P4
```

No arm is assumed to win before data.

## 4. G0 — exact PX4 mixed-axis boundary

Before full controller implementation, the exact local PX4 revision must demonstrate:

```text
external a_N / a_E accepted at intended PX4 boundary
PX4 XY velocity PID not reapplying correction on externally supplied XY acceleration
PX4 Z altitude loop remains active
PX4 attitude/rate/allocation remain active
fixed yaw reference remains applied
no hidden setpoint substitution/fallback
source/timestamp provenance retained
```

Status:

```text
G0=SOURCE_SUPPORTED_LOCAL_SMOKE_REQUIRED
```

## 5. Path guidance gate

For N/E path tangent `t`:

\[
e_\perp=(I-tt^T)(p-p_0).
\]

Preferred initial bounded guidance:

\[
v_{ct}=-v_{ct,max}\tanh(e_\perp/e_0),
\]

\[
v_{sp}=v_{parallel}t+v_{ct}.
\]

Hard N/E velocity limits apply after guidance.

Status:

```text
G1=SUPPORTED_READY_FOR_IMPLEMENTATION
```

## 6. Wind-force contract and G2 feasibility

Primary disturbance-force campaign:

```text
horizontal NED external force
0 N <= ||F_w,NE|| <= 15 N
F_D = 0
```

Prior measurement:

```text
MEASURED_REFERENCE_FORCE_LIMIT=20_N
```

The 20 N reference is not a guaranteed moving-flight safety limit and does not establish a 5 N safety margin at 15 N.

Initial operating grid:

```text
v_N = {-3, 0, +3} m/s
v_E = {-2.5, 0, +2.5} m/s
```

Initial force levels:

```text
0, 3, 6, 9, 12, 15 N
```

Directions:

```text
+N, -N, +E, -E
NE, NW, SE, SW
```

Refine only around observed PASS/FAIL boundaries.

Mandatory feasibility outputs:

```text
peak tilt
thrust magnitude / allocation headroom
motor saturation count/duration
altitude error
v_D excursion
N/E velocity error
cross-track deviation
recovery time
reset/failure events
```

Status:

```text
G2=UNRESOLVED_HIGHEST_PRIORITY_EMPIRICAL_GATE
```

## 7. D-axis acceptance

Altitude is held by PX4 using a fixed `z_sp` in the primary campaign.

D-axis is a monitored constraint channel, not a primary optimization target.

A candidate fails if N/E performance is achieved with unacceptable:

```text
altitude loss
v_D excursion
vertical thrust starvation
excessive tilt
headroom loss
saturation
```

Promotion of D into the primary model is allowed only after evidence shows material N/E model inadequacy attributable to vertical coupling.

## 8. Yaw contract

Primary ID and benchmark use an explicit fixed yaw reference.

```text
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
YAW_PRIMARY_REFERENCE=fixed
```

After primary identification, run a bounded invariance subset at multiple headings such as:

```text
0, 90, 180, 270 deg
```

Require no material unexplained yaw-dependent change in N/E dynamics or tracking metrics.

If this fails, investigate frame conversion, aerodynamic anisotropy, and actuator coupling before adding yaw to the model.

## 9. Identification methodology

Preferred data path:

```text
orthogonal multisine / information-efficient excitation
→ FFT / spectral matrices / coherence
→ high-order 2x2 MIMO VARX predictor
→ predictor-based subspace / SVD reduction
→ compact state-space
→ local model family / LPV only if required
```

Signals:

\[
u=[a_{sp,N},a_{sp,E}]^T,
\qquad
y=[v_N,v_E]^T.
\]

Treat the campaign as closed-loop identification unless input-output independence is explicitly demonstrated.

Validation requires:

```text
held-out one-step prediction
held-out free-run simulation
residual whiteness
input-residual correlation
N/E cross-axis residual audit
FRF agreement
coherence-qualified frequency band
delay/source-age audit
model-order robustness
```

R-squared alone is not sufficient.

Status:

```text
G3=STRONGLY_SUPPORTED
PREFERRED_ID=VARX_TO_REDUCED_STATE_SPACE
```

## 10. Optimizer policy

The benchmark is optimizer-neutral.

Possible offline challengers include:

```text
GA
NSGA-II
CMA-ES
Differential Evolution
local refinement where appropriate
```

Freeze the objective, hard constraints, uncertainty ensemble, and held-out protocol before comparing optimizers.

No optimizer may trade a safety violation for lower tracking cost.

## 11. Time-domain objective set

Primary metrics:

```text
velocity_RMSE_N
velocity_RMSE_E
position_RMSE_N
position_RMSE_E
cross_track_RMSE
cross_track_peak
along_track_velocity_error
IAE
ITAE
overshoot
settling_time
endpoint_error
```

Disturbance metrics:

```text
onset_to_peak_deviation
peak_cross_track_deviation
peak_velocity_error_NE
onset_to_recovery
post-event residual
integrated disturbance error
```

Control-quality metrics:

```text
control effort
TV(command)
jerk
saturation count/duration
headroom_min
peak tilt
```

## 12. Frequency-domain design and validation

Required:

```text
FFT / PSD
2x2 MIMO FRF
coherence
bandwidth
phase margin
gain margin
maximum sensitivity where applicable
high-frequency command energy
resonance-band command energy
```

The optimizer must not be rewarded for aggressive high-frequency control that reduces RMSE while exciting noise or plant resonances.

## 13. Stability verification

Use complementary checks:

```text
eigenvalue / pole checks
local/LPV Lyapunov certificates where applicable
robust plant-ensemble validation
frequency-domain robustness margins
constraint/saturation campaign
```

A missing common quadratic Lyapunov certificate is not automatically proof of instability; certificate scope and conservatism must be reported.

## 14. INDI gate

P4 is retained over P3 only if held-out disturbance rejection improves materially without unacceptable penalties in:

```text
noise sensitivity
HF command energy
jerk
saturation
headroom
p99 latency
implementation complexity
```

Design intent:

```text
PID/feedforward = nominal and low-frequency tracking
INDI = fast incremental mismatch/disturbance correction
```

Frequency-domain analysis must verify no pathological PID/INDI overlap in resonance/noise bands.

Status:

```text
G4=PROMISING_CONDITIONAL_ABLATION_REQUIRED
```

## 15. Latency and runtime

Report separately:

```text
T_compute = source → controller output
T_accept  = source → qualified PX4 accepted boundary
T_effect  = disturbance → first useful correction
T_recover = disturbance → recovery threshold
```

For each:

```text
p50 p95 p99 max jitter
```

Also record:

```text
CPU/update
CPU total
memory
allocations
callback count
DDS traffic
deadline misses
max source age
```

## 16. Fairness invariants

```text
same vehicle/model
same PX4 identity
same NED semantics
same fixed yaw reference in primary campaign
same altitude reference
same initial-condition distribution
same N/E operating point
same force realization/timing
same sensor availability
same safety/authority boundary
same control constraints
```

No arm receives native/future disturbance truth unavailable to comparators.

## 17. Data partitions

```text
IDENTIFICATION
OPTIMIZER_TUNING_DEVELOPMENT
HELD_OUT_VALIDATION
FINAL_BENCHMARK
```

No tuning on FINAL_BENCHMARK data.

Force magnitudes, directions, operating points, and excitation realizations must include held-out combinations.

## 18. Promotion rules

### Promote 2D N/E architecture

Require:

```text
G0 PASS
G2 characterized over declared envelope
G3 model adequacy PASS
D-axis safety/coupling monitor PASS
fixed-yaw primary campaign PASS
yaw invariance audit no material blocker
```

### Promote D into model

Only if repeatable evidence shows N/E prediction/control failure materially explained by D-axis coupling.

### Add further algorithms

Only after a repeatable failure mode is observed.

Examples:

```text
measured resonance → notch/input shaping
INDI noise issue → actuator-aware/filter refinement
constraint failure → reference-governor refinement
yaw dependence → anisotropy/frame investigation
vertical coupling → reconsider 3D model
```

## 19. Replacement decision

Let `P*` be the simplest PID-centered arm on the Pareto frontier.

`PID_REPLACEMENT_CANDIDATE` requires:

```text
P* N/E path/velocity accuracy >= B0
P* disturbance rejection >= B0 over declared feasible 0–15 N envelope
P* D-axis safety monitor PASS
P* p99 T_accept < B0
P* runtime/engineering complexity materially lower
no safety/authority regression
```

If P* wins only over a narrower envelope, classify it as `PID_MISSION_SPECIFIC_ALTERNATIVE`.

If B0 materially wins robustness/OOD behavior, classify the PID branch as `PID_SIMPLICITY_BASELINE_ONLY`.

## 20. Ordered next work

```text
1. G0 exact-local mixed-axis PX4 smoke
2. G2 moving-flight 0–15 N feasibility map
3. G3 orthogonal-multisine 2x2 acquisition
4. VARX→reduced-state-space validation
5. P0→P4 controller implementation and ablation
6. final Pareto benchmark against B0
```

Do not expand the algorithm stack before G0 and G2 are empirically closed.

## 21. Current authority/state

```text
ACTIVE_PID_BENCHMARK_MATRIX=SIM_V6_NE_ONLY
PID_BRANCH_RESEARCH_DIRECTION=STRONG
PRIMARY_CONTROLLER_DIMENSION=2D_NE
D_AXIS_PX4_ALTITUDE_CONTROL=true
D_AXIS_SAFETY_MONITOR=true
YAW_PRIMARY_ID_FIXED=true
YAW_MODEL_FEATURE=false
G0=SOURCE_SUPPORTED_LOCAL_SMOKE_REQUIRED
G1=SUPPORTED_READY_FOR_IMPLEMENTATION
G2=UNRESOLVED_HIGHEST_PRIORITY
G3=STRONGLY_SUPPORTED
G4=PROMISING_CONDITIONAL
FULL_CONTROLLER_IMPLEMENTATION=BLOCKED_ON_G0_G2
CANONICAL_PIPELINE_CHANGED=false
PRODUCTION_AUTHORITY=false
```
