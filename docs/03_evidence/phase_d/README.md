# Phase-D B0 Evidence Index

This directory is the canonical compact evidence package for **Phase-D B0 characterization**. Large runtime roots and telemetry remain under `/media/nahhao74/KINGSTON`; Git stores compact audit reports, lineage, decisions, hashes, and human-readable bindings.

## Current state

```text
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
READY_FOR_FAST_CHALLENGER_SELECTION=false
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED

INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_REVIEW

CURRENT_BLOCKER=CHECKPOINT_C_CANONICAL_BINDING_AND_PREFIX_COMPLETENESS
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
```

For authoritative current status, read `../../00_overview/CURRENT_STATUS.md` first.

## Directory map

```text
phase_d/
├── README.md
├── campaign/
│   └── CAMPAIGN_STATUS.md
├── closures/
│   └── CLOSURE_SUMMARY.md
└── prewind/
    ├── PHASE_D_PREWIND_HISTORICAL_AUDIT_20260908.md
    ├── PREWIND_PREDICATE_AUDIT_20260908.md
    ├── PREWIND_CHECKPOINT_DECISION_20260908.md          # superseded historical rule
    ├── PREWIND_CHECKPOINT_C_DECISION_20260908.md        # current owner decision
    ├── SOURCE_FRONTIER_BINDING_AUDIT_20260908.md
    ├── SCHEDULED_FRONTIER_AUDIT_20260908.md
    └── evidence/
        ├── HISTORICAL_EVIDENCE_HASHES_20260908.json
        └── REPLAY_INPUT_HASHES_20260908.json
```

Frozen scientific/qualification contract bindings are stored separately under:

```text
../../../05_scientific_contracts/phase_d/
```

## Evidence lineage

### 1. Original Phase-D campaign

The original B0 campaign produced four measured rows before stopping on the original slot-5 C1 trace-retention defect. Those rows remain recorded descriptive evidence but final formal admission remains subject to the prewind qualification/comparability audit.

### 2. C1 trace closure

Original root cause:

```text
CANONICAL_C1_TRACE_RETENTION_GAP_INTERNAL_CALLBACK_VS_PERSISTENCE_HOP_UNPROVEN
```

Prospective closure introduced explicit callback/persistence/drop/error/gap/finalization accounting under `V3_C1_TRACE_WRITER_ACCOUNTING_V1`.

### 3. Slot-5 strict-JSON closure

The later slot-5 acquisition completed runtime evidence but failed strict JSON serialization because fixed-width `ActuatorMotors.control[4..11]` contained expected non-finite PX4 padding.

Qualified representation:

```text
active Sparrow channels 0..3 -> required finite
unused padding 4..11        -> JSON null + mask/count/index
allow_nan=false retained
active-channel nonfinite    -> fail closed
```

Read-only derived requalification preserved F0–F4 and latency exactly.

### 4. V2 slot-6 attestation implementation closure

The first refrozen slot-6 attempt stopped precollector/pre-F0 on `UnboundLocalError: runtime_attestation_emitted`. Minimal closure binding repair qualified offline. The failed root remains immutable and is not salvageable as a scientific row because it never reached F0.

### 5. Historical prewind audit

Historical slots 1–5 lack canonical contemporaneous attestation / `TRACE_MEASUREMENT_READY` evidence. Canonical replay proves their retained pre-F0 evaluation populations are eventually accountable with finalized downstream evidence, but not that all required evidence was available live before historical F0.

Therefore:

```text
HISTORICAL_ORIGINAL_PREWIND_STATUS=UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
HISTORICAL_CONTROL_FAILURE=NOT_INFERRED
```

### 6. Full-prefix `[V2,F0)` boundary audit

Two subsequent source/frontier audits established that the ordinary Phase-D path cannot safely use physical F0 as the live pre-emission population end:

- ordinary commands are immediate native v1 commands driven by host-monotonic phase timing;
- exact physical F0 is only known at native Gazebo application;
- enabling scheduled-native v2 alone would change timing ownership and does not prove ordinary-path comparability;
- a future target still does not prove the final pre-target evaluations are already produced/persisted/reconciled at an earlier decision boundary;
- sequence contiguity cannot detect a missing suffix without an owner upper-watermark.

The earlier `[V2,F0)` owner decision is therefore preserved as superseded lineage, not current authority.

### 7. Current checkpoint-C qualification decision

Current prospective admission population:

```text
PREWIND_POPULATION_V2=[V2_ELIGIBLE,C)
```

`C` must be prospectively frozen, source-owned, control-independent, non-adaptive, and immutable for the bound execution design.

```text
[V2,C) = prewind admission population
[C,F0)  = transition observation
[F0,...) = Phase-D scientific response
```

Physical F0 remains unchanged as native application truth for metrics.

At the existing disturbance opportunity:

```text
sealed PASS over [V2,C) -> disturbance eligible
FAIL / UNKNOWN / incomplete / infra invalid -> no disturbance; retain root; stop
```

No waiting, retry-until-PASS, F0 shift, favorable-state selection, dynamic C selection, dynamic prefix shortening, window restart, or 20-second dwell is allowed.

## Qualification-contract accounting

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

## Read order inside Phase-D

1. `../../00_overview/CURRENT_STATUS.md`
2. `README.md` (this file)
3. `../../../05_scientific_contracts/phase_d/README.md`
4. `../../../05_scientific_contracts/phase_d/PREWIND_QUALIFICATION_V2.md`
5. `prewind/PREWIND_CHECKPOINT_C_DECISION_20260908.md`
6. `prewind/SOURCE_FRONTIER_BINDING_AUDIT_20260908.md`
7. `prewind/SCHEDULED_FRONTIER_AUDIT_20260908.md`
8. `prewind/PREWIND_PREDICATE_AUDIT_20260908.md`
9. `prewind/PHASE_D_PREWIND_HISTORICAL_AUDIT_20260908.md`
10. `closures/CLOSURE_SUMMARY.md`
11. `campaign/CAMPAIGN_STATUS.md`

## Integrity rule

Historical reports/results and decisions remain immutable lineage. New closures or contract revisions do not overwrite historical classifications; they supersede prospective authority explicitly and preserve the previous state for audit.
