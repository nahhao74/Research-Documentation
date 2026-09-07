# Current Status — 2026-09-07

## Executive state

The active mainline is now **D0 V3 causal-observability/readiness closure before Phase D FAST bottleneck measurement**.

The previous WM randomized-science track remains historically valid but is not the current executable priority. `G_action` training/scientific admission remains blocked; SEALED remains locked; production authority remains false.

The latest formal D0 root is immutable `_04`:

```text
ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260906_04
RESULT=UNKNOWN_MISSING_EVIDENCE
TOTAL_REQUIRED=4001
ACCOUNTED=4001
VALID_CONTROL=2879
EXPLAINED_CONTROL_UNAVAILABLE=1094
UNEXPLAINED_CONTROL_UNAVAILABLE=28
```

After the prospective explained-cause registry was frozen and `_04` replayed read-only:

```text
DERIVED_04_VALID=2879
DERIVED_04_EXPLAINED=1116
DERIVED_04_NOT_READY=5
DERIVED_04_UNKNOWN=1
```

The remaining semantic blockers are:

```text
1 × CAUSAL_EXPECTED_AVAILABLE=false
    -> UNKNOWN pending fresh expected-setpoint owner-time provenance

5 × motor_command_stale_or_missing
    -> NOT_READY_CONTROL_UNAVAILABLE
    -> must not be explained away
```

The latest diagnostic provenance probe did not reach collection because canonical DATA0 failed precollector startup at:

```text
FIRST_FAILED_PROOF_OBLIGATION=runtime_source_counter_attestation:trace_attestation_marker_missing
ROOT_CAUSE=CANONICAL_DATA0_PRECOLLECTOR_TRACE_ATTESTATION_MARKER_ABSENT
```

This is a measurement/lifecycle infrastructure failure, not a FAST/AURA/W20/C1/E8 control defect.

## Current canonical state

```text
D0_CONTRACT=PHASE_D0_PHASE_D_READINESS_V3
D0_FORMAL_CLOSURE=false
READY_FOR_PHASE_D=false

V3_RUNTIME_BINDING=QUALIFIED
V3_CONCRETE_DATA0_BACKEND=QUALIFIED
V3_STARTUP_TRACE_STATE_MACHINE=QUALIFIED
V3_AURA_OWNER_OPERAND_LEDGER=QUALIFIED
V3_DOWNSTREAM_EVALUATION_CORRESPONDENCE=QUALIFIED
V3_E8_SECONDARY_EVIDENCE_INGESTION=QUALIFIED
REFERENCE_COLLECTOR_CLEANUP=QUALIFIED
V3_EXPLAINED_CAUSE_REGISTRY=V3_EXPLAINED_CAUSE_REGISTRY_V1

KNOWN_REASON_COUNT=24_PLUS_UNKNOWN_FALLBACK
UNMAPPED_KNOWN_REASON_COUNT=0

CURRENT_BLOCKER=DATA0_PRECOLLECTOR_STARTUP_ATTESTATION_CONTRACT
NEXT_RUNTIME_ACTION=NONE_UNTIL_OFFLINE_STARTUP_CONTRACT_CLOSED
```

## What D0 V3 has already proven

### Production runtime and fixed window

Formal roots `_02`, `_03`, and `_04` completed a fixed 20 s V3 observation window with exact evaluation accounting.

Root `_04` proved:

```text
TOTAL_REQUIRED=4001
ACCOUNTED=4001
OWNER_SEQUENCE_DUPLICATES=0
OWNER_SEQUENCE_OMISSIONS=0
RECORDER_MALFORMED=0
RECORDER_DROPS=0
RECORDER_GAPS=0
WRITER_ERRORS=0
TRACE_FINALIZATION=PASS
REFERENCE_COLLECTOR=DEAD_AND_REAPED
```

### Owner-side causal evidence

The canonical owner ledger now records at the actual decision sites:

```text
M3 ordered predicates + actual first-false
runtime-gate operands
attitude owner-time lookup provenance
velocity owner-time lookup provenance
expected-state lookup provenance schema
existing aura-shadow diagnostic identity
```

Frozen M3 order:

```text
SOURCE_IDENTITY_VALID
→ RESET_GENERATION_UNCHANGED
→ CALIBRATION_UNCHANGED
→ CAUSAL_ATTITUDE_AVAILABLE
→ CAUSAL_EXPECTED_AVAILABLE
```

Runtime-gate operands:

```text
ACCELERATION_AVAILABLE
→ ARMED
→ COMMAND_FRESH
```

### Exact downstream correspondence

One diagnostic identity remains control-inert:

```text
aura-shadow:<sequence>
```

It joins:

```text
AURA evaluation
→ W20
→ C1
→ E8
```

while never replacing source frontier, reset identity, native generation, ingress generation or PX4 audit identity.

Root `_04` established:

```text
W20_CORRESPONDENCE=4001/4001
C1_CORRESPONDENCE=4001/4001
E8_EXACT_DIAGNOSTIC_ID_MATCHES=4001
E8_EXACT_C1_ID_MATCHES=4001
E8_UNMATCHED_REQUIRED_EVALUATIONS=0
E8_DUPLICATES=0
E8_MALFORMED=0
E8_FINALIZATION=PASS
```

### Explained-cause registry

`V3_EXPLAINED_CAUSE_REGISTRY_V1` is frozen prospectively.

Examples:

```text
M3 attitude stale
→ EXPLAINED_CONTROL_UNAVAILABLE(ATTITUDE_FRESHNESS)

M3 attitude NO_CANDIDATE
→ EXPLAINED_CONTROL_UNAVAILABLE(CAUSAL_ATTITUDE_UNAVAILABLE)

motor warmup
→ EXPLAINED_CONTROL_UNAVAILABLE(MOTOR_WARMUP)

W20 no causal baseline
→ EXPLAINED_CONTROL_UNAVAILABLE(W20_NOT_READY_NO_CAUSAL_BASELINE)

motor_command_stale_or_missing
→ NOT_READY_CONTROL_UNAVAILABLE

CAUSAL_EXPECTED_AVAILABLE=false
→ UNKNOWN pending fresh owner-time provenance and owner disposition
```

A future D0 PASS now requires all of:

```text
UNKNOWN_MISSING_EVIDENCE=0
NOT_READY_CONTROL_UNAVAILABLE=0
READINESS_FAILURE=0
INVARIANT_VIOLATION=0
```

`EXPLAINED_CONTROL_UNAVAILABLE` is allowed only for registry-approved, exact-evidence fail-closed states.

## Current blocker — DATA0 precollector startup contract

The first expected/motor provenance probe is immutable and inconclusive:

```text
ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/v3_expected_motor_provenance_probe_20260906_01
RESULT=PROBE_INCONCLUSIVE_INVALID_MEASUREMENT_INFRASTRUCTURE_PRECOLLECTOR
```

Collector never started, so the probe produced no evidence about expected-state failures or motor-command failures.

The current task is an **offline global precollector startup/lifecycle audit**, not a single marker patch.

It must close the dependency chain:

```text
bootstrap
→ root/row initialization
→ trace writer process start
→ source-counter attestation
→ trace-attestation readiness
→ trace measurement-ready
→ runtime-child eligibility
→ collector eligibility
→ collector start
→ acquisition
```

Required anti-whack-a-mole outcomes:

```text
UNMAPPED_PRECOLLECTOR_OBLIGATION_COUNT=0
CYCLIC_DEPENDENCY_COUNT=0
IMPLICIT_FILE_EXISTENCE_ASSUMPTION_COUNT=0
ARBITRARY_SLEEP_DEPENDENCY_COUNT=0
```

## Next execution sequence

```text
NOW
1. offline DATA0 precollector startup/attestation contract audit + deterministic repair
2. offline qualification; no real UAV runtime
3. one new expected/motor provenance probe if startup contract passes
4. resolve CAUSAL_EXPECTED_AVAILABLE disposition
5. resolve root cause of motor_command_stale_or_missing while retaining NOT_READY until proven otherwise
6. freeze final registry/evaluator
7. exactly one fresh D0 V3 qualification root
8. if UNKNOWN=0, NOT_READY=0, READINESS_FAILURE=0, INVARIANT_VIOLATION=0 -> D0 CLOSED
9. freeze Phase-D measurement campaign
10. measure actual FAST bottleneck
```

## FAST / Phase D boundary

No replacement FAST algorithm is selected.

Do not optimize FAST before D0 closes. Phase D must first measure:

```text
source age / AoI
F0→F4 latency and F5 diagnostic plant response
ONSET / SUSTAINED / CLEAR behavior
peak position/velocity error
RMSE
recovery
overshoot
control effort
TV / jerk
projection / saturation / headroom
```

Only after the dominant limitation is measured may a smallest-justified challenger be selected.

## World Model / WISE boundary

Frozen scientific target remains:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1 baseline
```

World-Model training remains blocked until a complete valid causal dataset is separately admitted. D0/Phase-D control-readiness work does not authorize WM training or SEALED access.

## Hard invariants

```text
PX4 inner loops remain authoritative
FAST remains immediate disturbance-response path
WM/WISE must not block first response
candidate action remains bounded incremental augmentation
StateBank remains causal/always warm
missing/stale unsupported evidence fails closed
failed roots remain immutable
large runtime artifacts remain on /media/nahhao74/KINGSTON
production_authority=false
SEALED=LOCKED_PRE_EVALUATION
```
