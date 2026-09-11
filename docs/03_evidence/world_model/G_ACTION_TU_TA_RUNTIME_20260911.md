# G-action T_U/T_A Runtime Evidence — 2026-09-11

## Scope

Compact evidence update for the current `G_action` acquisition branch after two fresh engineering runtime smokes.

Large runtime data remains under `/media/nahhao74/KINGSTON`.

## Runtime 1 — minimal bounded-contiguous qualification

Root:

```text
/media/nahhao74/KINGSTON/g_action_contiguous_minimal_runtime_qualification_20260910_234803
```

Result:

```text
STATUS=BLOCKED_MATERIAL_CONTROL_CONFLICT
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
```

Key evidence:

```text
OFFER_FRONTIER_US=15652000
first source-bound candidate application=15656000
T_A=15676000
requested hold=20000 us
```

The candidate entered E8 before the exact accepted ACK. This proved the old requirement `candidate=ZERO before T_A` incompatible with the current acceptance protocol.

Other evidence:

```text
FAST_ACTIVE_DURING_SMOKE=true
CYCLES_WITH_ACTIVE_CANDIDATE=4
CYCLES_WITH_ZERO_CANDIDATE_DURING_ACTIVE_WINDOW=0
BASELINE_PLUS_CANDIDATE_COMPOSITION_PASS=true
PHYSICAL_ZERO_BEFORE_OR_AT_FIRST_POST_EXPIRY_CYCLE=true
RELEASE_PARENT_BINDING_PASS=true
RELEASE_ACCEPTANCE_VALID=true
RELEASE_ACK_VALID=true
RELEASE_ACK_EXTENDED_PHYSICAL_DOSE=false
MISSING_EXPOSURE_CYCLES=1
```

The root is immutable and is not promoted.

## Owner decision

Owner approved:

```text
APPROVE_G_ACTION_TREATMENT_ONSET_AT_FIRST_SOURCE_BOUND_APPLICATION
```

New timing semantics:

```text
T_U = first source-bound candidate application
T_A = exact native accepted ACK
TREATMENT_ONSET=T_U
HOLD_EXPIRY_ORIGIN=T_U_PLUS_PLANNED_HOLD_DURATION_US
G_TARGET_ORIGIN_PROPOSAL=T_U
T_A_SEMANTICS_MODIFIED=false
```

## Runtime 2 — T_U-origin requalification

Root:

```text
/media/nahhao74/KINGSTON/g_action_tu_ta_contiguous_requalification_20260911_001245
```

Result:

```text
STATUS=INVALID_RUNTIME_IMPLEMENTATION
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
```

Preflight:

```text
TU_STATIC_TESTS_PASS=PASS_65
LEGACY_EQUIVALENCE_REGRESSION=PASS
QUALIFIED_LIFECYCLE_REGRESSION=PASS
ROS_IMPORTS_PASS=PASS
```

Runtime:

```text
BASELINE_FLIGHT_STABLE=true
FAST_ACTIVE_DURING_SMOKE=true
LANDING_PASS=true
CLEANUP_PASS=true
```

Nonzero timing:

```text
OFFER_FRONTIER_US=16436000
T_U_US=16440000
T_A_US=16440000
PRE_ACK_EXPOSURE_US=0
FIRST_ACTIVE_SOURCE_US=16440000
LAST_ACTIVE_SOURCE_US=16440000
EXPIRY_TARGET_US=16460000
```

Defect at:

```text
16456000 px4_boot_us
```

Observed behavior:

```text
C1 evaluation invalid
bounded candidate fail-closed
ledger still emitted stale pre-gate candidate_active=true
```

Classification:

```text
PER_CYCLE_CANDIDATE_DECOMPOSITION=INVALID_STALE_ACTIVE_FLAG_AFTER_FAIL_CLOSED_C1
EXPOSURE_LEDGER_CONTINUITY=false
T_U_SOURCE_QUALIFIED=false
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
```

This is an implementation/observability defect, not a new timing-semantic conflict.

Release evidence remained valid:

```text
RELEASE_PARENT_BINDING_PASS=true
RELEASE_ACCEPTANCE_VALID=true
RELEASE_ACK_VALID=true
T_R_ACK_US=16600000
RELEASE_ACK_LATENCY_US=4000
RELEASE_ACK_EXTENDED_PHYSICAL_DOSE=false
```

## Current repair target

```text
NEXT_TASK=G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

Prospective invariant:

```text
current-cycle gates
-> effective post-gate candidate state
-> control composition
-> diagnostics/status
-> exposure ledger
```

All outputs for one source frontier must share the same effective state.

Required identity:

```text
LEDGER_CANDIDATE == CONTROL_COMPOSITION_CANDIDATE
LEDGER_TOTAL == CONTROL_COMPOSED_TOTAL
```

Every qualified C1 evaluation must produce exactly one decomposition record, including ZERO/no-offer cycles.

## Scientific status

```text
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
```

No model training or causal claim was performed from either runtime root.