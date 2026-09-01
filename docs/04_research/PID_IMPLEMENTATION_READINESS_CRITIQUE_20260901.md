# PID Benchmark — Implementation Readiness Critique — 2026-09-01

Branch: `research/pid-benchmark-20260901`

This document converts the latest architecture critique into explicit implementation gates. It is intentionally skeptical: the PID-centered branch remains a strong research direction, but it is **not implementation-ready by default** simply because its components are individually plausible.

## 1. Current status

```text
PID_BRANCH_RESEARCH_DIRECTION=STRONG
PID_ARCHITECTURE_IMPLEMENTABLE=YES
PID_ARCHITECTURE_READY_FOR_FULL_IMPLEMENTATION=NOT_YET
CANONICAL_PIPELINE_CHANGED=false
PRODUCTION_AUTHORITY=false
```

The exact purpose remains comparative:

> Determine whether a materially smaller NED controller stack can match or beat the current AURA/FAST/T1/C1 path on tracking accuracy, path retention, disturbance rejection, accepted-action latency, robustness, runtime determinism and implementation complexity.

## 2. Mandatory gate A — exact control/plant boundary

Do not identify a plant that already contains the velocity controller being replaced.

Preferred research boundary:

```text
external research controller
        ↓
a_sp^NED
        ↓
PX4 acceleration/thrust-attitude conversion
        ↓
PX4 attitude controller
        ↓
PX4 rate controller
        ↓
control allocation / actuator / vehicle
        ↓
measured NED motion
```

The identified plant should therefore bind a qualified PX4-side acceleration/virtual-control input to NED response, e.g.:

\[
a_{sp}^{NED}\rightarrow[v_N,v_E,v_D]\]

or an equivalent acceleration-response formulation if source semantics make that boundary cleaner.

Forbidden shortcut:

```text
identify requested velocity → output velocity
while the old PX4 velocity controller remains inside the identified plant
then use that model to claim a replacement velocity controller design
```

That would make the model boundary circular.

Gate A closes only when the exact source/action/acceptance boundary is frozen from code semantics and telemetry.

## 3. Mandatory gate B — path objective and velocity objective must be separated

Velocity tracking alone does not guarantee path retention.

A gust can displace the vehicle from the desired line while the controller later restores exactly the commanded velocity. If velocity error becomes zero, a pure velocity PID has no reason to remove the accumulated cross-track displacement.

For a desired path tangent \(t\), define cross-track error:

\[
e_\perp=(I-tt^T)(p-p_{line}).\]

The outer path loop should generate a corrective velocity component, for example:

\[
v_{sp}=v_{nom}t-K_{ct}e_\perp\]

subject to reference-governor / acceleration / velocity limits.

Required architectural separation:

```text
3D NED path / mission reference
        ↓
path / cross-track controller
        ↓
requested NED velocity
        ↓
optional reference governor / command shaper
        ↓
RGS 2-DOF NED velocity controller
```

Required metrics must distinguish:

```text
cross_track_error
along_track_velocity_error
N/E/D velocity error
absolute N/E/D position error where appropriate
```

## 4. Mandatory gate C — 15 N test envelope is not proven safe merely because 20 N was previously measured

Current facts:

```text
ACTIVE_DISTURBANCE_FORCE_RANGE=0_TO_15_N
PREVIOUS_MEASURED_NO_DRIFT_REFERENCE=20_N
```

The 20 N value is retained as:

```text
MEASURED_REFERENCE_FORCE_LIMIT
```

not automatically as:

```text
SAFE_FORCE_LIMIT
```

Do not interpret:

\[
20N-15N=5N
\]

as a guaranteed 5 N safety reserve during flight at nonzero velocity.

At signed NED velocities

\[
v_N\in[-3,3],\quad v_E\in[-2.5,2.5],\quad v_D\in[-0.8,0.8]\]

the aircraft is already consuming thrust/control authority for translational motion, drag compensation, vertical acceleration/velocity and attitude maintenance.

Required new object:

```text
SIGNED_NED_FEASIBILITY_MAP
```

At minimum characterize across operating points:

```text
available thrust/control authority
nominal required thrust
peak tilt
motor/control-allocation headroom
saturation exposure
recoverability under external force
```

The 0–15 N campaign may proceed scientifically only after it is shown to remain inside the declared simulation safety/control envelope for the tested operating points.

## 5. Mandatory gate D — NED interfaces do not prove yaw irrelevance

All model/controller interfaces remain NED-native.

```text
COORDINATE_FRAME=NED
YAW_OBJECTIVE=false
YAW_SCHEDULING_VARIABLE=false
YAW_MODEL_FEATURE=false
```

However, yaw can still affect the physical plant through body-frame aerodynamic anisotropy, actuator geometry, frame conversion errors or other orientation-dependent effects.

Preferred benchmark policy:

```text
freeze one yaw reference for the primary controller campaign
keep PX4 yaw stabilization active
exclude yaw from scheduling/model features
```

Then perform a separate yaw-invariance audit at multiple headings, for example:

```text
0 deg
90 deg
180 deg
270 deg
```

under equivalent NED path and disturbance conditions.

Required checks:

```text
NED response equivalence
identified-model parameter/residual shift
cross-axis coupling shift
residual correlation with yaw
tracking metric shift
```

If yaw materially changes NED dynamics, do not hide the effect by simply omitting yaw. First audit frame conversion, aerodynamic anisotropy and actuator coupling; only then decide whether the model structure must change.

## 6. Mandatory gate E — signed velocity values are operating-envelope boundaries, not the entire identification grid

Active signed bounds:

\[
v_N\in[-3,3]\text{ m/s}\]
\[
v_E\in[-2.5,2.5]\text{ m/s}\]
\[
v_D\in[-0.8,0.8]\text{ m/s}.\]

The 8 sign-corner combinations remain required stress cases, but they are not sufficient to identify a continuous gain surface.

Identification must include interior points and zero crossings.

Required coverage includes:

```text
zero velocity neighborhoods
positive/negative intermediate N/E/D velocities
entry/exit trajectories through operating regions
axis reversals
full-vector reversal
```

Do not assume + and - dynamics are symmetric. In particular, ascent and descent may differ.

## 7. Mandatory gate F — promote identification from 2D N/E to 3D NED

Because the mission includes \(v_D=\pm0.8\) m/s and horizontal tilt changes vertical thrust availability, the preferred identification baseline becomes 3×3 NED MIMO rather than horizontal-only N/E MIMO.

Suggested form:

\[
y_k=\begin{bmatrix}v_N&v_E&v_D\end{bmatrix}^T\]

\[
u_k=\begin{bmatrix}a_{sp,N}&a_{sp,E}&a_{sp,D}\end{bmatrix}^T.\]

Identification ladder:

```text
ID0 local low-order SISO sanity models
ID1 3x3 MIMO VARX
ID2 regularized/order-qualified 3x3 MIMO VARX
ID3 high-order VARX → reduced MIMO state-space
ID4 local reduced state-space family
ID5 LPV/interpolated local model family
```

Levinson/block-Levinson remains an optional numerical challenger for structured estimation/order sweeps; it is not a required solver.

## 8. Optimizer neutrality

GA/NSGA-II is an optimization method, not part of the scientific identity of the controller.

Freeze:

```text
model ensemble
objective definitions
hard constraints
train/development/held-out/final partitions
random-seed provenance
```

Then compare optimization methods if useful:

```text
GA
NSGA-II
CMA-ES
Differential Evolution
local gradient/nonlinear least-squares polishing where applicable
```

The selected controller is the simplest reproducible Pareto solution, not automatically the solution returned by GA.

## 9. Safety constraints are hard rejects, not soft cost tradeoffs

GA or any optimizer must not be allowed to trade a safety violation for lower tracking error.

Hard-reject conditions should include the frozen simulation limits for:

```text
closed-loop instability
unrecoverable divergence
excess tilt
thrust/control saturation beyond allowed exposure
insufficient motor/control-allocation headroom
integrator runaway
pathological gain discontinuity
missed control deadlines
invalid/stale scheduler state
```

Soft objectives may include:

```text
RMSE
IAE / ITAE
cross-track error
settling / overshoot
control effort
TV(command)
jerk
high-frequency command energy
resonance-band energy
```

## 10. Lyapunov verification semantics

Lyapunov analysis remains required as a strong stability verification tool, but failure to find one common quadratic Lyapunov function must not automatically be interpreted as proof of instability.

Preferred ladder:

```text
local eigenvalue / pole checks
common quadratic Lyapunov attempt
parameter-dependent/local Lyapunov checks if required
frequency-domain robustness margins
held-out nonlinear simulation
constraint/saturation qualification
```

A certificate on an unsaturated reduced linear model does not by itself certify saturated nonlinear behavior.

## 11. Fourier / FRF becomes core methodology

Frequency-domain analysis is mandatory before controller promotion.

Use:

```text
FFT / PSD
cross-spectral density
coherence
MIMO FRF / Bode
phase/gain margin
maximum sensitivity Ms
bandwidth
resonance detection
command spectral-energy accounting
```

Suggested identification excitation:

```text
bounded orthogonal multisine
chirp / sine sweeps where appropriate
CIBES-informed excitation design later
```

GA/optimizer objectives should include frequency-domain gates/penalties so aggressive gains cannot win solely by reducing time-domain error while exciting resonant/high-frequency modes.

## 12. Reference governor fairness semantics

A reference governor / command shaper is allowed, but benchmark reporting must preserve both the user's requested mission command and the governed feasible command.

Log:

```text
r_requested
r_governed
y_measured
```

Report separately:

\[
e_{mission}=r_{requested}-y\]

and

\[
e_{control}=r_{governed}-y.\]

Do not score only against the governed reference and claim perfect mission tracking.

## 13. INDI promotion remains conditional

INDI is a high-value challenger, not an assumed winner.

```text
P3 = RGS-2DOF-PID + feedforward
P4 = P3 + bounded INDI
```

P4 is retained only if held-out 0–15 N disturbance testing shows a material benefit without unacceptable penalties in:

```text
noise sensitivity
command spectral energy
jerk
saturation/headroom
accepted-action latency
complexity
```

Actuator-dynamics-aware and control-effectiveness-adaptive INDI are later variants, not default complexity.

## 14. Separate force rejection from aerodynamic-wind realism

The primary 0–15 N external-force campaign tests force rejection.

It must be labeled:

```text
DIRECT_FORCE_DISTURBANCE_CAMPAIGN
```

A later, separate campaign should test aerodynamic wind using relative airflow / drag semantics:

```text
AERODYNAMIC_WIND_CAMPAIGN
```

Do not claim that a center-of-mass 15 N force pulse is automatically equivalent to all physically realistic wind conditions.

## 15. Proposed implementation order

Do not build the full PID+VARX+LPV+INDI stack immediately.

```text
G0 freeze plant/control boundary
G1 freeze path-vs-velocity objective split
G2 build signed-NED feasibility map for 0–15 N envelope
G3 acquire NED identification data
G4 FRF/coherence + 3x3 MIMO VARX validation
G5 reduced state-space / LPV decision
G6 fixed robust 2-DOF PID baseline
G7 gain-scheduling ablation
G8 model/feedforward ablation
G9 INDI ablation
G10 optional RG / adaptive effectiveness / DOB / ANN only if measured gaps remain
```

## 16. Current decision

```text
PID_BRANCH_RESEARCH_DIRECTION=STRONG
PID_ARCHITECTURE_IMPLEMENTABLE=YES
PID_FULL_STACK_IMPLEMENTATION=BLOCKED_ON_G0_G1_G2
EXPECTED_FIRST_CONTROLLER_BASELINE=FIXED_ROBUST_2DOF_PID
EXPECTED_STRONG_CHALLENGER=RGS_2DOF_PID_FF_INDI
EXPECTED_CHAMPION=UNKNOWN_UNTIL_EVIDENCE
```

The research branch should be abandoned or materially redesigned if the early gates show that the assumed control boundary, force envelope or path-control decomposition is invalid. The goal is not to protect the PID hypothesis; the goal is to find the smallest controller architecture on the best measured Pareto frontier.
