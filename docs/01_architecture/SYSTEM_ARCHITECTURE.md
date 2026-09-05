# System Architecture — Moving Mode vNext

## 1. Scope

This document defines the canonical Moving-Mode architecture for the current AURA–WISE–World Model–AEGIS pipeline. It is the primary structural reference for implementation, debugging and scientific interpretation.

Current runtime state and next allowed action are defined by `../00_overview/CURRENT_STATUS.md` and the latest execution ladder, not by this architecture file.

The design separates two responsibilities:

1. **Immediate disturbance response**: causal, low-latency, robust to World-Model unavailability.
2. **Predictive refinement**: uses causal state history and learned action-conditioned dynamics to improve future trajectory behavior.

## 2. End-to-end structure

```text
                                      PREDICTIVE PATH
Sensors / PX4 / Reference ───────────────┬───────────────────────────────┐
                                        │                               │
                                        │                               v
                                        │                       StateBank (warm)
                                        │                               │
                                        │                               v
                                        │                       World Model / WISE
                                        │                               │
                                        │                         candidate U_plan
                                        │                               │
                                        │                               v
                                        │                       AEGIS candidate path
                                        │                               │
                                        v                               v
Sensors / PX4 / Reference ────────────> AURA ───────────────> AEGIS FAST/T1/C1
                                                                    │
                                                                    v
                                                                   PX4
                                                                    │
                                                                    v
                                                                   UAV
                                                                    │
                                                                    └──> new sensor/state feedback
```

The fast and predictive paths coexist. The predictive path augments the active controller; it does not replace the baseline response stack.

## 3. Fundamental architectural invariants

### 3.1 World Model cannot block first response

If the World Model is unavailable, stale, uncertain, still warming, misses a deadline, or has no acceptable candidate, this path must remain functional:

```text
AURA -> AEGIS FAST/T1/C1 -> PX4 -> UAV
```

This is a hard architecture property.

### 3.2 PX4 remains authoritative

AEGIS and WM do not directly command motors, raw thrust, attitude or rate loops. Candidate and FAST corrections enter only through the qualified bounded acceleration-correction path. PX4 retains authority over:

```text
velocity/position control internals
acceleration-to-thrust conversion
attitude/rate control
control allocation
actuator limits and motor output
```

### 3.3 Candidate is incremental

```text
u_baseline = FAST/T1/C1 requested contribution
u_total_requested = u_baseline + u_candidate
```

This equality is exact only at the requested composition boundary. The physical closed-loop response is nonlinear, so the architecture does not assume:

```text
Y(B+U) = Y(B) + Y(U)
```

### 3.4 StateBank is causal and always warm

StateBank retains causal native-time history so WM/WISE can evaluate current state and recent treatment/controller history without waiting for a new event to initialize memory.

## 4. Module responsibilities

### PX4

PX4 is the final realtime flight-control authority. Runtime/build identity is frozen per scientific root.

The qualified FAST/candidate correction is applied in the position-control acceleration path before PX4 `_accelerationControl()`; downstream attitude/rate/allocation remain native PX4 behavior.

### AURA

AURA estimates the current external/disturbance-equivalent acceleration effect and carries validity, freshness, confidence and causal identity.

AURA serves two roles:

```text
low-latency disturbance estimate for FAST
+
source-bound state for StateBank / downstream prediction
```

The fast W20 observable is causal and onset/change sensitive. Longer continuity behavior is supplied by the retained FAST/T1/C1 baseline rather than assuming W20 alone represents persistent wind indefinitely.

### AEGIS FAST/T1/C1

AEGIS is the response/control mediation layer.

```text
FAST
= immediate bounded acceleration correction from current AURA state

T1/C1 baseline
= retained qualified continuation/bridge behavior active during WM experiments

candidate path
= bounded incremental predictive contribution accepted only with exact identity/provenance contract
```

The experimental baseline is:

```text
B = active PX4 + AURA + FAST/T1/C1 closed loop
```

### StateBank

StateBank stores source-bound causal state across:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

It enforces source/session/reset identity and a causal snapshot barrier. Bootstrap readiness requires all mandatory streams to be present before session-start snapshot acceptance.

### World Model

Current decomposition:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

where:

```text
F_nominal
= future evolution of the active baseline system

G_action
= incremental closed-loop effect of bounded candidate action
```

This is deliberately different from learning an open-loop airframe while pretending PX4/FAST are absent.

### WISE

WISE is the predictive planning/scoring layer. It consumes current StateBank state plus WM rollouts and evaluates bounded candidate plans against trajectory, uncertainty and control constraints.

Conceptually:

```text
causal state/history
→ generate bounded candidates
→ WM rollout
→ score tracking / velocity / cross-track / effort / uncertainty / constraints
→ select acceptable plan
→ AEGIS bounded execution path
```

WISE is model-predictive in function; it does not require a heavyweight online nonlinear optimizer if a smaller candidate library/search policy meets latency and quality requirements.

## 5. Fast path vs predictive path

### Fast path

React immediately to detected disturbance with minimal latency. It must remain causal and functional without WM.

### Predictive path

Use recent state/action history to predict future trajectory response and reduce future error. It refines rather than delays the immediate response.

This division is essential because optimization latency and model uncertainty must never disable first response.

## 6. Scientific interpretation

The causal target is:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

with the active baseline present in both arms.

A randomized candidate may trigger additional FAST/PX4 reactions after treatment. Those downstream reactions are part of the realized treatment path and must not be removed as pre-treatment confounding.

Pre-treatment AURA/FAST/controller state may be context/moderation variables because it exists before assignment.

## 7. Timing and causal identity

The architecture distinguishes:

```text
T_D = causal decision frontier
T_A = actual accepted-action frontier
```

Source continuity is native PX4/source identity. Cross-domain clock alignment is separate and must carry explicit provenance.

Host time, Gazebo simulation time and PX4 boot/source time must not be silently collapsed into one timestamp domain.

## 8. E8 source-causal pairing

E8 currently uses a bounded immutable AURA callback ledger for decision-authoritative AURA/C1 pairing.

Conceptually:

```text
for C1 frontier T_C1
select newest received positive-source AURA with T_AURA <= T_C1
then apply existing exact session/reset/validity/freshness/provenance/clock gates
```

Hard rules:

```text
no future AURA sample
no cross-reset carryover
no fallback to an older favorable AURA state when the newest causal state fails
```

`self._aura` may remain for diagnostic/API compatibility but is not the decision-authoritative pairing source.

## 9. Native-event lifecycle

Native GUST events are physical simulation events with explicit lifecycle identity. A later block may not assume readiness merely because the previous block's local transaction logic advanced.

Current required invariant under repair/qualification is:

```text
previous exact native event ACTIVE
→ matching canonical CLEAR / retirement
→ next native-event arm eligibility
```

This is inter-block readiness and must not silently alter frozen within-block GUST/treatment timing.

## 10. Failure isolation

Failures should remain classified by layer:

```text
source / transport
clock alignment
AURA state validity
StateBank causality/readiness
C1 lifecycle/replay
E8 AURA/C1 pairing
candidate binding / accepted status
native-event lifecycle
exact exposure
PX4 projection / constraints
scientific target validity
model/statistical adequacy
```

A failure in one layer must not be silently relabeled as another.

Examples:

```text
Timesync mapping transition != native source loss
bootstrap readiness failure != treatment failure
invalid infrastructure root != evidence candidate has no physical effect
weak treatment signal != infrastructure invalidity
terminal timeout != necessarily first causal divergence
```

## 11. Current integration state

Qualified infrastructure before the latest scientific retry includes:

```text
Option-B / Direct Guard bounded delayed-launch mechanism
AURA_C1_SOURCE_RESET authority
WM reverse-processing + peeling validity engine
status-observer/source-frontier retention repair
continuous-C1 replay/recovery qualification
TRACE_QOS_DEPTH=4096 diagnostic evidence path
post-reset E8 source-causal AURA/C1 pairing
post-reset accepted-status handoff
StateBank seven-stream startup barrier
bootstrap_only session-start path
canonical plugin-bearing scientific world
```

Latest randomized root `fresh_33` passed preflight and reached the first `GUST_E` row, then stopped fail-closed when block 3 tried to arm before block 2's native event reached canonical CLEAR.

Current immediate gate:

```text
native-event lifecycle ownership audit
→ implementation-preserving CLEAR/retirement readiness repair
→ deterministic regression
→ bounded non-scientific consecutive-event qualification
→ PRE_RETRY_VALID_CAUSAL_CORE=true
→ owner review
→ only then next fresh complete randomized root
```

The unresolved gate is therefore **not** a structural architecture redesign and **not** a World-Model algorithm question. It is current inter-block native-event lifecycle synchronization.

## 12. Forward architecture path

After one complete valid randomized root and causal-dataset acceptance:

```text
G_action identification
→ F_nominal + G_action World Model
→ WISE bounded predictive refinement
→ latency/AoI/freshness characterization
→ uncertainty calibration
→ causal-learning/adaptation research
→ stronger AEGIS runtime assurance
```

World-Model training must not begin from partial or infrastructure-invalid roots.
