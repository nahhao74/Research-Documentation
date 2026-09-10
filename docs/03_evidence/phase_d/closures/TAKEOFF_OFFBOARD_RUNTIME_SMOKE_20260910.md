# Takeoff / Offboard Runtime Smoke Closure — 2026-09-10

## Decision

```text
TASK=PHASE_D_TAKEOFF_OFFBOARD_RUNTIME_SMOKE_V1
RESULT=PASS
FAILURE_CLASS=NONE
READY_FOR_FRESH_Q1_COMPONENT_QUALIFICATION=true
READY_FOR_PHASE_D_RUNTIME=false
```

This was a bounded engineering smoke only. It was not a Phase-D scientific row and applied no scientific disturbance.

## Immutable evidence root

```text
/media/nahhao74/KINGSTON/phase_d_takeoff_offboard_runtime_smoke_v1_20260910_20/DATA0_FWD_POS_055_r1_A
```

Large runtime evidence remains on KINGSTON and is intentionally not copied into Git.

## Runtime closure

```text
PX4_STARTED=true
DDS_CONNECTED=true
SETPOINT_STREAM_ACTIVE=true
OFFBOARD_CONTROL_MODE_STREAM_ACTIVE=true
ARM_COMMAND_RESULT=command=400 result=0
ARMED=true
OFFBOARD_ACCEPTED=true
ACCEPTS_OFFBOARD_SETPOINTS=true
TAKEOFF_ALTITUDE_REACHED=true
STABLE_HOVER_CONFIRMED=true
```

Altitude evidence:

```text
INITIAL_Z=-1.4764480590820312
TARGET_Z=-3.9764480590820312
FINAL_Z=-4.007709980010986
```

The stable-hover check retained the production takeoff/settle criteria; no new acceptance tolerance was invented for the smoke.

## Root cause and repair

Attempt15 had stopped at the takeoff prerequisite. Subsequent runtime diagnosis showed Gazebo could not resolve the requested DART physics plugin name.

The project-side repair in:

```text
1_AURA/scripts/start_vehicle.sh
```

keeps `/usr/share/gz` visible, exposes the installed Gazebo physics engine-plugin directory, and creates a runtime-only temporary alias for the ABI-qualified installed DART library.

The alias lives under `/tmp/aura_data_acquisition/...`; no runtime symlink is placed on the KINGSTON evidence mount.

```text
PX4_FIRMWARE_MODIFIED=false
SCIENTIFIC_SEMANTIC_DELTA=NONE
CONTROL_SEMANTIC_DELTA=NONE
```

A focused test update was made in:

```text
1_AURA/tests/test_vnext_data0_nominal.py
```

## Safety

```text
Q1_MODE=false
SCIENTIFIC_DISTURBANCE_AUTHORITY=false
SCENARIO_DISTURBANCE_EVENTS=0
NATIVE_TRUTH_APPLIED_COUNT=0
TRACE_NATIVE_COUNT=0
DIRECT_ACTUATOR_COUNT=0
SCIENTIFIC_DISTURBANCE_APPLIED=false
```

No E8, Phase-D disturbance, or scientific authority was exercised.

## Lifecycle / writer health

```text
COLLECTOR_EXIT_CODE=0
EXECUTE_REPORT=DIAGNOSTIC_PASS
VALIDATION=DIAGNOSTIC_PASS
CLEANUP_RETURNCODE=0
TRACE_FINALIZED=true
TRACE_SEQUENCE_SUBMITTED=34373
TRACE_DROPS=0
TRACE_ERRORS=0
TRACE_SEQUENCE_GAPS=0
ORPHAN_PROCESSES=false
```

## Regression

Passed:

```text
bash syntax
Python compile
colcon build aura_data_acquisition
vnext DATA0 nominal tests: 26 passed
runtime environment tests: 4 passed
control observability tests: 12 passed
wrench/runtime-independence tests: 4 passed
Gazebo DART one-iteration loader check
scoped diff check
```

## Interpretation

This closes the takeoff/offboard/Gazebo runtime prerequisite. It does **not** close Q1 component qualification, full B0/V3 readiness, or Phase-D runtime authorization.

Owner direction after this closure is to pause qualification work and move the engineering critical path to FAST response characterization and challenger selection. The qualification branch remains resumable from the preserved state.
