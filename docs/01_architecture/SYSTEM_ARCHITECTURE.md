# System Architecture — Moving Mode vNext

## 1. Scope

This document defines the canonical Moving-Mode architecture for the current AURA–WISE–World Model–AEGIS pipeline. It is the primary structural reference for implementation, debugging and scientific interpretation.

Current runtime state and next allowed action are defined by `../00_overview/CURRENT_STATUS.md` and the latest execution ladder, not by this architecture file.

The design separates two responsibilities:

1. **Immediate disturbance response** — causal, low-latency and independent of World-Model availability.
2. **Predictive refinement** — uses causal state history and learned action-conditioned dynamics to improve future trajectory behavior.

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

If the World Model is unavailable, stale, uncertain, still warming, misses a deadline or has no acceptable candidate, this path must remain functional:

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

This equality is exact only at the requested composition boundary. The architecture does not assume physical closed-loop linearity:

```text
Y(B+U) = Y(B) + Y(U)
```

### 3.4 StateBank is causal and always warm

StateBank retains native-time causal history so WM/WISE can evaluate current state and recent treatment/controller history without waiting for an event to initialize memory.

## 4. Module responsibilities

### PX4

PX4 is the final realtime flight-control authority. Runtime/build identity is frozen per scientific root.

The qualified FAST/candidate correction is applied in the position-control acceleration path before PX4 `_accelerationControl()`; downstream attitude/rate/allocation remain native PX4 behavior.

### AURA

AURA estimates current external/disturbance-equivalent acceleration and carries validity, freshness, confidence and causal identity.

```text
low-latency disturbance estimate for FAST
+
source-bound state for StateBank / downstream prediction
```

The fast observable is onset/change sensitive. Longer continuity behavior is supplied by the retained FAST/T1/C1 baseline rather than assuming the fast estimate alone represents persistent wind indefinitely.

### AEGIS FAST/T1/C1

```text
FAST
= immediate bounded acceleration correction from current AURA state

T1/C1 baseline
= retained qualified continuation/bridge behavior active during current WM experiments

candidate path
= bounded incremental predictive contribution accepted only with exact identity/provenance contract
```

Current scientific baseline:

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

WISE consumes causal StateBank state plus WM rollouts and evaluates bounded candidate plans against trajectory, uncertainty and control constraints.

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

The active baseline is present in both arms. Randomized candidate action may cause additional FAST/PX4 reactions after treatment; those downstream reactions are part of the realized treatment path.

A material change to FAST semantics changes baseline `B`. Therefore a future promoted FAST algorithm cannot be assumed to preserve the same `G_action` mapping without a versioned baseline/model review.

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

The qualified inter-block lifecycle is:

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

## 10. Applied-status mirror and next_status contract

The `next_status` observer requires a strict source-forward successor:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

E8 mirror publication occurs before the existing runtime source-health/application-authority gate so observer visibility and runtime application authority remain separate contracts.

The bounded next-status qualification produced 689 strict-future lookups with zero timeout and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

## 11. Accepted-cycle status visibility

`fresh_35` exposed a separate infrastructure boundary earlier than `next_status`.

The accepted-cycle probe requests a same-lineage status within the existing source-match budget. A valid native status existed within that budget, but the probe matcher returned no match and timed out.

Current implementation facts relevant to the forensic are:

```text
accepted-cycle matcher scans self.statuses[-2000:]
per-lineage latest slot exists separately
find_cycle does not use the per-lineage latest slot
```

The evidence does **not** yet prove whether the contract-valid status:

```text
never reached the observer callback
was received and later evicted from bounded retention
or remained retained but could not be selected by matcher/indexing logic
```

This is the current implementation forensic boundary. No timeout/QoS/source predicate change is authorized as a substitute for proving it.

## 12. Causal-validity pipeline

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

## 13. Failure isolation

Failures remain classified by layer:

```text
source / transport
clock alignment
AURA state validity
StateBank causality/readiness
C1 lifecycle/replay
E8 AURA/C1 pairing
applied-status mirror / next_status successor
accepted-cycle callback/retention/matcher visibility
candidate binding / accepted status
native-event lifecycle
exact exposure
PX4 projection / constraints
scientific target validity
model/statistical adequacy
```

Examples:

```text
producer status exists != probe callback visibility proven
bounded capacity pressure != eviction proven
terminal timeout != first causal divergence
invalid infrastructure root != negative treatment result
weak treatment signal != infrastructure invalidity
```

## 14. Current integration state

Qualified infrastructure retained before `fresh_35` includes:

```text
Option-B / Direct Guard
AURA_C1_SOURCE_RESET authority
WM reverse-processing + Tarjan + peeling validity engine
continuous-C1 replay/recovery qualification
TRACE_QOS_DEPTH=4096 diagnostic evidence path
post-reset E8 source-causal AURA/C1 pairing
post-reset accepted-status handoff
native-event CLEAR/retirement lifecycle gate
next_status mirror/successor-frontier repair
StateBank seven-stream startup barrier
bootstrap_only session-start path
canonical plugin-bearing scientific world
```

Latest root `fresh_35` passed preflight and stopped in the first CALM row before scientific admission:

```text
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN
```

The next executable state is a separate accepted-cycle callback visibility/retention forensic, followed by minimal repair and bounded non-scientific qualification only if a concrete implementation defect is proven.

## 15. FAST research boundary

The current production/scientific FAST baseline is unchanged.

Separate simulator shadow/replay research may benchmark alternative FAST algorithms against the current `-d_hat + T1/C1` path. The purpose is to determine whether immediate wind response, tracking and recovery can be materially improved while keeping PX4 firmware control semantics unchanged.

No replacement algorithm is currently selected. The earlier residual-PI proposal is not current main-pipeline direction.

Any future FAST promotion would change baseline `B` and requires a versioned control/scientific contract after current Phase-0 identification closes.

## 16. Forward architecture path

Current sequence:

```text
accepted-cycle visibility/retention forensic
→ implementation-preserving repair if proven
→ bounded non-scientific qualification
→ owner review
→ later fresh randomized root only if separately authorized
→ complete causal dataset admission
→ G_action identification
→ F_nominal + G_action World Model
→ WISE bounded predictive refinement
```

FAST shadow research may proceed independently but must not change the current Phase-0 baseline.

World-Model training must not begin from partial or infrastructure-invalid roots.
