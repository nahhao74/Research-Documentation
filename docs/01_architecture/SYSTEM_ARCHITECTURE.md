# System Architecture — Moving Mode vNext

## 1. Scope

This document defines the canonical Moving-Mode architecture for the current AURA–WISE–World Model–AEGIS pipeline. It is the primary structural reference for implementation, debugging and scientific interpretation.

The design separates two responsibilities:

1. **Immediate disturbance response**: causal, low-latency, robust to WM unavailability.
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
                                        │                       AEGIS ActivePlan
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

If the World Model is unavailable, stale, uncertain, still warming, misses a deadline, or has no acceptable candidate, the following path must remain functional:

```text
AURA -> AEGIS FAST/T1/C1 -> PX4 -> UAV
```

This is a hard architecture property, not a tuning preference.

### 3.2 PX4 remains authoritative

AEGIS and WM do not directly command motors, raw thrust, attitude or rate loops. Candidate and FAST corrections enter only through the qualified bounded acceleration-correction path. PX4 retains authority over:

- velocity/position control internals;
- acceleration-to-thrust conversion;
- attitude/rate control;
- control allocation;
- actuator limits and motor output.

### 3.3 Candidate is incremental

The candidate is composed on top of the active baseline:

```text
u_baseline = FAST/T1/C1 requested contribution
u_total_requested = u_baseline + u_candidate
```

This equality is exact only at the requested composition boundary. The physical closed-loop response is nonlinear, therefore the architecture does **not** assume:

```text
Y(B+U) = Y(B) + Y(U)
```

### 3.4 StateBank is causal and always warm

StateBank retains a causal native-time history so WM/WISE can evaluate current state and recent treatment/controller history without waiting for a new event to initialize memory.

## 4. Module responsibilities

### PX4

PX4 is the final realtime flight-control authority. The project currently uses PX4 v1.15 lineage with exact runtime/build identities frozen per scientific root.

Important semantic boundary: the qualified FAST/candidate correction is applied in the position-control acceleration path before PX4 `_accelerationControl()`; downstream attitude/rate/allocation remain native PX4 behavior.

### AURA

AURA estimates the current external/disturbance-equivalent acceleration effect and carries validity, freshness, confidence and causal identity.

AURA serves two roles:

1. provide a low-latency current disturbance estimate for AEGIS FAST;
2. provide source-bound state to StateBank and downstream predictive modeling.

The fast W20 observable is causal and onset/change sensitive. A sustained disturbance may become absorbed by its recent baseline; therefore longer continuity behavior is supplied by the retained FAST/T1/C1 baseline rather than assuming W20 alone represents persistent wind indefinitely.

### AEGIS FAST/T1/C1

AEGIS is the response/control mediation layer.

- **FAST**: immediate bounded acceleration correction derived from current AURA disturbance.
- **T1/C1 baseline**: retained qualified continuation/bridge behavior that remains active during WM experiments.
- **Candidate path**: bounded incremental contribution generated by the predictive layer and accepted only with exact source/session/generation binding.

The experimental baseline `B` is the entire active PX4 + AURA + FAST/T1/C1 closed-loop system.

### StateBank

StateBank stores source-bound causal state across required streams:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

It enforces source/session/reset identity and a causal snapshot barrier. Bootstrap readiness requires all mandatory streams to be present before a session-start snapshot can be accepted.

### World Model

The current WM decomposition is:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

where:

- `F_nominal` models future evolution of the actively controlled baseline system;
- `G_action` models the incremental closed-loop effect of a bounded candidate action plan.

This is deliberately different from learning an open-loop vehicle plant while pretending PX4/FAST are absent.

### WISE

WISE is the predictive planning/scoring layer. It consumes current StateBank state plus WM rollouts and evaluates candidate plans against trajectory, uncertainty and control constraints.

Conceptually it can perform model-predictive reasoning without requiring a heavy online nonlinear MPC implementation. A typical role is:

```text
state/history
   -> generate bounded candidate U_plan
   -> WM rollout
   -> score tracking error / velocity / cross-track / effort / uncertainty / constraints
   -> choose acceptable plan
   -> AEGIS bounded application
```

## 5. Fast path vs predictive path

### Fast path objective

React immediately to a detected disturbance with minimal latency. Accuracy may be lower than a full predictive optimizer, but the path must be causal and fast.

### Predictive path objective

Use recent state and action history to predict future trajectory response and reduce error over upcoming horizons. The predictive path should refine rather than delay the immediate reaction.

This division is essential for real UAV operation because optimization latency and model uncertainty cannot be allowed to disable first response.

## 6. Scientific interpretation of the architecture

The causal target is:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

with the baseline closed loop active in both arms.

A randomized candidate may trigger additional FAST/PX4 reactions after treatment. Those downstream reactions are part of the realized treatment path and should not be removed as if they were pre-treatment confounding.

Pre-treatment AURA/FAST/controller state may be used as context or moderation variables because it exists before assignment.

## 7. Failure isolation

The architecture is designed so failures can be classified by layer:

```text
source / transport
clock alignment
AURA state validity
StateBank causality/readiness
candidate binding / E8
exact exposure
PX4 projection / constraints
scientific target validity
model/statistical adequacy
```

A failure in one layer should not be silently relabeled as another. In particular:

- Timesync mapping transition != native source loss;
- bootstrap readiness failure != treatment failure;
- invalid infrastructure root != evidence candidate has no physical effect;
- weak treatment signal != infrastructure invalidity.

## 8. Current integration state

Qualified current infrastructure includes:

- incremental candidate live mechanism on active FAST/T1/C1 baseline;
- dual-domain timestamp authority;
- atomic `SensorCombinedStampedV1` provenance wire;
- live source continuity qualification;
- StateBank seven-stream startup barrier;
- specialized pilot `bootstrap_only=True` call-site closure and exact runner preflight.

The next unresolved gate is not a structural architecture question. It is execution of one complete valid randomized pilot to determine whether the bounded candidate produces usable action-conditioned signal for WM training.