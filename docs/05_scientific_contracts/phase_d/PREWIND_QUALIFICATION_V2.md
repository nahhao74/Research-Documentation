# Phase-D Prewind Qualification Contract V2

**Status:** `APPROVED_PROSPECTIVE_NOT_EXECUTED`  
**Scope:** qualification/admission only; no control or metric semantic change.

## Contract revision

The prior prospective prewind population `[V2_ELIGIBLE, F0)` is not live-provable on the ordinary Phase-D command path without either predicting the physical-F0 source frontier or requiring downstream evidence from a suffix that may not yet have persisted before command emission.

Qualification V2 therefore freezes the admission population as:

```text
PREWIND_POPULATION_V2 = [V2_ELIGIBLE, C)
```

where `C` is a fixed source-owned checkpoint bound prospectively before the row.

## Checkpoint-C invariants

`C` must be:

- prospectively frozen;
- source-domain owned;
- independent of control outcome and readiness result;
- non-adaptive;
- immutable for the bound execution design;
- not chosen as the latest fully closed prefix;
- not moved after observing evidence quality.

The concrete canonical binding of `C` is still pending source audit/qualification. Runtime remains unauthorized until that binding and the exact harness pass.

## Interval semantics

```text
[V2_ELIGIBLE, C) = prewind admission population
[C, physical_F0) = transition observation only
[physical_F0, ...) = Phase-D scientific response
```

Physical F0 remains native Gazebo application truth and retains all frozen metric semantics.

## Admission rule

At the existing disturbance opportunity:

```text
canonical V3 predicates over [V2,C)
+ owner upper-watermark through C
+ writer flush/accounting through C
+ exact C1 reconciliation through C
+ exact E8 reconciliation through C
= PASS -> disturbance eligible

FAIL / UNKNOWN / incomplete evidence / infrastructure invalid
= no disturbance; retain root; stop row
```

No wait, retry-until-PASS, F0 shift, favorable-state selection, dynamic prefix shortening, or 20-second dwell is allowed.

## Finalization separation

The prewind prefix seal only proves completeness of the admission prefix. End-of-row finalization remains independently mandatory for writer, C1, E8, collector, lifecycle, postprocessing, strict JSON, and result/report serialization.

## Semantic accounting

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

This document supplements the frozen Phase-D scientific contract. It does not alter B0, the eight scientific conditions, F0–F4 metric definitions, disturbance/reference parameters, or retry policy.

## Current implementation state

```text
CHECKPOINT_C_CANONICAL_BINDING=PENDING
PREWIND_PREFIX_SEAL=NOT_IMPLEMENTED
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
READY_FOR_PHASE_D_RUNTIME=false
```

Related evidence:

- `../../03_evidence/phase_d/prewind/SOURCE_FRONTIER_BINDING_AUDIT_20260908.md`
- `../../03_evidence/phase_d/prewind/SCHEDULED_FRONTIER_AUDIT_20260908.md`
- `../../03_evidence/phase_d/prewind/PREWIND_CHECKPOINT_C_DECISION_20260908.md`
