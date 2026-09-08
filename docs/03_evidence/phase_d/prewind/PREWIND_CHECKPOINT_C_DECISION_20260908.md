# Phase-D Prewind Checkpoint-C Owner Decision

**Date:** 2026-09-08  
**Status:** `APPROVED_QUALIFICATION_CONTRACT_REVISION_NO_RUNTIME_AUTHORITY`

## 1. Decision

The previously approved live prewind population `[V2_ELIGIBLE, F0)` is revised prospectively because the ordinary Phase-D path cannot know the exact physical-F0 source frontier before command emission and cannot prove the late pre-F0 suffix persisted/reconciled before that emission.

The approved qualification population is now:

```text
PREWIND_POPULATION_START = first canonical V2-eligible evaluation
PREWIND_POPULATION_END_EXCLUSIVE = fixed source-owned checkpoint C
PREWIND_POPULATION = [V2_ELIGIBLE, C)
```

`C` must be bound prospectively before a scientific row. It must not be selected from control outcome, readiness result, latest-closed-prefix convenience, or favorable-state behavior.

## 2. Checkpoint-C requirements

`C` must be:

```text
PROSPECTIVELY_FROZEN
SOURCE_OWNED
CONTROL_INDEPENDENT
NON_ADAPTIVE
IMMUTABLE_FOR_THE_BOUND_EXECUTION_DESIGN
```

Preference order for binding `C`:

1. an existing canonical source-owned lifecycle frontier;
2. an existing deterministic evaluation-sequence frontier;
3. if neither exists, an explicitly versioned new qualification frontier approved before runtime.

No numeric offset may be chosen merely to make the harness pass.

## 3. Admission and transition intervals

```text
PREWIND_ADMISSION_POPULATION = [V2_ELIGIBLE, C)
TRANSITION_OBSERVATION       = [C, physical_F0)
POST_F0_SCIENCE              = [physical_F0, ...)
```

`[C,F0)` remains recorded and audited. It is not silently discarded and is not retroactively included in the prewind admission decision.

Physical F0 remains the native Gazebo application truth used by Phase-D metrics.

## 4. One-shot disturbance authorization

At the existing disturbance opportunity:

```text
prefix [V2,C) sealed + canonical V3 disposition PASS
    -> emit the existing disturbance command

FAIL / UNKNOWN / incomplete prefix / infrastructure invalid
    -> do not emit disturbance
    -> retain root
    -> stop row
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

## 5. Prefix completeness

A control-inert measurement/orchestration mechanism may close the prefix after `C`.

Before disturbance authorization it must prove for every evaluation `< C`:

- canonical evaluation identity is known;
- an owner upper-watermark proves no trailing evaluation below `C` is missing;
- required C1 evidence has persisted and reconciled;
- required E8 evidence has persisted and reconciled;
- source/reset/generation/session identities are available;
- no known omission, duplicate, contradiction, malformed mandatory record, or unresolved required evidence remains;
- the writer has crossed an actual flush/accounting boundary, not merely incremented an in-memory persistence counter.

The prefix seal is not end-of-row finalization. Full writer/C1/E8/collector/lifecycle/postprocess/result finalization remains independently mandatory.

## 6. Semantic accounting

This revision must not be reported as fully semantic-neutral.

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

The revision changes qualification/admission evidence membership, not B0 control mathematics, scientific disturbance conditions, or F0 metric semantics.

## 7. Historical evidence

Do not rewrite historical slots 1–5.

```text
HISTORICAL_ORIGINAL_PREWIND_STATUS=UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

After `C` is concretely bound, historical data may be replayed separately for:

```text
HISTORICAL_COMPARABILITY_UNDER_PREWIND_V2=PASS | FAIL | UNKNOWN
```

That comparison does not manufacture a historical live prefix seal or overwrite historical classifications.

## 8. Required exact harness

Before scientific runtime, prove at minimum:

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

Adversarial requirement: if the final evaluation `< C` is delayed or missing from downstream persistence, the gate must remain incomplete/UNKNOWN and the disturbance must not be emitted.

## 9. Authorization boundary

Authorized now:

```text
identify and freeze exact canonical C
offline prefix watermark/flush/reconciliation implementation
one-shot disturbance-authorization wiring
exact execution-harness qualification
Astra independent audit
```

Not authorized yet:

```text
scientific Phase-D runtime
new execution binding/refreeze
FAST challenger
```

Current decision:

```text
OWNER_DECISION=APPROVE_FIXED_PREWIND_CHECKPOINT_C_CONTRACT_REVISION
RUNTIME_AUTHORIZED=false
```
