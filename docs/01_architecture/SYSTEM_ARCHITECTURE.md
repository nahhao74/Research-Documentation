# System Architecture — Moving Mode vNext

## 1. Scope

This document defines the canonical structural architecture of the AURA–WISE–World Model–AEGIS Moving-Mode pipeline.

It defines module ownership, data/control flow and invariants. It does **not** define the latest runtime blocker or next executable task.

Current execution state is defined only by:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

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
                                                                    └──> sensor/state feedback
```

The fast and predictive paths coexist. The predictive path augments the active controller; it does not replace first-response control.

## 3. Fundamental invariants

### 3.1 World Model cannot block first response

The following path must remain functional when WM/WISE is unavailable, stale, uncertain, warming or late:

```text
AURA -> AEGIS FAST/T1/C1 -> PX4 -> UAV
```

### 3.2 PX4 remains authoritative

FAST and candidate corrections enter only through the qualified bounded acceleration-correction path.

PX4 retains authority over:

```text
position/velocity control internals
acceleration-to-thrust conversion
attitude/rate control
control allocation
actuator limits and motor output
```

No AURA/WM/WISE/AEGIS candidate owns direct motor/raw-thrust authority.

### 3.3 Candidate action is incremental

At the requested composition boundary:

```text
u_baseline = FAST/T1/C1 requested contribution
u_total_requested = u_baseline + u_candidate
```

This does not imply physical linear superposition:

```text
Y(B+U) = Y(B) + Y(U)
```

is not assumed.

### 3.4 StateBank is causal and always warm

StateBank retains causal native-time history so predictive/scientific consumers do not need to initialize memory after a new event.

### 3.5 Fail-closed degradation

If a predictive candidate is stale, invalid, uncertain or unavailable:

```text
u_candidate = ZERO / unavailable
baseline continues
```

## 4. Module ownership

### PX4

PX4 is the final realtime flight-control authority.

FAST/candidate acceleration corrections enter before `_accelerationControl()` in the qualified PositionControl acceleration path. Downstream thrust conversion, attitude/rate control and allocation remain native PX4 behavior.

### AURA

AURA produces a causal disturbance-equivalent acceleration estimate plus:

```text
validity
freshness
confidence
source/session/reset identity
```

AURA supports:

```text
low-latency FAST response
+
source-bound state for StateBank / prediction
```

### FAST/T1/C1

```text
FAST
= immediate bounded acceleration correction from current AURA state

T1/C1
= qualified continuation/bridge behavior retained in the active baseline
```

Current scientific baseline definition:

```text
B = active PX4 + AURA + FAST/T1/C1 closed loop
```

The exact current FAST semantics are frozen by the active scientific contract during Phase-0.

### StateBank

StateBank stores causal source-bound history for:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

It enforces source/session/reset identity and an atomic causal snapshot barrier.

### World Model

Canonical decomposition:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

where:

```text
F_nominal = future evolution under active baseline B
G_action  = incremental candidate effect relative to ZERO under B
```

### WISE

WISE performs bounded predictive candidate selection:

```text
causal StateBank state
→ bounded candidates
→ WM rollout
→ score future tracking / effort / uncertainty / constraints
→ choose acceptable plan
→ AEGIS candidate path
```

### AEGIS

AEGIS is the bounded execution/assurance boundary for FAST and predictive candidate contributions.

It owns acceptance/bounds/provenance enforcement at the qualified acceleration-correction interface; it does not replace PX4 inner-loop authority.

## 5. Fast path vs predictive path

### Fast path

Purpose:

```text
respond to disturbance now
minimize first useful correction latency
remain functional without WM
```

### Predictive path

Purpose:

```text
predict controlled future response
anticipate future tracking/constraint consequences
add bounded refinement only when useful and valid
```

The predictive path must improve future behavior without delaying the fast path.

## 6. Scientific interpretation

The current action-conditioned estimand is:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

The active baseline exists in both arms.

Candidate-induced downstream FAST/PX4 reactions are part of the realized closed-loop treatment response.

A material FAST change changes baseline `B`; action-conditioned data/model validity must therefore be reviewed under the new baseline before production use.

## 7. Timing and causal identity

```text
T_D = causal decision frontier
T_A = actual accepted-action frontier
```

Native PX4/source time is the authority for source continuity. Cross-domain clock alignment is a separate provenance problem.

Never silently collapse:

```text
PX4 boot/source time
Gazebo simulation time
host receipt/monotonic time
mapped cross-domain time
```

Detailed rules live in `TIMING_CAUSALITY_STATEBANK.md`.

## 8. E8 source-causal pairing

For a C1 source frontier:

```text
select newest received positive-source nonfuture AURA record
→ then apply exact session/reset/validity/freshness/provenance/clock gates
```

Hard rules:

```text
no future AURA sample
no cross-reset carryover
no older-favorable fallback when the newest causal state fails
```

## 9. Applied-status observation

Observer visibility and runtime application authority are separate contracts.

Canonical `next_status` successor predicate:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

Infrastructure that retains/selects accepted-cycle statuses must preserve exact generation/session/reset/source semantics rather than changing the scientific contract to accommodate storage or callback behavior.

## 10. Native-event lifecycle

Qualified inter-block lifecycle:

```text
arm
→ consume/onset
→ exact matching native CLEAR
→ clear
→ complete
→ retire
→ next block may arm
```

This is an inter-block readiness invariant, not a change to within-block treatment timing.

## 11. Canonical causal-validity engine

Offline/preflight/post-run validation uses:

```text
reverse validity indexing
→ canonical dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

This machinery is not part of the control hot path.

See `WM_CAUSAL_VALIDITY_ENGINE.md`.

## 12. Failure-isolation layers

Classify failures at the earliest proven layer:

```text
source / transport
clock alignment
AURA validity
StateBank causality/readiness
C1 lifecycle/replay
E8 source-causal pairing
status observation / retention / matching
candidate binding / accepted status
native-event lifecycle
exact treatment exposure
PX4 projection / constraints
scientific validity
model/statistical adequacy
```

Do not equate:

```text
terminal timeout with root cause
capacity pressure with proven eviction
infrastructure failure with scientific negative result
weak treatment signal with infrastructure invalidity
```

## 13. Research boundary

FAST improvement and WM/WISE development follow the active roadmap but do not silently modify this canonical architecture.

Any material change to:

```text
PX4 authority
FAST/T1/C1 semantics
candidate insertion boundary
scientific baseline B
World-Model treatment semantics
```

requires explicit versioning/review and qualification.
