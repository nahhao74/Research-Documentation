# Milestone and Root-Cause Summary

This is the compact canonical audit trail. Large runtime roots and telemetry remain under `/media/nahhao74/KINGSTON`; detailed D0 and Phase-D evidence are indexed under `docs/03_evidence/`.

For current authority use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_PHASE_D_20260908.md
phase_d/README.md
```

## Historical foundation retained

Before the current Phase-D program, the project had already established:

```text
bounded additive AEGIS candidate architecture
PX4 control authority
exact candidate/exposure identity
native-source vs clock-mapping separation
StateBank startup/causal barriers
Option-B Direct Guard
WM reverse-index -> graph -> Tarjan SCC -> fixed-point peeling validity engine
continuous-C1 replay/recovery
post-reset E8 source-causal pairing
native-event CLEAR lifecycle
next_status source-frontier repair
```

The randomized WM1 `G_action` scientific campaign remains separately gated; it is not the current executable priority.

## D0 V3 closure

After multiple infrastructure/observability repairs, fresh canonical D0 V3 readiness closed successfully:

```text
ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260907_02
RESULT=PASS_D0_V3_READINESS
TOTAL_REQUIRED=4000
ACCOUNTED=4000
VALID_CONTROL=2537
EXPLAINED_CONTROL_UNAVAILABLE=1463
NOT_READY=0
READINESS_FAILURE=0
INVARIANT=0
UNKNOWN=0
W20/C1/E8=4000/4000
```

This moved the active mainline from D0 closure into Phase-D B0 characterization.

## Phase-D B0 scientific freeze

Frozen baseline:

```text
B0 = PX4 + AURA + current FAST/T1/C1
```

Frozen contract bindings:

```text
METRIC_CONTRACT_ID=PHASE_D_B0_METRICS_V1_1
METRIC_CONTRACT_SHA256=5928dceea0a6e8e745f94282f7834bc268d0c10ccf61f2ccc4b424d43b78ed93
CAMPAIGN_MANIFEST_SHA256=dfab451d58f45e087fc9b25ab5eb9866bbff8e8d8ac6bd0475dffe25c1a2d247
PLANNED_SCIENTIFIC_CONDITIONS=8
PERFORMANCE_THRESHOLDS=NONE_FROZEN_DESCRIPTIVE_ONLY
RETRY_UNTIL_FAVORABLE=false
FAST_CHALLENGER_SELECTION=NOT_PERFORMED
```

## Phase-D runtime and closure sequence

### 1. Original campaign — four measured conditions

The original campaign produced complete F0–F4 measurements for slots 1–4, then stopped at original slot 5 on a C1 trace-retention defect.

These measurements remain descriptive evidence; formal admission is still subject to prewind qualification/comparability review.

### 2. C1 trace-retention closure

Root cause:

```text
CANONICAL_C1_TRACE_RETENTION_GAP_INTERNAL_CALLBACK_VS_PERSISTENCE_HOP_UNPROVEN
```

Prospective repair introduced explicit callback/persistence/drop/error/gap/finalization accounting under:

```text
V3_C1_TRACE_WRITER_ACCOUNTING_V1
```

No control semantic change was introduced.

### 3. Option-B slot-5 strict-JSON failure

A later slot-5 acquisition retained complete runtime evidence but failed postprocessing because fixed-width `ActuatorMotors.control[4..11]` contained expected non-finite PX4 padding.

Qualified repair:

```text
active Sparrow channels 0..3 -> required finite
unused padding 4..11        -> JSON null + mask/count/index
allow_nan=false retained
active-channel nonfinite    -> fail closed
```

Read-only derived requalification preserved F0–F4 and latency exactly.

### 4. Refreeze V2 slot-6 precollector failure

The next slot-6 attempt stopped before collector/F0 on:

```text
UnboundLocalError: runtime_attestation_emitted
```

Minimal closure binding repair qualified offline with affected D0/Phase-D/C1 regression coverage. The failed historical root remains immutable and is not scientifically salvageable because F0 was never reached.

### 5. Historical prewind evidence audit

Retained slots 1–5 do not contain the canonical contemporaneous attestation marker / `TRACE_MEASUREMENT_READY` lifecycle transition.

Canonical replay with finalized downstream evidence proves the underlying evaluation populations are eventually accountable, but does not prove the complete evidence set existed live before historical F0.

Current historical disposition:

```text
UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
HISTORICAL_CONTROL_FAILURE=NOT_INFERRED
```

### 6. Full-prefix `[V2,F0)` boundary found causally unqualified

Offline source/frontier audits established:

- ordinary Phase-D uses host-monotonic phase timing and immediate native v1 commands;
- exact physical F0 is only known when Gazebo applies the disturbance in `PreUpdate`;
- ordinary command emission therefore has no prospectively known exact physical-F0 source frontier;
- a scheduled future target alone does not prove the final pre-target evaluations are already produced, transported, persisted, flushed, and reconciled at an earlier authorization boundary;
- received sequence contiguity cannot prove no trailing owner evaluation is missing.

The previously approved prospective `[V2,F0)` live population was therefore superseded.

### 7. Prewind Qualification V2 — fixed checkpoint C

Current owner-approved prospective rule:

```text
PREWIND_POPULATION_V2=[V2_ELIGIBLE,C)
```

with:

```text
[V2,C) = prewind admission
[C,F0)  = transition observation
[F0,...) = scientific response
```

`C` must be prospectively frozen, source-owned, control-independent, non-adaptive, and immutable for the bound execution design.

This is explicitly a qualification/admission contract revision:

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

Physical F0 remains native Gazebo application truth.

## Current milestone gate

Current state:

```text
INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_REVIEW
CHECKPOINT_C_CANONICAL_BINDING=PENDING
PREWIND_PREFIX_SEAL=NOT_IMPLEMENTED
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
READY_FOR_PHASE_D_RUNTIME=false
READY_FOR_FAST_CHALLENGER_SELECTION=false
```

Immediate sequence:

```text
identify/freeze exact canonical C
-> owner upper-watermark through C
-> writer flush/accounting through C
-> C1/E8 reconciliation through C
-> one-shot disturbance authorization
-> adversarial exact execution harness
-> Astra independent audit
-> historical comparability / missing-condition decision
-> execution binding
-> Phase-D runtime
```

No scientific runtime is authorized before the exact harness passes.

## Evidence retention rule

Formal roots, failed attempts, historical audits, and superseded owner decisions remain immutable lineage. New prospective qualification decisions do not rewrite historical classifications.
