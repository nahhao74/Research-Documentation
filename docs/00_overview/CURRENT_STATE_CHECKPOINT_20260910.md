# Current State Checkpoint — 2026-09-10

## Executive decision

The program is temporarily switching from qualification-first execution to **engineering FAST characterization and challenger selection**.

```text
ENGINEERING_BUILD_MODE=ACTIVE
FORMAL_PHASE_D_QUALIFICATION=PAUSED_NOT_DELETED
CURRENT_FAST_BASELINE=PX4+AURA+FAST/T1/C1_CURRENT
FAST_BASELINE_FUNCTIONAL=true
FAST_STRUCTURE_SELECTION=NOT_COMPLETED
READY_FOR_ENGINEERING_FAST_CHARACTERIZATION=true
READY_FOR_PHASE_D_RUNTIME=false
```

This does not invalidate or rewrite any existing Phase-D/Q1 evidence. Qualification infrastructure remains available and can be resumed later from the current checkpoint.

## Current functional baseline

The immediate-response path already exists and runs:

```text
PX4
  ↓
AURA disturbance detection
  ↓
FAST/T1/C1 current immediate response
  ↓
PX4/UAV
```

The current research question is no longer whether FAST exists, but **which bounded FAST structure gives the best disturbance response trade-off**.

The current FAST/T1/C1 configuration therefore remains the reference baseline for engineering comparison. It is not yet frozen as the final FAST architecture.

## Takeoff/offboard runtime closure

`PHASE_D_TAKEOFF_OFFBOARD_RUNTIME_SMOKE_V1` passed on the fresh root:

```text
/media/nahhao74/KINGSTON/phase_d_takeoff_offboard_runtime_smoke_v1_20260910_20/DATA0_FWD_POS_055_r1_A
```

Verified runtime facts:

```text
PX4_STARTED=true
DDS_CONNECTED=true
SETPOINT_STREAM_ACTIVE=true
OFFBOARD_CONTROL_MODE_STREAM_ACTIVE=true
ARMED=true
OFFBOARD_ACCEPTED=true
TAKEOFF_ALTITUDE_REACHED=true
STABLE_HOVER_CONFIRMED=true
SCIENTIFIC_DISTURBANCE_APPLIED=false
PX4_FIRMWARE_MODIFIED=false
SCIENTIFIC_SEMANTIC_DELTA=NONE
CONTROL_SEMANTIC_DELTA=NONE
```

Altitude evidence:

```text
initial_z=-1.4764480590820312
target_z=-3.9764480590820312
final_z=-4.007709980010986
```

The blocker from attempt15 was closed by an implementation-preserving Gazebo DART plugin loader repair in `1_AURA/scripts/start_vehicle.sh`. The runtime exposes the installed ABI-qualified DART plugin through a temporary alias under `/tmp`; PX4 firmware and control semantics were unchanged.

Regression evidence for the smoke repair passed, including shell syntax, Python compile, `aura_data_acquisition` build, focused runtime tests, DART loader check, and scoped diff validation.

## Q1 / Phase-D qualification state

The reduced Q1 component contract was implemented and passed offline qualification:

```text
MINIMUM_PREPARED_CONTRACT_ID=PHASE_D_MINIMUM_PREPARED_Q1_SCOPE_V1
Q1_RESULT_SCHEMA=PHASE_D_Q1_COMPONENT_QUALIFICATION_V1
OWNER_WATERMARK_PRODUCTION_BINDING=PASS_OFFLINE
OWNER_CLOSURE_REMAINS_MANDATORY=true
TRACE_BARRIER_PREDECISION_POLICY=AUXILIARY
E8_INACTIVE_Q1_POLICY=NOT_APPLICABLE
FULL_B0_V3_CONTRACT_UNCHANGED=true
FINAL_PERSISTENCE_RECONCILIATION=PASS_OFFLINE
FULL_REGRESSION=1489 passed, 1 skipped
```

Attempt15 itself remains an immutable fail-closed root because it timed out before the active opportunity. It did not reach V2/C/Q1 decision. That failure was runtime/environmental rather than scientific.

After the successful takeoff/offboard smoke, the system is technically ready for a fresh Q1 component qualification if the owner chooses to resume it. However, the owner has now chosen to **pause qualification work** while FAST engineering characterization proceeds.

```text
READY_FOR_FRESH_Q1_COMPONENT_QUALIFICATION=true
Q1_COMPONENT_QUALIFICATION_EXECUTION=PAUSED_BY_OWNER
READY_FOR_PHASE_D_RUNTIME=false
FULL_B0_V3_READY=false
```

## Qualification branches that are not on the current engineering critical path

The following mechanisms remain implemented and preserved but do not block exploratory engineering response tests:

```text
checkpoint C admission
owner watermark / population closure
pre-decision trace barrier
Q1 prepared seal
E8 prefix qualification
full V3 readiness proof
scientific persistence reconciliation
```

They must still be used when generating formal Phase-D scientific evidence. Engineering runs must not be promoted to scientific evidence retroactively.

## Current objective — FAST characterization and selection

The next engineering stage is:

```text
CURRENT_GOAL=SELECT_BEST_FAST_STRUCTURE
```

Recommended comparison sequence:

```text
1. Current FAST/T1/C1 reference
2. Bounded FAST challenger A
3. Bounded FAST challenger B
4. Additional challenger only when motivated by observed response
5. Select the best trade-off before freezing the baseline for World Model / WISE
```

All candidates should be exercised against identical disturbance profiles and initial conditions.

Primary engineering metrics:

```text
response latency
peak position error
peak velocity error
RMS tracking error
recovery time
overshoot / oscillation
control effort
headroom / saturation
trajectory deviation
```

The objective is not simply minimum position error. A candidate that reduces position error by creating excessive velocity spikes, attitude excursion, saturation, or oscillation is not automatically preferred.

## Engineering visualization

A passive Tkinter monitor is planned as a parallel observer, not part of the control loop.

Minimum display:

```text
position setpoint vs actual: x/y/z
velocity setpoint vs actual: vx/vy/vz
position/velocity error
applied disturbance Fx/Fy/Fz, magnitude and direction
optional estimated wind
FAST/candidate identity
response metrics
```

Architecture:

```text
PX4 / AURA / FAST ───────┬──── scientific logger
                         └──── Tkinter engineering monitor
```

The monitor must remain subscribe-only with respect to control and must not become a dependency of the scientific pipeline.

## Near-term execution ladder

```text
TAKEOFF/OFFBOARD FUNCTIONAL BASELINE       CLOSED
        ↓
TKINTER RESPONSE MONITOR                   NEXT
        ↓
CURRENT FAST REFERENCE RESPONSE            NEXT
        ↓
BOUNDED FAST CHALLENGER COMPARISONS        NEXT
        ↓
SELECT / FREEZE BEST FAST STRUCTURE
        ↓
FORMAL B0 CHARACTERIZATION                 RESUME LATER
        ↓
WORLD MODEL / WISE RANDOMIZED U/ZERO ID
        ↓
AEGIS PREDICTIVE REFINEMENT
```

Scientific target remains unchanged:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + selected FAST baseline
```

The World Model should be trained against a stable selected baseline rather than a FAST structure that is still being changed.

## Hard boundaries retained

```text
PX4 remains authoritative
no PX4 firmware modification from these repairs
FAST remains the immediate response path
World Model must not block first response
large runtime/data artifacts remain under /media/nahhao74/KINGSTON
historical failed roots remain immutable
engineering runs are not formal scientific evidence
READY_FOR_PHASE_D_RUNTIME=false
```

## Source/evidence pointers

Project-side reports corresponding to the latest closure:

```text
reports/phase_d_minimum_prepared_q1_scope_v1/PHASE_D_MINIMUM_PREPARED_Q1_SCOPE_V1_REPORT.md
reports/phase_d_q1_component_live_qualification_v1_attempt15/PHASE_D_Q1_COMPONENT_LIVE_QUALIFICATION_V1_REPORT.md
reports/phase_d_takeoff_offboard_runtime_smoke_v1/PHASE_D_TAKEOFF_OFFBOARD_RUNTIME_SMOKE_V1_REPORT.md
```

Large immutable runtime roots stay on KINGSTON and are intentionally not copied into this documentation repository.
