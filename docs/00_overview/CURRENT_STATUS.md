# Current Status — 2026-09-08

## Executive state

The active mainline is **Phase-D B0 characterization closure**. Formal D0 V3 readiness is already closed; the current blocker is the prospective prewind qualification boundary and exact execution-harness closure.

```text
BASELINE_ID=B0_PX4_AURA_FAST_T1_C1_CURRENT
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
READY_FOR_FAST_CHALLENGER_SELECTION=false
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
```

Current accounting remains immutable during the offline qualification work:

```text
INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_REVIEW
```

## What is already closed

### D0 V3 readiness

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

### Phase-D infrastructure closures

Three independent infrastructure defects have been closed without inferring a control defect:

1. original slot-5 C1 retention gap — prospective C1 callback/persistence accounting qualified;
2. Option-B slot-5 NaN serialization — strict JSON-safe `null + mask/count/index` representation qualified, with read-only derived requalification preserving F0–F4 and latency;
3. V2 slot-6 runtime-attestation closure bug — minimal `nonlocal` closure repair qualified offline.

Historical failed roots remain immutable.

## Frozen Phase-D science

```text
METRIC_CONTRACT_ID=PHASE_D_B0_METRICS_V1_1
METRIC_CONTRACT_SHA256=5928dceea0a6e8e745f94282f7834bc268d0c10ccf61f2ccc4b424d43b78ed93
CAMPAIGN_MANIFEST_SHA256=dfab451d58f45e087fc9b25ab5eb9866bbff8e8d8ac6bd0475dffe25c1a2d247
PLANNED_SCIENTIFIC_CONDITIONS=8
PERFORMANCE_THRESHOLDS=NONE_FROZEN_DESCRIPTIVE_ONLY
RETRY_UNTIL_FAVORABLE=false
ADAPTIVE_CHANGES=false
FAST_CHALLENGER_SELECTION=NOT_PERFORMED
```

No 20-second Phase-D prewind dwell is frozen or required.

## Why the earlier `[V2,F0)` live gate was superseded

Two offline source/frontier audits established a causal boundary problem in the ordinary Phase-D path:

- the ordinary disturbance path uses host-monotonic phase timing and emits an immediate native v1 command;
- the exact physical-F0 source frontier is only known when Gazebo applies the disturbance in `PreUpdate`;
- therefore an exact `SCHEDULED_F0_SOURCE_US` is not available before ordinary command emission;
- even a prospectively scheduled future target does not prove the final pre-target evaluations are already produced, transported, persisted, flushed, and reconciled at an earlier authorization decision;
- sequence contiguity cannot prove that no trailing owner evaluation is missing.

Accordingly the previously approved live population `[V2_ELIGIBLE,F0)` is retained as historical decision lineage but is no longer the current qualification rule.

## Current owner-approved prewind qualification V2

The prospective admission population is now:

```text
PREWIND_POPULATION_V2 = [V2_ELIGIBLE, C)
```

where `C` is a fixed source-owned checkpoint that must be bound prospectively before scientific runtime.

Required properties of `C`:

```text
PROSPECTIVELY_FROZEN
SOURCE_OWNED
CONTROL_INDEPENDENT
NON_ADAPTIVE
IMMUTABLE_FOR_THE_BOUND_EXECUTION_DESIGN
```

Interval semantics:

```text
[V2_ELIGIBLE, C) = prewind admission population
[C, physical_F0) = transition observation only
[physical_F0, ...) = Phase-D scientific response
```

Physical F0 remains native Gazebo application truth and retains the frozen metric definition.

At the existing disturbance opportunity:

```text
PASS over sealed [V2,C) -> disturbance eligible
FAIL / UNKNOWN / incomplete prefix / infrastructure invalid -> no disturbance, retain root, stop row
```

Forbidden:

```text
NO_WAIT=true
NO_RETRY_UNTIL_PASS=true
NO_F0_SHIFT=true
NO_FAVORABLE_STATE_SELECTION=true
NO_DYNAMIC_C_SELECTION=true
NO_DYNAMIC_PREFIX_SHORTENING=true
NO_WINDOW_RESTART=true
NO_20S_DWELL=true
```

## Qualification-contract accounting

This revision is not reported as fully semantic-neutral:

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

The eight-condition B0 experiment, disturbance/reference, control mathematics, F0–F4 metrics, and retry policy remain unchanged.

## Prefix completeness requirement

Before disturbance authorization, the system must prove the entire `[V2,C)` population is closed and accountable while writers remain live.

At minimum:

```text
owner upper-watermark through C
writer-owned flush/accounting through C
exact C1 reconciliation through C
exact E8 reconciliation through C
source/reset/generation/session identity complete
no unresolved mandatory omission/duplicate/contradiction
```

A persistence counter alone is insufficient if it increments before the actual writer flush.

The prefix seal is not end-of-row finalization. Writer/C1/E8/collector/lifecycle/postprocess/result finalization remains independently mandatory after the row.

## Historical slots 1–5

Historical status remains unchanged:

```text
HISTORICAL_ORIGINAL_PREWIND_STATUS=UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
HISTORICAL_CONTROL_FAILURE=NOT_INFERRED
```

After `C` is concretely bound, historical evidence may be replayed only as a separate comparability analysis:

```text
HISTORICAL_COMPARABILITY_UNDER_PREWIND_V2=PASS | FAIL | UNKNOWN
```

It must not fabricate a historical live seal or overwrite historical classifications.

## Immediate next task

Offline only:

```text
1. identify and freeze exact canonical checkpoint C
2. implement owner upper-watermark through C
3. implement writer flush/accounting through C
4. reconcile C1/E8 identities through C
5. wire one-shot disturbance authorization
6. qualify exact production execution harness
7. Astra independently audits source, evidence, timing, lifecycle, and provenance
```

Required harness outcomes include:

```text
C_MEMBERSHIP_FIXED_BEFORE_ROW=PASS
C_CONTROL_INDEPENDENT=PASS
OWNER_UPPER_WATERMARK_THROUGH_C=PASS
WRITER_FLUSH_THROUGH_C=PASS
C1_RECONCILIATION_THROUGH_C=PASS
E8_RECONCILIATION_THROUGH_C=PASS
PREWIND_PREFIX_SEAL=PASS
FAIL_ALLOWED_DISTURBANCE_COUNT=0
UNKNOWN_ALLOWED_DISTURBANCE_COUNT=0
INCOMPLETE_PREFIX_ALLOWED_DISTURBANCE_COUNT=0
DYNAMIC_C_SELECTION_COUNT=0
DYNAMIC_PREFIX_SHORTEN_COUNT=0
FAVORABLE_STATE_RETRY_COUNT=0
DISTURBANCE_SCHEDULE_SHIFT_COUNT=0
```

Adversarial delayed-final-evaluation `< C` must produce incomplete/UNKNOWN and no disturbance.

No scientific Phase-D runtime, new execution binding/refreeze, or FAST challenger is authorized until the exact harness and independent audit pass.

## Authoritative documents

- Current execution ladder: `CURRENT_EXECUTION_LADDER_PHASE_D_20260908.md`
- Phase-D evidence index: `../03_evidence/phase_d/README.md`
- Prewind qualification V2: `../05_scientific_contracts/phase_d/PREWIND_QUALIFICATION_V2.md`
- Current checkpoint-C owner decision: `../03_evidence/phase_d/prewind/PREWIND_CHECKPOINT_C_DECISION_20260908.md`
- Source-frontier audit: `../03_evidence/phase_d/prewind/SOURCE_FRONTIER_BINDING_AUDIT_20260908.md`
- Scheduled-frontier audit: `../03_evidence/phase_d/prewind/SCHEDULED_FRONTIER_AUDIT_20260908.md`
