# Current Execution Ladder — D0 V3 to Phase D — 2026-09-07

## Purpose

This is the single executable ladder for the current mainline. The project is not authorized to run another D0 root or Phase D yet.

## Current state

```text
LATEST_FORMAL_D0_ROOT=_04
D0_RESULT=UNKNOWN_MISSING_EVIDENCE
D0_INFRASTRUCTURE_CLOSED=false
READY_FOR_PHASE_D=false

V3_EXPLAINED_CAUSE_REGISTRY=V3_EXPLAINED_CAUSE_REGISTRY_V1
DERIVED_04_VALID=2879
DERIVED_04_EXPLAINED=1116
DERIVED_04_NOT_READY=5
DERIVED_04_UNKNOWN=1

LATEST_DIAGNOSTIC_PROBE=v3_expected_motor_provenance_probe_20260906_01
PROBE_RESULT=INCONCLUSIVE_INVALID_MEASUREMENT_INFRASTRUCTURE_PRECOLLECTOR
CURRENT_BLOCKER=TRACE_ATTESTATION_PRECOLLECTOR_STARTUP_CONTRACT
```

## Step 1 — Close the DATA0 precollector startup contract

Audit the entire dependency DAG before collector startup.

Required nodes include:

```text
ROOT_CREATED
ROW_LAYOUT_READY
PARENT_LIFECYCLE_READY
TRACE_WRITER_PROCESS_STARTED
TRACE_PATH_AVAILABLE
SOURCE_COUNTER_ATTESTATION_AVAILABLE
TRACE_ATTESTATION_MARKER_AVAILABLE
TRACE_WRITER_MEASUREMENT_READY
RUNTIME_CHILDREN_ELIGIBLE
RUNTIME_CHILDREN_STARTED
COLLECTOR_ELIGIBLE
COLLECTOR_STARTED
```

Rules:

```text
no arbitrary sleep-based repair
no implicit file-exists assumption
missing-before-owner != missing-after-owner
no producer/consumer lifecycle cycles
bootstrap/root immutability boundary must be explicit
```

The historical probe failure must reproduce offline before repair and reach the next legitimate startup state after repair without fabricating evidence.

## Step 2 — Offline qualification only

Before new runtime:

```text
UNMAPPED_PRECOLLECTOR_OBLIGATION_COUNT=0
CYCLIC_DEPENDENCY_COUNT=0
IMPLICIT_FILE_EXISTENCE_ASSUMPTION_COUNT=0
ARBITRARY_SLEEP_DEPENDENCY_COUNT=0
FALSE_STARTUP_PASS_COUNT=0
FALSE_ATTESTATION_PASS_COUNT=0
FALSE_COLLECTOR_READY_COUNT=0
FALSE_ROOT_REUSE_COUNT=0
```

No PX4/Gazebo UAV runtime is needed for this step.

## Step 3 — One expected/motor provenance probe

Only after Step 2 passes, run one new diagnostic probe that is explicitly **not a D0 root**.

Primary question:

```text
CAUSAL_EXPECTED_AVAILABLE=false
→ what exact owner-time expected-setpoint lookup reason causes it?
```

Expected-state owner:

```text
AuraMovingRuntimeNode._expected_callback
→ AuraVNextDisturbanceAssembler.update_expected
→ update_imu->_causal(self._expected,...)
```

Expected object:

```text
PX4_VehicleLocalPositionSetpoint.acceleration
```

Secondary question:

```text
motor_command_stale_or_missing
→ genuine missing motor command?
→ stale timestamp_sample?
→ timestamp_sample=0 semantics?
→ multirate/scheduling gap?
→ lookup/provenance defect?
```

Do not conflate this with runtime-gate `COMMAND_FRESH`; they are distinct command domains.

## Step 4 — Final owner disposition

After successful provenance acquisition:

- freeze exact disposition for `CAUSAL_EXPECTED_AVAILABLE=false`;
- retain `motor_command_stale_or_missing -> NOT_READY_CONTROL_UNAVAILABLE` unless evidence proves a diagnostic predicate defect that is separately repaired/qualified;
- ensure every known reason has exactly one registry disposition;
- unknown/unregistered reason remains fail-closed.

## Step 5 — Final fresh D0 V3 root

Authorize exactly one new immutable formal D0 root only when Steps 1–4 are closed.

PASS requires:

```text
TOTAL_REQUIRED == ACCOUNTED
UNEXPLAINED/UNKNOWN = 0
NOT_READY = 0
READINESS_FAILURE = 0
INVARIANT_VIOLATION = 0
contradictory evidence = 0
owner sequence omissions/duplicates = 0
recorder/writer gaps/drops/errors = 0
W20/C1/E8 correspondence complete
E8 sidecar finalized
collector cleanup PASS
```

D0 may contain registry-approved `EXPLAINED_CONTROL_UNAVAILABLE` states; continuous control validity is not required.

## Step 6 — D0 closure gate

Only a fresh root under the final registry may set:

```text
D0_INFRASTRUCTURE_CLOSED=true
READY_FOR_PHASE_D=true
```

Historical roots `_01.._04` remain immutable and are never relabeled.

## Step 7 — Freeze Phase-D campaign

After D0 closure, freeze metrics and acquisition semantics before disturbance tests.

Separate:

```text
ONSET
SUSTAINED
CLEAR / RECOVERY
```

Measure:

```text
source age / AoI
F0→F4 latency
F5 diagnostic plant response
peak velocity error
peak position error
RMSE
recovery time
overshoot / settling
control effort
TV / jerk
projection / saturation / headroom
validity-gap rate/duration
```

## Step 8 — Identify FAST bottleneck

Do not choose a challenger until evidence determines whether the dominant limitation is:

```text
measurement freshness / scheduling
estimation
filtering/phase
scaling/shaping
plant/headroom
another measured mechanism
```

## Step 9 — Smallest justified FAST challenger

Only then compare the current F0 baseline against the smallest credible algorithmic change under identical simulator conditions.

No current FAST challenger is selected.

## Hard boundaries

```text
no Phase D before D0 closure
no FAST baseline change before measured bottleneck
no WM training from incomplete roots
no SEALED open
no semantic relaxation to obtain PASS
no patch-and-continue inside immutable roots
PX4 remains authoritative
production_authority=false
```
