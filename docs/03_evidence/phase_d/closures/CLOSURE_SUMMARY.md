# Phase-D Infrastructure Closure Summary

This document records the three major Phase-D infrastructure closures completed so far. None of these closures changes FAST/control/scientific/metric semantics.

## 1. C1 mutation trace retention closure

Historical failure owner:

```text
canonical vnext_data0_trace diagnostic recorder
```

Root cause:

```text
CANONICAL_C1_TRACE_RETENTION_GAP_INTERNAL_CALLBACK_VS_PERSISTENCE_HOP_UNPROVEN
```

Evidence showed E8 had independently received missing C1 identities while the canonical C1 trace had not persisted them. The old recorder lacked callback→persistence accounting.

Prospective repair:

```text
V3_C1_TRACE_WRITER_ACCOUNTING_V1
```

Properties:

```text
bounded FIFO writer
nonblocking callback handoff
callback/persisted/drop/error/gap counters
explicit finalization
replay-critical identity reconciliation
```

Offline qualification passed 293 tests with no false completeness/validity/control-delta counts.

Historical original slot-5 failure remains immutable.

## 2. NaN serialization / slot-5 derived requalification

Later Option-B slot 5 reached canonical runtime completion but failed postprocessing because strict JSON rejected non-finite `ActuatorMotors.control` padding.

Sparrow channel semantics:

```text
active channels = 0..3
unused PX4 fixed-width padding = 4..11
```

Qualified representation:

```text
finite active values -> JSON numbers
unused non-finite padding -> JSON null
preserve channel positions
preserve finite mask/count/non-finite indices
active-channel non-finite -> fail closed
allow_nan=false retained
```

Qualified local postprocessor SHA256:

```text
0abab1a0a595fc38fd6d0b51335b9e4eb597eca26b7afc6cda771946622a20b4
```

Read-only slot-5 reprocessing was scientifically admissible and preserved F0–F4 plus latency exactly.

Derived artifact SHA256:

```text
33c325f0c7170bca990f0f145e833d066436f80d36dd518668189a8ab0b020f9
```

The historical runtime attempt remains `INVALID_INFRASTRUCTURE_POSTPROCESS_SERIALIZATION_NAN`; the derived artifact is separately lineage-bound.

## 3. Runtime-attestation closure binding bug

The first refrozen V2 slot-6 attempt stopped before collector/F0 on:

```text
UnboundLocalError: runtime_attestation_emitted
```

Defect owner:

```text
1_AURA/aura_data_acquisition/vnext_data0_trace.py:record()
```

Minimal repair:

```text
nonlocal sequence, runtime_attestation_emitted
```

Qualified local trace-owner SHA256:

```text
6631af1ac8b9fd34cfe9c86f5b635a37815dd893ae7555bf27084d813acae247
```

Qualification:

```text
RCLPY_IMPORT=PASS
TEST_VNEXT_DATA0_NOMINAL=PASS_21
AFFECTED_D0_PHASE_D_C1_REGRESSIONS=PASS_157
FALSE_ATTESTATION_EMISSION_COUNT=0
FALSE_ATTESTATION_MISSING_COUNT=0
FALSE_MEASUREMENT_READY_COUNT=0
FALSE_CONTROL_DELTA_COUNT=0
```

The historical V2 slot-6 root is immutable and not scientifically salvageable because it never reached collector/F0.

## Semantic boundary

Across all three closures:

```text
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

The current open issue is not one of these repaired defects; it is the prewind evidence-boundary / live-prefix-completeness problem documented under `../prewind/`.
