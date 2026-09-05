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

```text
low-latency disturbance estimate for FAST
+
source-bound state for StateBank / downstream prediction
```

The fast W20 observable is causal and onset/change sensitive. Longer continuity behavior is supplied by the retained FAST/T1/C1 baseline rather than assuming W20 alone represents persistent wind indefinitely.

### AEGIS FAST/T1/C1

```text
FAST
= immediate bounded acceleration correction from current AURA state

T1/C1 baseline
= retained qualified continuation/bridge behavior active during WM experiments

candidate path
= bounded incremental predictive contribution accepted only with exact identity/provenance contract
```

The experimental baseline remains:

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

It enforces source/session/reset identity and a causal snapshot barrier.

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

### WISE

WISE consumes current StateBank state plus WM rollouts and evaluates bounded candidate plans against trajectory, uncertainty and control constraints.

```text
causal state/history
→ generate bounded candidates
→ WM rollout
→ score tracking / velocity / cross-track / effort / uncertainty / constraints
→ select acceptable plan
→ AEGIS bounded execution path
```

## 5. Fast path vs predictive path

### Fast path

React immediately to detected disturbance with minimal latency. It remains causal and functional without WM.

### Predictive path

Use recent state/action history to predict future trajectory response and reduce future error. It refines rather than delays the immediate response.

## 6. Scientific interpretation

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

The active baseline is present in both arms. A randomized candidate may trigger additional FAST/PX4 reactions after treatment; those downstream reactions are part of the realized treatment path and are not removed as pre-treatment confounding.

## 7. Timing and causal identity

```text
T_D = causal decision frontier
T_A = actual accepted-action frontier
```

Source continuity uses native PX4/source identity. Cross-domain clock alignment is separate and must carry explicit provenance.

Host time, Gazebo simulation time and PX4 boot/source time must not be silently collapsed into one timestamp domain.

## 8. E8 source-causal pairing

E8 uses a bounded immutable AURA callback ledger for decision-authoritative AURA/C1 pairing.

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

## 9. Native-event lifecycle

The inter-block lifecycle is now qualified as:

```text
arm
→ consume/onset
→ exact matching native CLEAR
→ clear
→ complete
→ retire
→ next block may arm
```

This is an inter-block readiness invariant only; it does not alter frozen within-block GUST/treatment timing.

Bounded qualification demonstrated three consecutive GUST blocks with zero `PREVIOUS_EVENT_STILL_ACTIVE` rejection and zero overlap.

## 10. Applied-status mirror and next-status successor contract

The scientific/qualification observer requires a strict source-forward successor matching:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

The E8 mirror now publishes each native status to the observer path before applying the existing ingress/source-health/ACK gate. This preserves visibility of the canonical status-successor contract without changing native authority or control semantics.

Important separation:

```text
observer/mirror visibility contract
!=
runtime source-health / application authority gate
```

The latest bounded qualification produced 689 strict-future successor lookups with zero timeout and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

## 11. Causal-validity pipeline

The WM validity engine is a canonical offline/preflight/post-run mechanism:

```text
reverse validity indexing
→ canonical source-grounded dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

Canonical graph shape is 21 nodes / 34 edges with fail-closed tri-state precedence:

```text
FAIL > UNKNOWN > PASS
```

This machinery must not enter the FAST/control hot path.

## 12. Failure isolation

Failures remain classified by layer:

```text
source / transport
clock alignment
AURA state validity
StateBank causality/readiness
C1 lifecycle/replay
E8 AURA/C1 pairing
applied-status mirror / next-status successor
candidate binding / accepted status
native-event lifecycle
exact exposure
PX4 projection / constraints
scientific target validity
model/statistical adequacy
```

Examples:

```text
Timesync mapping transition != native source loss
bootstrap readiness failure != treatment failure
invalid infrastructure root != evidence candidate has no physical effect
weak treatment signal != infrastructure invalidity
terminal timeout != necessarily first causal divergence
```

## 13. Current integration state

Qualified infrastructure now includes:

```text
Option-B / Direct Guard bounded delayed-launch mechanism
AURA_C1_SOURCE_RESET authority
WM reverse-processing + Tarjan + peeling validity engine
continuous-C1 replay/recovery qualification
TRACE_QOS_DEPTH=4096 diagnostic evidence path
post-reset E8 source-causal AURA/C1 pairing
post-reset accepted-status handoff
native-event CLEAR/retirement lifecycle gate
next-status mirror/successor-frontier repair
StateBank seven-stream startup barrier
bootstrap_only session-start path
canonical plugin-bearing scientific world
```

Latest scientific root `fresh_34` passed preflight but stopped in its first CALM row because the old E8 mirror filtered contract-valid strict-future statuses before observer publication. The root remains `INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE` with no scientific data admitted.

That implementation defect has since been repaired and separately qualified:

```text
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
PRE_RETRY_VALID_CAUSAL_CORE=true
```

The owner has authorized one new fresh randomized scientific root. It has not yet been executed.

Current immediate execution state:

```text
new immutable root
→ full frozen preflight
→ exact 8-session / 96-block matrix
→ stop on first invalid block
→ canonical reverse/Tarjan/peeling audit
→ if and only if 96/96 valid: separate causal-dataset admission
```

## 14. Post-Phase-0 FASTv2 research hypothesis

The current production/scientific FAST baseline is unchanged. A future challenger has been identified from the main AURA design plus the independent PID benchmark research:

```text
PX4 firmware PID/inner loops unchanged
+
AURA disturbance feedforward (-d_hat)
+
T1/C1 temporal continuation
+
bounded residual 2-DOF PI at the qualified acceleration-correction boundary
```

Conceptually:

```text
a_FASTv2 = -d_hat + a_T1/C1 + a_residual_2DOF_PI
```

Initial research constraints:

```text
fixed robust PI gains first
explicit saturation and anti-windup
source/session/reset/lifecycle-aware integrator state
identify residual closed-loop plant with current PX4+AURA+FAST/T1/C1 active
no gain scheduling until a deployable causal scheduling state is supported
INDI only as a later ablation challenger
DOB/ESO/ANN only if a measured residual failure class remains
```

This is not part of current Phase-0 authority. Promoting it would change baseline `B` and therefore requires a new versioned control/scientific contract after current randomized identification closes.

## 15. Forward architecture path

After one complete valid randomized root and causal-dataset acceptance:

```text
G_action identification
→ F_nominal + G_action World Model
→ WISE bounded predictive refinement
→ end-to-end latency/AoI/freshness characterization
→ uncertainty calibration
→ FASTv2 / causal-learning/adaptation research under new contracts
→ stronger AEGIS runtime assurance
```

World-Model training must not begin from partial or infrastructure-invalid roots.
