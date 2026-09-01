# PID Benchmark Matrix — Simulation v5 Readiness-Gated — 2026-09-01

Branch: `research/pid-benchmark-20260901`

This supersedes v4 as the active simulation benchmark matrix. It preserves the signed-NED velocity and 0–15 N force envelope but adds the mandatory implementation-readiness gates from `PID_IMPLEMENTATION_READINESS_CRITIQUE_20260901.md`.

## 1. Coordinate / mission contract

```text
COORDINATE_FRAME=NED
NED_X=NORTH
NED_Y=EAST
NED_Z=DOWN
```

Signed operating envelope:

\[
v_N\in[-3.0,+3.0]\text{ m/s}\]
\[
v_E\in[-2.5,+2.5]\text{ m/s}\]
\[
v_D\in[-0.8,+0.8]\text{ m/s}\]

Required sign-corner stress cases remain all 8 combinations of:

```text
v_N = ±3.0 m/s
v_E = ±2.5 m/s
v_D = ±0.8 m/s
```

These are envelope boundaries / stress points, not the complete identification grid.

Yaw:

```text
YAW_OBJECTIVE=false
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
PRIMARY_YAW_REFERENCE=FIXED_FOR_MAIN_CAMPAIGN
YAW_INVARIANCE_AUDIT=REQUIRED
```

## 2. Disturbance contract

Primary direct-force campaign:

```text
DIRECT_FORCE_DISTURBANCE_CAMPAIGN=true
0 N <= ||F_w,NE|| <= 15 N
F_D = 0 for the primary horizontal-force campaign
```

Recommended levels:

```text
0, 3, 6, 9, 12, 15 N
```

Directions:

```text
±N
±E
NE/NW/SE/SW
headwind/tailwind/crosswind relative to commanded NED path
```

Previously measured reference:

```text
MEASURED_REFERENCE_FORCE_LIMIT=20_N
SAFE_FORCE_LIMIT=UNPROVEN_FOR_SIGNED_MOVING_ENVELOPE
```

A later separate campaign may use aerodynamic-wind/relative-airflow semantics. Do not conflate it with the direct-force campaign.

## 3. Pre-implementation mandatory gates

### G0 — exact plant/control boundary

Required result:

```text
PLANT_BOUNDARY_FROZEN=true
OLD_VELOCITY_CONTROLLER_NOT_HIDDEN_INSIDE_REPLACEMENT_MODEL=true
PX4_ACCEPTED_ACTION_BOUNDARY_SOURCE_GROUNDED=true
```

Preferred plant input is a qualified NED acceleration / virtual-control setpoint boundary with PX4 attitude/rate/allocation/vehicle downstream.

### G1 — path vs velocity objective split

Required architecture:

```text
3D NED path
→ path/cross-track controller
→ requested v_sp^NED
→ optional governor/shaper
→ velocity controller
```

Required metrics:

```text
horizontal/3D cross-track error
along-track velocity error
N/E/D velocity error
position displacement/recovery
```

### G2 — signed-NED 15 N feasibility map

Required before calling the entire 0–15 N moving envelope safe/feasible.

Characterize across signed NED operating points:

```text
required thrust
available thrust/control authority
peak tilt
headroom
saturation exposure
recovery success
```

Do not infer a guaranteed 5 N reserve from 20 N − 15 N.

## 4. Identification ladder

Use interior points, zero crossings, transitions and reversals in addition to sign-corner stress cases.

```text
ID0 local low-order SISO sanity models
ID1 3x3 MIMO VARX
ID2 regularized/order-qualified 3x3 MIMO VARX
ID3 high-order VARX → reduced MIMO state-space
ID4 local reduced MIMO state-space family
ID5 LPV/interpolated local model family
```

Preferred model variables:

\[
y=[v_N,v_E,v_D]^T\]

\[
u=[a_{sp,N},a_{sp,E},a_{sp,D}]^T\]

or an equivalent source-grounded acceleration-response boundary.

Closed-loop identification audit is mandatory.

## 5. Frequency-domain methodology

Before controller promotion require:

```text
FFT / PSD
cross spectra
coherence
MIMO FRF / Bode
gain margin
phase margin
maximum sensitivity Ms
bandwidth
resonance map
high-frequency command energy
```

Excitation ladder:

```text
bounded step/ramp sanity
chirp/sine sweep
orthogonal multisine
CIBES-informed excitation if evidence warrants
```

Frequency estimates must not be trusted outside adequate coherence/excitation regions.

## 6. Controller arms

```text
P0 FIXED-ROBUST-2DOF-PID

P1 LOCAL-OPTIMIZED-2DOF-PID
   local robust gains; optimizer not fixed to GA only

P2 RGS-2DOF-PID
   bounded gain scheduling + bumpless transfer

P3 RGS-2DOF-PID-FF
   + bounded model/physics feedforward

P4 RGS-2DOF-PID-FF-INDI
   + bounded measured-response INDI

P5 P4 + DOB
   conditional only

P6 P4/P5 + ANN/RBF gain-surface compression
   conditional only

H0 DATA_DRIVEN_H_INFINITY_CHALLENGER
   optional lightweight robust-control comparator

C_FAST current qualified AURA + FAST/T1/C1 revision
C_FULL future qualified predictive pipeline revision; not assumed available
```

Mandatory controller ablation:

```text
P0 → P1 → P2 → P3 → P4
```

P5/P6/H0 are conditional/comparator arms, not default stack complexity.

## 7. Optimizer policy

Scientific identity is not “GA”.

Freeze:

```text
plant/model ensemble
objective definitions
hard constraints
seed provenance
data partitions
```

Then compare, when useful:

```text
GA
NSGA-II
CMA-ES
Differential Evolution
local numerical polishing
```

Select the simplest reproducible Pareto solution.

## 8. Hard reject constraints

No optimizer candidate survives if it violates a frozen hard gate for:

```text
closed-loop instability
unrecoverable divergence
tilt limit
thrust/control saturation exposure
minimum headroom
integrator runaway
gain discontinuity / non-bumpless transfer
control deadline
invalid/stale scheduler state
```

Do not convert these into soft penalties that can be traded for lower RMSE.

## 9. Soft objectives

Time domain:

```text
velocity RMSE N/E/D
position/path RMSE
cross-track error
IAE / ITAE
overshoot
settling
recovery time
control effort
TV(command)
jerk
```

Frequency domain:

```text
high-frequency command energy
resonance-band command energy
bandwidth shaping
robustness margins
Ms
```

## 10. Lyapunov / stability verification ladder

```text
local poles/eigenvalues
common quadratic Lyapunov attempt
parameter-dependent/local Lyapunov checks if needed
frequency-domain robustness margins
held-out nonlinear simulation
saturation/constraint qualification
```

Failure to find a common quadratic certificate is not automatically proof of instability; it triggers a less conservative verification step.

## 11. Reference governor semantics

If used:

```text
log r_requested
log r_governed
log y_measured
```

Report separately:

```text
mission tracking error = r_requested − y
controller tracking error = r_governed − y
```

A governor may improve safety/feasibility but must not hide mission-command deviation.

## 12. Yaw-invariance campaign

Primary campaign holds a fixed yaw reference.

Separate audit headings should include at least:

```text
0 deg
90 deg
180 deg
270 deg
```

under equivalent NED path/force cases.

Require no material unexplained yaw-dependent change in:

```text
NED dynamics
MIMO residuals
N/E/D coupling
tracking metrics
force rejection
```

If this fails, investigate body-to-NED transforms, aerodynamic anisotropy and actuator coupling before expanding model state.

## 13. Feasibility / constraint campaign

At each relevant signed NED operating region evaluate external force levels through 15 N and record:

```text
peak tilt
thrust demand
headroom_min
saturation duration
path deviation
velocity deviation
recovery success/time
```

Classify each point:

```text
FEASIBLE_WITH_MARGIN
FEASIBLE_NEAR_BOUNDARY
INFEASIBLE_OR_UNRECOVERABLE
UNKNOWN_INSUFFICIENT_EVIDENCE
```

Only FEASIBLE points may be used to claim controller-performance capability.

## 14. Data partitions

```text
IDENTIFICATION
OPTIMIZER_DEVELOPMENT
HELD_OUT_VALIDATION
FINAL_BENCHMARK
```

No optimizer/model/gain selection may use FINAL_BENCHMARK results.

Hold out combinations across:

```text
signed velocity regions
force magnitudes
force directions
transitions/reversals
model-uncertainty perturbations
```

## 15. PID / INDI promotion criteria

P2 over P1:

```text
scheduler improves envelope performance materially
no discontinuity / latency / robustness penalty
```

P3 over P2:

```text
feedforward improves tracking/control effort without reducing robustness
```

P4 over P3:

```text
material held-out force-rejection benefit over 0–15 N envelope
no unacceptable noise sensitivity
no unacceptable HF/resonance command energy
no unacceptable saturation/headroom penalty
no unacceptable p99 accepted-action latency
```

## 16. Current-pipeline replacement decision

Let `P*` be the simplest PID-centered arm on the measured Pareto frontier.

`PID_REPLACEMENT_CANDIDATE` requires at minimum:

```text
G0 PASS
G1 PASS
G2 PASS
model validation PASS
frequency/robustness validation PASS
yaw-invariance PASS or explicitly bounded yaw-specific scope
P* path/velocity accuracy >= C_FAST
P* disturbance rejection >= C_FAST over same feasible 0–15 N envelope
P* p99 T_accept < C_FAST
P* runtime/implementation complexity materially lower
no authority/safety regression
```

`PID_MISSION_SPECIFIC_ALTERNATIVE` applies if P* strongly wins latency/complexity in a narrower declared envelope but loses robustness outside it.

`PID_SIMPLICITY_BASELINE_ONLY` applies if C_FAST materially wins disturbance/OOD/path-retention behavior.

## 17. Active execution order

```text
G0 plant boundary
→ G1 path/velocity split
→ G2 force-feasibility map
→ acquisition design
→ FRF/coherence
→ 3x3 MIMO VARX
→ reduced model / LPV decision
→ P0 fixed robust 2DOF PID
→ P1 optimizer comparison
→ P2 scheduling
→ P3 feedforward
→ P4 INDI
→ conditional additions only from measured failure modes
→ fair comparison with exact C_FAST identity
```

## 18. Current authority/status

```text
BENCHMARK_BRANCH_ONLY=true
SIMULATION_SCOPE=true
PID_BRANCH_RESEARCH_DIRECTION=STRONG
PID_ARCHITECTURE_IMPLEMENTABLE=YES
PID_FULL_STACK_IMPLEMENTATION=BLOCKED_ON_G0_G1_G2
EXPECTED_FIRST_CONTROLLER_BASELINE=FIXED_ROBUST_2DOF_PID
EXPECTED_CHAMPION=UNKNOWN_UNTIL_EVIDENCE
CANONICAL_PIPELINE_REPLACEMENT=false
PRODUCTION_AUTHORITY=false
CURRENT_CANONICAL_PHASE0_GATE_UNCHANGED=true
```
