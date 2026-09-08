# Phase-D B0 Evidence Index

This directory is the canonical compact evidence package for **Phase-D B0 characterization**. Large runtime roots and telemetry stay on `/media/nahhao74/KINGSTON`; Git stores contracts, reports, machine-readable results, audit summaries, and hash ledgers.

## Current state

```text
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
READY_FOR_FAST_CHALLENGER_SELECTION=false

INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_AUDIT

CURRENT_BLOCKER=PHASE_D_PREWIND_EVIDENCE_BOUNDARY_AND_LIVE_PREFIX_COMPLETENESS
```

For authoritative current status, read `../../00_overview/CURRENT_STATUS.md` first.

## Directory map

```text
phase_d/
├── README.md
├── campaign/
│   ├── PHASE_D_B0_CHARACTERIZATION_REPORT.md
│   └── phase_d_b0_campaign_result.json
├── closures/
│   ├── PHASE_D_C1_MUTATION_TRACE_CLOSURE_REPORT.md
│   ├── PHASE_D_NAN_SERIALIZATION_CLOSURE_REPORT.md
│   ├── derived_slot5_postprocess_requalification.json
│   └── PHASE_D_RUNTIME_ATTESTATION_CLOSURE_REPORT.md
├── continuation/
│   ├── PHASE_D_B0_CONTINUATION_FREEZE_REPORT.md
│   ├── phase_d_b0_continuation_manifest.json
│   ├── PHASE_D_B0_CONTINUATION_RUNTIME_REPORT.md
│   ├── phase_d_b0_continuation_runtime_result.json
│   ├── PHASE_D_B0_CONTINUATION_REFREEZE_REPORT.md
│   ├── phase_d_b0_continuation_manifest_v2.json
│   ├── PHASE_D_B0_CONTINUATION_RUNTIME_REPORT_V2.md
│   └── phase_d_b0_continuation_runtime_result_v2.json
├── prewind/
│   ├── PHASE_D_PREWIND_HISTORICAL_AUDIT_20260908.md
│   ├── PREWIND_PREDICATE_AUDIT_20260908.md
│   ├── PREWIND_CHECKPOINT_DECISION_20260908.md
│   └── evidence/
│       ├── HISTORICAL_EVIDENCE_HASHES_20260908.json
│       └── REPLAY_INPUT_HASHES_20260908.json
└── sync/
    └── PHASE_D_SYNC_MANIFEST_20260908.json
```

Frozen scientific contract files are stored separately under:

```text
../../../05_scientific_contracts/phase_d/
```

## Evidence lineage

### 1. Original Phase-D campaign

The original B0 campaign reached four valid measured conditions, then stopped on the original slot-5 C1 trace-retention defect.

The four measured rows retain descriptive latency evidence, but their final formal admission remains subject to the current prewind audit.

### 2. C1 trace closure

Root cause:

```text
CANONICAL_C1_TRACE_RETENTION_GAP_INTERNAL_CALLBACK_VS_PERSISTENCE_HOP_UNPROVEN
```

Prospective closure introduced explicit callback/persistence/drop/error/gap/finalization accounting under:

```text
V3_C1_TRACE_WRITER_ACCOUNTING_V1
```

### 3. Option-B slot-5 postprocessing closure

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

The first refrozen slot-6 attempt stopped precollector/pre-F0 on:

```text
UnboundLocalError: runtime_attestation_emitted
```

Minimal closure binding repair qualified offline. The failed root remains immutable and is not salvageable as a scientific row because it never reached F0.

### 5. Current prewind evidence boundary

Historical slots 1–5 lack the canonical contemporaneous attestation artifact / `TRACE_MEASUREMENT_READY` lifecycle transition.

Canonical replay proves their retained pre-F0 evaluation populations are eventually accountable when finalized downstream evidence is supplied, but does not prove that all required evidence was already complete at live F0 time.

Therefore current historical disposition remains:

```text
UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

This is not a control-failure claim.

## Owner-approved prospective prewind rule

```text
population_start = first canonical evaluation satisfying V3_RUNTIME_START_EVENT_V2
population_end_exclusive = frozen scheduled native F0 source frontier
population = [start, F0)
```

At the one-shot F0 authorization point:

```text
PASS -> F0 eligible
FAIL / UNKNOWN / INFRA_INVALID -> F0 forbidden and row stops
```

No dwell, F0 shift, favorable-state retry, dynamic prefix shortening, or window restart is allowed.

A control-inert prefix-completeness seal may prove closed-prefix evidence persistence while writers remain live. End-of-row finalization remains separate and mandatory.

## Read order inside Phase-D

1. `../../00_overview/CURRENT_STATUS.md`
2. `README.md` (this file)
3. `../../../05_scientific_contracts/phase_d/README.md`
4. `prewind/PREWIND_PREDICATE_AUDIT_20260908.md`
5. `prewind/PREWIND_CHECKPOINT_DECISION_20260908.md`
6. `closures/PHASE_D_RUNTIME_ATTESTATION_CLOSURE_REPORT.md`
7. `closures/PHASE_D_NAN_SERIALIZATION_CLOSURE_REPORT.md`
8. `closures/PHASE_D_C1_MUTATION_TRACE_CLOSURE_REPORT.md`
9. `campaign/PHASE_D_B0_CHARACTERIZATION_REPORT.md`
10. `continuation/` for exact attempt lineage.

## Integrity rule

Historical reports/results are immutable evidence snapshots. New closures, derived artifacts, or owner decisions do not overwrite the historical classification; they add explicit lineage.
