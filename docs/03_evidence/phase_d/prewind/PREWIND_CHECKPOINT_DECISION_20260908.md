# Phase-D Prewind Checkpoint Owner Decision — Superseded

**Date:** 2026-09-08  
**Historical status:** `SUPERSEDED_BY_PREWIND_CHECKPOINT_C_DECISION_20260908`

> This document preserves the earlier owner decision that attempted to use the full live population `[V2_ELIGIBLE, scheduled physical F0)`. Subsequent source/frontier audits proved that the ordinary Phase-D path cannot know the exact physical-F0 source frontier before command emission and cannot guarantee live completeness of the late prefix suffix before that emission. The current authoritative qualification decision is [`PREWIND_CHECKPOINT_C_DECISION_20260908.md`](PREWIND_CHECKPOINT_C_DECISION_20260908.md).

## Historical rule

```text
PREWIND_POPULATION_START = first canonical evaluation satisfying V3_RUNTIME_START_EVENT_V2
PREWIND_POPULATION_END_EXCLUSIVE = frozen scheduled native-F0 source frontier
PREWIND_POPULATION = [V2 eligibility, scheduled F0)
```

The historical intent was:

```text
PASS -> F0 eligible
FAIL / UNKNOWN / INFRASTRUCTURE_INVALID -> F0 forbidden
```

with no dwell, F0 shift, retry-until-PASS, favorable-state selection, dynamic prefix shortening, or window restart.

## Why it was superseded

Later audits established:

1. the ordinary Phase-D path emits an immediate v1 native command and does not expose an exact prospectively bound `SCHEDULED_F0_SOURCE_US` before command emission;
2. physical F0 is only known when Gazebo applies the disturbance in `PreUpdate`;
3. even a future scheduled target does not prove the final pre-target evaluations are already produced, transported, persisted, flushed, and reconciled before an earlier authorization decision;
4. sequence contiguity alone cannot prove that a trailing owner evaluation is not missing.

Therefore the full `[V2,F0)` live admission population creates a causal completeness problem that cannot be solved by a `seal.json` file alone.

## Preserved invariants

The following intent remains valid and is inherited by Qualification V2:

```text
NO_20S_DWELL=true
NO_FAVORABLE_STATE_SELECTION=true
NO_RETRY_UNTIL_PASS=true
NO_WINDOW_RESTART=true
END_OF_ROW_FINALIZATION_REMAINS_MANDATORY=true
```

Historical rows and prior accounting remain immutable.

## Current replacement

See:

- [`PREWIND_CHECKPOINT_C_DECISION_20260908.md`](PREWIND_CHECKPOINT_C_DECISION_20260908.md)
- [`SOURCE_FRONTIER_BINDING_AUDIT_20260908.md`](SOURCE_FRONTIER_BINDING_AUDIT_20260908.md)
- [`SCHEDULED_FRONTIER_AUDIT_20260908.md`](SCHEDULED_FRONTIER_AUDIT_20260908.md)
- [`../../../05_scientific_contracts/phase_d/PREWIND_QUALIFICATION_V2.md`](../../../05_scientific_contracts/phase_d/PREWIND_QUALIFICATION_V2.md)
