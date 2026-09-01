# PID N/E-Only Research Closure — 2026-09-01

**Branch:** `research/pid-benchmark-20260901`  
**Scope:** simulation-only PID benchmark research  
**Status:** research closure for N/E-only architecture; no canonical/main authority change

## 1. Executive decision

The preferred research architecture is now **N/E-only control and identification**, with PX4 retaining the D-axis/altitude loop.

```text
PRIMARY_CONTROL_AXES=N,E
PRIMARY_MODEL_AXES=N,E
D_AXIS_PRIMARY_CONTROL=false
D_AXIS_PX4_ALTITUDE_LOOP=true
D_AXIS_ROLE=SAFETY_AND_COUPLING_MONITOR
YAW_MODEL_FEATURE=false
YAW_PRIMARY_ID_REFERENCE=FIXED
```

This change reduces identification, optimization, runtime, and verification complexity without removing vertical safety accounting.

## 2. Scientific objective

The branch now asks:

> Can a low-complexity N/E path and velocity controller match or beat the current qualified AURA + FAST/T1/C1 comparator on horizontal NED path tracking, disturbance rejection over the declared 0–15 N horizontal-force envelope, accepted-action latency, runtime, and implementation complexity, while PX4 independently preserves altitude and inner-loop authority?

The velocity envelope is:

\[
v_N \in [-3,3]\;m/s,
\qquad
v_E \in [-2.5,2.5]\;m/s.
\]

The signed extrema are stress-test boundaries, not the complete identification grid.

## 3. Preferred PX4 control boundary

Source-level analysis of PX4 v1.15.4 `PositionControl` supports a candidate mixed-axis configuration in which X/Y may receive external acceleration setpoints while Z remains under PX4 position/velocity control.

Preferred scientific boundary:

\[
[a_{sp,N},a_{sp,E}]
\rightarrow
[v_N,v_E].
\]

PX4 remains authoritative for:

```text
Z position/velocity control
acceleration-to-thrust conversion
attitude control
rate control
control allocation
actuator constraints
```

This source-level result is **not yet a local-runtime qualification**. The exact project PX4 revision must pass a bounded mixed-axis smoke before the boundary is frozen.

### G0 smoke requirements

```text
fixed altitude reference z_sp
fixed yaw reference
small bounded N-only acceleration excitation
external a_N observed at the intended PX4 accepted boundary
PX4 XY velocity-PID contribution absent for externally supplied XY acceleration
PX4 Z loop remains active
attitude/rate/allocation remain active
no unexpected setpoint substitution or fallback
source/timestamp provenance retained
```

Decision:

```text
G0_MIXED_AXIS_PX4_BOUNDARY=SOURCE_SUPPORTED
G0_RUNTIME_QUALIFICATION=REQUIRED
```

## 4. D-axis policy

Dropping D from the primary model does **not** mean ignoring vertical dynamics.

Primary recommendation:

```text
z_sp = fixed altitude
PX4 vertical controller = active
```

D-axis quantities remain mandatory monitors:

```text
z error
v_D
vertical acceleration
thrust magnitude
peak tilt
motor/control-allocation headroom
saturation duration
```

A horizontal controller fails if it achieves good N/E tracking by consuming unacceptable vertical authority or altitude margin.

The architecture is therefore:

```text
2D controlled model
+
vertical constraint channel
```

rather than a fully decoupled 2D vehicle assumption.

## 5. Path objective is separate from velocity objective

A velocity controller alone cannot guarantee return to the original path after a disturbance. If a gust creates cross-track displacement but velocity later returns to the commanded vector, velocity error may become zero while the vehicle remains offset from the path.

A bounded outer path/cross-track law is therefore mandatory.

For unit path tangent `t` in N/E:

\[
e_\perp=(I-tt^T)(p-p_0).
\]

Preferred lightweight guidance form:

\[
v_{ct}=-v_{ct,max}\tanh(e_\perp/e_0),
\]

\[
v_{sp}=v_{parallel}t+v_{ct}.
\]

Hard N/E velocity limits remain authoritative. The exact path-law parameters may be tuned or challenged, but the path-versus-velocity separation is frozen as a design requirement.

Decision:

```text
G1_NE_PATH_GUIDANCE=SUPPORTED
G1_READY_FOR_IMPLEMENTATION=true
```

## 6. Wind-force feasibility is still unresolved

The prior measured `20 N` no-drift value is retained only as a reference measurement.

```text
MEASURED_REFERENCE_FORCE_LIMIT=20_N
ACTIVE_BENCHMARK_HORIZONTAL_FORCE_MAX=15_N
```

It is invalid to infer a guaranteed `5 N` safety margin from `20 N - 15 N` during moving flight.

Required horizontal thrust authority depends on operating point, drag, tilt, vertical thrust demand, actuator limits, and PX4 thrust allocation.

The true research task is to characterize:

\[
F_{max,feasible}(v_N,v_E,direction).
\]

### Initial G2 grid

Use operating points:

```text
v_N = {-3, 0, +3} m/s
v_E = {-2.5, 0, +2.5} m/s
```

and horizontal force directions:

```text
+N, -N, +E, -E
NE, NW, SE, SW
```

with staged magnitudes:

```text
0, 3, 6, 9, 12, 15 N
```

If a PASS/FAIL boundary occurs between levels, refine only that region rather than expanding the entire grid.

### Feasibility metrics

```text
peak tilt
thrust / allocation headroom
motor saturation count and duration
altitude error
v_D excursion
horizontal velocity error
cross-track deviation
recovery time
failure/reset events
```

A case is not accepted merely because the simulator does not crash.

Decision:

```text
G2_0_TO_15N_MOVING_FEASIBILITY=UNRESOLVED
G2_PRIORITY=HIGHEST_EMPIRICAL_GATE
```

## 7. Yaw policy

Yaw is not a model feature or scheduling variable, but primary identification must not allow yaw to drift uncontrolled.

Preferred primary campaign:

```text
yaw_sp = fixed explicit heading, e.g. 0 rad
YAW_MODEL_FEATURE=false
YAW_SCHEDULING_VARIABLE=false
```

Reason: a NaN yaw setpoint may effectively preserve the current yaw rather than freeze one scientific orientation; uncontrolled heading drift can contaminate N/E identification through frame and aerodynamic anisotropy.

After primary identification, repeat a bounded subset at headings such as:

```text
0 deg
90 deg
180 deg
270 deg
```

and audit N/E model/metric invariance.

If yaw materially changes N/E dynamics, first investigate coordinate transforms, aerodynamic anisotropy, and actuator coupling before promoting yaw into the model.

## 8. Preferred identification methodology

The preferred identification path is now:

```text
orthogonal multisine / information-efficient excitation
→ FFT / spectral matrices / coherence
→ high-order 2x2 MIMO VARX predictor
→ predictor-based subspace / SVD reduction
→ compact state-space model
→ local operating-point family / LPV only if evidence requires
```

Primary signals:

\[
u=[a_{sp,N},a_{sp,E}]^T,
\qquad
y=[v_N,v_E]^T.
\]

The high-order VARX representation is an identification intermediate, not the intended onboard model.

### Identification validation gate

Require:

```text
held-out one-step prediction
held-out free-run simulation
residual whiteness
input-residual correlation audit
N/E cross-axis residual audit
FRF agreement
coherence-qualified frequency region
delay/source-age audit
model-order robustness
```

Do not select a model by R-squared alone.

Because excitation may still be applied while a slow holding controller or other feedback is active, data must be treated as closed-loop identification unless independence is explicitly demonstrated.

Decision:

```text
G3_2X2_MIMO_IDENTIFICATION=STRONGLY_SUPPORTED
PREFERRED_ID=VARX_TO_REDUCED_STATE_SPACE
```

## 9. PID, feedforward, and INDI interaction

The controller ladder remains an ablation, not a predetermined winner:

```text
B0 current qualified comparator
P0 fixed 2DOF PID
P1 robust optimized 2DOF PID
P2 gain-scheduled PID
P3 + feedforward
P4 + INDI
```

Optional challengers such as compact H-infinity control may be retained for comparison, but they do not expand the primary runtime architecture unless evidence justifies it.

INDI is retained only if it materially improves held-out disturbance rejection without unacceptable degradation in:

```text
noise sensitivity
high-frequency command energy
jerk
saturation
headroom
p99 latency
complexity
```

Design intent:

```text
PID / feedforward = nominal and low-frequency tracking/rejection
INDI = fast incremental model-error/disturbance correction
```

Frequency-domain analysis must verify that PID, derivative filtering, and INDI do not amplify the same resonance/noise band.

Decision:

```text
G4_PID_INDI=PROMISING_CONDITIONAL
G4_ABLATION_REQUIRED=true
```

## 10. Fourier and stability verification

Fourier/frequency-domain analysis is a core design method, not a post-hoc visualization.

Required quantities include:

```text
FFT / PSD
MIMO FRF
coherence
bandwidth
phase margin
gain margin
maximum sensitivity where applicable
high-frequency command energy
resonance-band command energy
```

Lyapunov/eigenvalue/robust stability checks remain complementary. Failure to find one common quadratic Lyapunov certificate is not by itself proof of instability; the certificate scope must match the actual local/LPV and saturation assumptions.

## 11. Runtime architecture after research

```text
N/E geometric path
        ↓
bounded cross-track guidance
        ↓
bounded v_N_sp / v_E_sp
        ↓
RGS 2DOF PID N/E
        +
model feedforward
        +
optional bounded INDI (P4 only)
        ↓
a_N_sp / a_E_sp
        ↓
PX4 mixed-axis boundary
  ├─ Z altitude hold
  ├─ acceleration→thrust
  ├─ tilt/thrust constraints
  ├─ attitude control
  ├─ rate control
  └─ allocation
        ↓
vehicle
```

Heavy identification and optimization remain offline.

## 12. Research stop rule

Do not broaden the algorithm stack before empirical closure of G0 and G2.

Next ordered work:

```text
1. G0 exact-local mixed-axis PX4 smoke
2. G2 bounded 0–15 N moving-feasibility characterization
3. G3 orthogonal-multisine acquisition and 2x2 identification
4. controller implementation ladder and ablations
```

Only research a new method after a repeatable failure mode appears, for example:

```text
material vertical coupling       → reconsider D-axis model promotion
material yaw dependence          → investigate anisotropy/frame effects
INDI noise sensitivity           → actuator-aware/filter refinement
measured resonance               → notch/input shaping research
hard constraint failure          → reference-governor refinement
```

## 13. Current state

```text
PID_BRANCH_RESEARCH_DIRECTION=STRONG
PRIMARY_CONTROLLER_DIMENSION=2D_NE
D_AXIS_PRIMARY_MODEL=false
D_AXIS_PX4_ALTITUDE_CONTROL=true
D_AXIS_SAFETY_MONITOR=true
YAW_PRIMARY_ID_FIXED=true
YAW_MODEL_FEATURE=false

G0_MIXED_AXIS_PX4_BOUNDARY=SOURCE_SUPPORTED_LOCAL_SMOKE_REQUIRED
G1_NE_PATH_GUIDANCE=SUPPORTED_READY_FOR_IMPLEMENTATION
G2_0_TO_15N_MOVING_FEASIBILITY=UNRESOLVED_HIGHEST_PRIORITY
G3_2X2_MIMO_IDENTIFICATION=STRONGLY_SUPPORTED
G4_PID_INDI=PROMISING_CONDITIONAL

FULL_CONTROLLER_IMPLEMENTATION=BLOCKED_ON_G0_G2
CANONICAL_PIPELINE_CHANGED=false
PRODUCTION_AUTHORITY=false
```
