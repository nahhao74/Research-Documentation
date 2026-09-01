# Parallel PID Benchmark Branch — 2026-09-01

## Purpose

This branch defines an **independent controller benchmark track** against the current AURA–WISE–WM–AEGIS pipeline. It does not replace or authorize changes to the canonical Phase-0 work.

The benchmark question is:

> Can a much smaller data-driven PID-centered architecture equal or outperform the current pipeline on tracking accuracy, disturbance rejection, end-to-end response latency, runtime determinism and implementation complexity?

The branch is intentionally designed so that it can begin **before the current pipeline is complete**. Offline identification, GA tuning, gain-schedule construction, replay and deterministic simulation can proceed now; live A/B comparison against later pipeline revisions must bind the exact pipeline identity used at comparison time.

---

# 1. Research conclusion

The strongest PID-centered candidate is **not** a plain GA-tuned PID and not an ANN that maps only `[error, delta_error] -> gains`.

The preferred ladder is:

```text
closed-loop flight data
        ↓
local / LPV dynamics identification
        ↓
robust multi-objective GA tuning
        ↓
2-DOF PID
+ derivative-on-measurement filtering
+ anti-windup
+ bumpless gain scheduling
        ↓
model/physics feedforward
        ↓
optional INDI augmentation
        ↓
optional disturbance-observer augmentation
        ↓
ANN/RBF gain-surface compression only if simpler scheduling is insufficient
```

The guiding rule is:

```text
retain each layer only if an ablation shows a measurable benefit.
```

Do not build the final stack by combining every method by default.

---

# 2. Proposed controller family name

Working name:

```text
RGS-2DOF-PID
Robust Gain-Scheduled Two-Degree-of-Freedom PID
```

Strong augmented candidate:

```text
RGS-2DOF-PID-INDI
```

Optional observer variant:

```text
RGS-2DOF-PID-INDI-DOB
```

These are benchmark names, not novelty/publication claims.

---

# 3. Why 2-DOF PID instead of basic PID

A 2-DOF PID separates setpoint tracking behavior from disturbance-rejection behavior through setpoint weighting.

A practical form is:

\[
u_c(t)=K_p(\beta r-y)+K_i\int(r-y)dt-K_d\,\dot y_f
\]

where:

```text
r      = reference
y      = measured output
beta   = proportional setpoint weight
y_f    = filtered measurement used by derivative action
```

Derivative action should initially be applied to measurement rather than setpoint:

```text
derivative setpoint weight c = 0
```

to avoid derivative kick on velocity-step commands.

This allows the GA to tune tracking and disturbance rejection without forcing one set of gains to trade both objectives through the same setpoint path.

---

# 4. Mandatory PID implementation details

The benchmark PID must not be a textbook three-term implementation.

Required features:

```text
2-DOF setpoint weighting
filtered derivative on measurement
explicit output saturation
anti-windup
integrator clamp / state validity
bumpless gain changes
bounded gain-rate change
fixed sample period
no dynamic allocation in controller step
```

## 4.1 Derivative filter

Use a first-order derivative filter rather than raw finite differencing.

The cutoff must be tuned against:

```text
sensor noise
controller sample rate
identified plant bandwidth
actuator bandwidth
```

A derivative filter is retained only if it improves tracking/robustness without materially increasing disturbance-response delay.

## 4.2 Anti-windup

Actuator/setpoint saturation is expected in aggressive velocity transitions and gust recovery.

Benchmark at least:

```text
AW0 — clamp only
AW1 — conditional integration
AW2 — back-calculation
AW3 — conditional integration + dynamic/back-calculation hybrid
```

The 2026 anti-windup review by Caparroz, Soltesz, Hägglund and Guzmán reports that saturation-induced windup can significantly degrade overshoot and recovery, and proposes systematic tuning directions for back-calculation and a conditional-integration/back-calculation hybrid.

No anti-windup strategy is promoted without the saturation campaign.

---

# 5. System identification strategy

Do not identify a single transfer function only as a function of requested velocity setpoint.

The plant varies with operating condition.

Define a scheduling vector initially as:

\[
\rho = [v_x, v_y, |v|, a_{cmd}, tilt, battery, motor\_headroom]
\]

Not all variables must survive the ablation. Start with the smallest measurable set:

```text
rho0 = [vx, vy, |v|]
```

then add a variable only if it materially explains dynamics variation.

## 5.1 Data source

Use closed-loop flight data with exact source timing and known applied/accepted controller action.

At minimum retain:

```text
reference velocity
measured velocity
acceleration
attitude / tilt
controller command
accepted PX4-side command or closest qualified application boundary
motor/control-allocation headroom if available
battery voltage
session/reset identity
source timestamps
```

## 5.2 Model forms

Evaluate in this order:

```text
ID0 — local SISO low-order transfer function
ID1 — local MIMO transfer/state-space model for N/E coupling
ID2 — local state-space model with delay
ID3 — LPV / interpolated local model family
```

Prefer the smallest model whose held-out residuals and cross-axis errors are adequate.

Closed-loop identification must explicitly account for feedback; naive open-loop regression of command against output is not accepted solely because the fit looks good.

## 5.3 Operating regions

Identify local models from operating regions, not isolated steady setpoints only.

Each region must include transitions through the region:

```text
constant velocity
step into region
step out of region
ramp through region
sine/chirp near region
```

This prevents a scheduler optimized only for steady-state points from failing during gain transitions.

---

# 6. Robust GA tuning

GA remains **offline only**.

Runtime never runs the genetic algorithm.

For each local model / operating region, solve a constrained multi-objective problem.

Candidate objective:

\[
J =
w_e J_{ITAE}
+w_p M_p
+w_s T_s
+w_u J_u
+w_{tv} TV(u)
+w_j J_{jerk}
+w_{sat} J_{sat}
+w_r J_{robust}
\]

where:

```text
ITAE   = time-weighted absolute tracking error
Mp     = overshoot
Ts     = settling time
Ju     = control effort
TV(u)  = total variation / actuator activity
Jjerk  = command jerk/smoothness penalty
Jsat   = saturation exposure / duration
Jrobust= robustness penalty across uncertain plants
```

## 6.1 Plant ensemble tuning

Do not tune against one nominal identified model only.

For each operating region construct:

\[
\mathcal G_i = \{G_i^{(1)},...,G_i^{(M)}\}
\]

covering plausible variation in:

```text
identification uncertainty
mass/payload
battery / thrust effectiveness
transport/controller delay
actuator bandwidth
sensor/filter variation
aerodynamic drag
```

Preferred robust objective:

\[
K_i^*=\arg\min_K\left[
\mathbb E_{G\in\mathcal G_i}J(G,K)
+\lambda\operatorname{Var}(J)
+\mu\max_G J(G,K)
\right]
\]

with hard rejection of unstable candidates.

## 6.2 Multi-objective optimization

If a single weighted scalar objective hides important tradeoffs, use NSGA-II/Pareto optimization for:

```text
tracking error
settling time / overshoot
control effort
robustness margin
```

Then freeze one Pareto point using a declared engineering policy before runtime.

---

# 7. Gain scheduling

Preferred implementation order:

```text
GS0 — discrete lookup
GS1 — linear/barycentric interpolation
GS2 — polynomial/spline surface
GS3 — LPV-qualified schedule
GS4 — RBF / tiny ANN compression only if required
```

Do not begin with ANN.

## 7.1 Why not ANN first

If optimal gains vary smoothly over a low-dimensional operating envelope, lookup/interpolation is:

```text
faster
simpler
more deterministic
easier to audit
easier to bound
```

than ANN inference.

## 7.2 When ANN/RBF becomes justified

ANN/RBF may be tested only if:

```text
scheduling dimension becomes large
lookup grid becomes impractical
simple interpolation creates excessive error
nonlinear gain surface is demonstrated on held-out regions
```

ANN input must encode operating state, not only `error` and `delta_error`.

Candidate input:

```text
[v_ref, v, |v|, acceleration, tilt, battery, motor_headroom]
```

ANN output must be constrained:

```text
K_min <= K <= K_max
|K(k)-K(k-1)| <= DeltaK_max
```

and pass out-of-distribution fallback to a certified table/scheduled gain.

---

# 8. Bumpless transfer and schedule-rate limits

Gain changes must not create controller-state discontinuities.

Requirements:

```text
continuous/smoothed gain interpolation
integral-state continuity
output continuity where possible
gain-rate limit
no instantaneous Ki jump without integral-state compensation
```

A scheduler that produces excellent frozen-point performance but transient spikes while crossing operating regions fails qualification.

---

# 9. Physics/model feedforward

The PID should not be forced to cancel dynamics that are already known.

Candidate decomposition:

\[
u = u_{ff}+u_{PID}
\]

with feedforward derived from the identified/physical model.

Candidate terms:

```text
reference acceleration feedforward
identified drag feedforward
steady tilt/thrust compensation
known gravity/frame transforms
actuator static-map compensation if qualified
```

The feedforward branch must be bounded and must not bypass PX4 authority.

Keep a feedback path capable of stable flight if feedforward is disabled.

---

# 10. INDI augmentation

INDI is the strongest disturbance-rejection augmentation candidate in this branch.

Experimental work by Smeur, de Croon and Chu demonstrated cascaded INDI on a quadrotor repeatedly entering/exiting a 10 m/s wind-tunnel flow; the published comparison reported average maximum position deviation of about 21 cm for INDI versus about 151 cm for the comparison controller using integrated-error feedback.

The key reason for testing INDI is not lower arithmetic cost than PID. It is the use of measured incremental response to reduce dependence on exact plant/disturbance models.

Preferred architecture:

```text
velocity/reference loop
        ↓
RGS-2DOF-PID
        ↓
desired acceleration / virtual control
        ↓
INDI incremental correction
        ↓
PX4-qualified command boundary
```

or, where the existing PX4 structure makes that boundary inappropriate:

```text
RGS-2DOF-PID nominal action
        +
bounded INDI residual correction
```

The exact placement must be chosen from source semantics and PX4 authority boundaries, not by convenience.

## 10.1 INDI variants to benchmark

```text
INDI0 — classical measured-response INDI
INDI1 — actuator-dynamics-aware INDI
INDI2 — control-effectiveness-adaptive INDI
INDI3 — low-order robust/H-infinity outer augmentation only if evidence requires it
```

Recent research specifically identifies uncertain control effectiveness and actuator dynamics as practical weaknesses of conventional INDI, and proposes actuator-dynamics-aware/adaptive variants.

---

# 11. Disturbance-observer augmentation

A DOB/NDO/ESO branch is optional and must be tested **after** PID+INDI.

Reason:

```text
INDI already rejects part of model error/disturbance through measured incremental response.
Adding a DOB may be redundant and may add filtering delay.
```

Use an observer only if ablation shows a remaining disturbance component that the simpler controller does not reject adequately.

Candidate observer requirements:

```text
low order
bounded output
causal
explicit filter delay measurement
no large neural model
no dependence on future state
```

Do not combine multiple observers just to improve nominal simulation metrics.

---

# 12. Recommended benchmark ladder

The branch must preserve every intermediate controller so ablation is possible.

```text
P0 — FIXED-2DOF-PID
     one robust globally tuned controller

P1 — GA-LOCAL-2DOF-PID
     independent robust local gains

P2 — RGS-2DOF-PID
     bounded/interpolated gain schedule

P3 — RGS-2DOF-PID-FF
     + physics/data feedforward

P4 — RGS-2DOF-PID-INDI
     + measured-response incremental correction

P5 — RGS-2DOF-PID-INDI-DOB
     + observer only if P4 leaves a measured gap

P6 — RGS-2DOF-PID-INDI-NN-SCHED
     ANN/RBF gain-surface compression only if GS0–GS3 are inadequate
```

Expected champion candidate before evidence:

```text
P4 — RGS-2DOF-PID-INDI
```

This is a hypothesis, not a decision.

---

# 13. Current-pipeline comparison arm

The current pipeline may be incomplete. Therefore every benchmark report must record the exact comparison identity.

At minimum define:

```text
C_FAST = current qualified fast-path revision
         PX4 + AURA + FAST/T1/C1

C_FULL = later full predictive revision
         PX4 + AURA + FAST/T1/C1
         + WM/WISE/AEGIS predictive refinement
         only when that revision actually exists and is qualified
```

Never compare P4 today with a hypothetical future `C_FULL` result.

Current benchmark can begin against `C_FAST`; a later replay/live campaign can add `C_FULL` without rewriting historical conclusions.

---

# 14. Fairness rules

All compared controllers must use the same:

```text
vehicle/model identity
PX4 build and parameters except controller-specific parameters
sensor stream
reference trajectory
wind/event realization
payload
battery/thrust condition
source-time clock
actuator constraints
control-authority boundary
initial condition distribution
```

Controllers must not be given privileged future disturbance truth unless all arms receive that information under the same causal contract.

No controller may directly command motors if the comparison baseline enters PX4 at a higher qualified boundary.

---

# 15. Scenario matrix

## Nominal tracking

```text
CALM hover
0→2 m/s step
2→5 m/s step
5→2 m/s step
+V→-V reversal
N/E diagonal transitions
ramps
sine
chirp
```

## Disturbance

```text
GUST +N / -N / +E / -E
continuous wind
wind onset/change/clear
crosswind during velocity transition
```

## Model variation

```text
payload +10%, +20%
thrust/control-effectiveness reduction
battery/thrust variation
actuator bandwidth variation
sensor noise increase
transport/executor delay injection
```

## Constraint region

```text
high acceleration request
large velocity reversal
motor/control-allocation headroom reduction
tilt-limit approach
saturation/recovery
```

---

# 16. Metrics

## Tracking accuracy

```text
velocity RMSE
position RMSE
IAE
ITAE
peak error
endpoint error
overshoot
settling time
```

## Disturbance rejection

```text
event onset → peak deviation
peak deviation
event onset → recovery
post-gust residual error
integrated error during disturbance
```

## Control quality

```text
control effort
TV(command)
jerk
saturation count
saturation duration
motor/control-allocation headroom
```

## Latency

Do not report only controller arithmetic time.

Report both:

```text
T_compute
source → controller output

T_accept
source → PX4 accepted control boundary

T_effect
physical disturbance onset → first useful correcting action

T_recover
physical disturbance onset → defined recovery threshold
```

For each:

```text
p50
p95
p99
max
jitter
```

This prevents a very cheap reactive PID from being mislabeled as a faster disturbance controller merely because its arithmetic path is short.

## Runtime / embedded

```text
CPU percentage
CPU time/update
memory
allocations
callback count
DDS traffic
missed deadlines
maximum source age
```

## Engineering complexity

```text
source LOC
number of runtime modules
number of state machines
number of tunable parameters
number of callback boundaries
branch/cyclomatic complexity
number of failure modes
forensic time after injected faults
```

---

# 17. QCS8550 deployment policy

QCS8550 provides substantially more compute than this branch should require; Qualcomm lists an 8-core Kryo CPU up to 3.2 GHz and a Hexagon HTP with HVX/HMX, and targets industrial drones among its applications.

The benchmark objective is therefore **not** to consume the available accelerator budget.

Target:

```text
controller arithmetic so small that
scheduler/transport/actuator dynamics dominate compute latency.
```

Deployment order:

```text
CPU scalar reference
→ CPU vectorized implementation if needed
→ fixed-point/SIMD/HVX only if profiling justifies it
```

GPU/NPU use is not allowed by default for the PID/INDI core because it would increase integration complexity without a demonstrated need.

---

# 18. Development phases

## PID-B0 — evidence and identifier preparation

```text
freeze signal semantics
freeze accepted-control boundary
construct closed-loop identification dataset
hold out sessions/regions
```

Exit:

```text
ID data quality sufficient for local models
```

## PID-B1 — identification

```text
ID0/ID1/ID2/ID3 ladder
N/E coupling assessment
delay / actuator-dynamics assessment
held-out residual analysis
```

Exit:

```text
smallest adequate model family frozen for tuning
```

## PID-B2 — robust GA tuning

```text
2-DOF PID
anti-windup ablation
multi-objective GA / NSGA-II
plant ensemble robustness
```

Exit:

```text
P0/P1 gain sets frozen
```

## PID-B3 — scheduler

```text
lookup
linear interpolation
transition testing
bumpless transfer
rate limits
```

Exit:

```text
P2 deterministic and stable on the full declared envelope
```

## PID-B4 — feedforward

```text
reference acceleration
identified drag/model compensation
```

Exit:

```text
P3 retained only if error improves without robustness loss
```

## PID-B5 — INDI

```text
classical INDI
actuator/control-effectiveness sensitivity
```

Exit:

```text
P4 retained only if disturbance-rejection benefit is material
```

## PID-B6 — optional DOB

Run only if P4 has a clear remaining disturbance gap.

## PID-B7 — optional ANN/RBF scheduler

Run only if the simple gain schedule is inadequate.

## PID-B8 — benchmark against current pipeline

```text
P0..P6
vs
C_FAST exact revision
```

Later:

```text
champion PID arm
vs
C_FULL exact qualified revision
```

---

# 19. Decision rule

Do not ask which architecture is theoretically more advanced.

Use Pareto dominance.

A PID branch is a serious replacement candidate if a controller such as P4 satisfies:

```text
tracking accuracy >= current pipeline
AND
disturbance rejection >= current pipeline within declared envelope
AND
p99 source→accepted latency < current pipeline
AND
runtime/engineering complexity materially lower
AND
no new safety/control-authority regression
```

If accuracy is slightly lower but complexity/latency are dramatically better, classify it as a mission-dependent alternative rather than a universal replacement.

If the current pipeline materially wins gust/OOD behavior, retain the PID branch as a simplicity baseline.

---

# 20. Recommended first implementation

The first serious controller to implement after identification is:

```text
RGS-2DOF-PID

2-DOF velocity PID
+ derivative on filtered measurement
+ hybrid/qualified anti-windup
+ robust GA tuned local gains
+ low-dimensional gain interpolation
+ gain bounds/rate limits
+ bumpless transitions
+ bounded model feedforward
```

Then implement:

```text
RGS-2DOF-PID-INDI
```

as the strongest expected benchmark arm.

Do **not** implement ANN gain scheduling before these two arms exist.

---

# 21. Current authority boundary

This branch is research/benchmark only.

It does not change:

```text
current Phase-0 state
Q1 authorization state
Option-B contract
M_STABLE_US / W_MAX_US
AURA/FAST/T1/C1 semantics
WM/WISE/AEGIS roadmap authority
SEALED policy
PX4 inner-loop authority
production_authority=false
```

Large runtime/capture/identification datasets must remain under the Kingston storage root used by the project rather than being committed to this documentation repository.

---

# 22. Primary research sources

1. **Automatic autopilot tuning framework using genetic algorithms and system identification**, Aerospace Science and Technology, 2025. Demonstrates a complete UAV workflow: system identification → simplified flight-dynamics model → GA tuning → real-flight validation. DOI: https://doi.org/10.1016/j.ast.2024.109779

2. **Data-driven identification of quadrotor dynamics: a tutorial**, IFAC-PapersOnLine, 2024. Focuses specifically on identifying quadrotor dynamics from experimental closed-loop data. DOI: https://doi.org/10.1016/j.ifacol.2024.08.533

3. **Cascaded incremental nonlinear dynamic inversion for MAV disturbance rejection**, Control Engineering Practice, 2018. Experimental quadrotor wind-tunnel disturbance rejection; strong INDI comparator/augmentation source. DOI: https://doi.org/10.1016/j.conengprac.2018.01.003

4. **Nonlinear dynamic inversion control with unknown control effectiveness and actuator dynamic**, Aerospace Science and Technology, 2025. Motivates actuator-dynamics/control-effectiveness-aware incremental inversion. DOI: https://doi.org/10.1016/j.ast.2025.110036

5. **Torque-based INDI for UAV Control: Eliminating Pseudo-inverses and Improving Control Gain Selection**, International Journal of Control, Automation and Systems, 2025. Flight-tested INDI variant with practical gain-selection guidance. DOI: https://doi.org/10.1007/s12555-024-0525-9

6. **Improving Incremental Nonlinear Dynamic Inversion Robustness Using Robust Control in Aerial Robotics**, 2025. INDI + structured low-order H-infinity outer-loop research with experimental quadrotor evaluation and reported gust-rejection improvement. arXiv: https://arxiv.org/abs/2501.07223

7. **Aggressive flight control of quadrotors using incremental nonlinear dynamic inversion with a high-fidelity thrust unit model**, Journal of the Franklin Institute, 2024. Shows the importance of actuator/thrust-unit model fidelity for aggressive INDI tracking. DOI: https://doi.org/10.1016/j.jfranklin.2024.106914

8. **PID control: Resilience with respect to controller implementation**, Frontiers in Control Engineering, 2022. Discusses practical 2-DOF PID structures, setpoint weighting and derivative filtering. https://www.frontiersin.org/journals/control-engineering/articles/10.3389/fcteg.2022.1061830/full

9. **Anti-Windup in PID Control: Review, Analysis, and New Tuning Directions**, 2026. Comparative anti-windup analysis and systematic tuning directions. https://arxiv.org/abs/2606.01959

10. **Optimal PID axis Control for UAV Quadrotor based on Multi-Objective PSO**, IFAC-PapersOnLine, 2022. Demonstrates multi-objective tuning involving settling time, overshoot, steady-state error and control effort. DOI: https://doi.org/10.1016/j.ifacol.2022.07.590

11. **Tuning of cascade PID controller gains of quadcopter under bounded disturbances using metaheuristic based research algorithm**, The Aeronautical Journal, 2025. Compares offline metaheuristic tuning using IAE/ISE/ITAE/ITSE and tests robustness under disturbances/parameter uncertainty.

12. **Evolutionary design of marginally robust multivariable PID controller**, Engineering Applications of Artificial Intelligence, 2023. Supports including explicit robustness/stability margins in evolutionary PID design. DOI: https://doi.org/10.1016/j.engappai.2023.105938

13. **Comparison of Parameter-Varying Decoupling Based Control Schemes for a Quadrotor**, IFAC-PapersOnLine, 2018. Shows parameter-varying/gain-scheduled decoupling can improve performance over fixed LTI control in high-speed quadrotor maneuvers. DOI: https://doi.org/10.1016/j.ifacol.2018.11.168

14. **Robust linear parameter varying attitude control of a quadrotor UAV with state constraints and input saturation subject to wind disturbance**, Transactions of the Institute of Measurement and Control, 2020. Supports LPV scheduling with explicit constraints/saturation. DOI: https://doi.org/10.1177/0142331219883452

15. **Integrated Physics-Data Based LPV Attitude Control of Quadrotor UAV System**, IEEE Transactions on Industrial Electronics, 2025. Relevant to a later physics+data gain-scheduling challenger. DOI: https://doi.org/10.1109/TIE.2025.3544211

16. **Genetic Algorithm Based Radial Basis Function Neural Network PID Control System for a Quadrotor UAV**, 2025/2026. Supports RBF/ANN scheduling as a challenger, not as the default baseline. DOI: https://doi.org/10.1109/CCNIS69465.2025.00082

17. **Qualcomm Dragonwing QCS8550 official specification**. QCS8550 includes Kryo CPU up to 3.2 GHz and Hexagon HTP with HVX/HMX and is positioned for industrial drones. https://www.qualcomm.com/internet-of-things/products/q8-series/qcs8550

---

# 23. Research hypothesis

```text
H_PID_BENCHMARK:

A robust data-identified, GA-tuned, gain-scheduled 2-DOF PID with model feedforward
and INDI augmentation can approach or exceed the current fast pipeline's tracking and
disturbance-rejection performance while materially reducing software latency, runtime
state complexity and onboard compute.
```

This hypothesis is intentionally falsifiable.
