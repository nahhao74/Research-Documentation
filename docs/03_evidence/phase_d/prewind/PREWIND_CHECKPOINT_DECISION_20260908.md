# Phase-D Prewind Checkpoint Owner Decision

**Date:** 2026-09-08  
**Status:** APPROVED_PROSPECTIVE_RULE_NO_RUNTIME_AUTHORITY

## 1. Fixed evaluation population

```text
PREWIND_POPULATION_START = first canonical evaluation satisfying V3_RUNTIME_START_EVENT_V2
PREWIND_POPULATION_END_EXCLUSIVE = frozen scheduled native-F0 source frontier
PREWIND_POPULATION = [V2 eligibility, scheduled F0)
```

Population membership is determined by canonical source/evaluation identity, not by evidence convenience or control outcome.

Do not dynamically shorten the prefix because downstream evidence for late pre-F0 evaluations is not yet complete.

## 2. One-shot decision boundary

The prewind decision is evaluated exactly once immediately before the native disturbance would be emitted at the already-frozen F0 opportunity.

```text
PASS -> F0 eligible
FAIL -> F0 forbidden
UNKNOWN -> F0 forbidden
INFRASTRUCTURE_INVALID -> F0 forbidden
```

If PASS is unavailable at that opportunity, retain the root and stop the row.

Forbidden:

- moving F0;
- waiting past F0;
- retry-until-PASS;
- favorable-state selection;
- window restart;
- adding a 20-second dwell.

`PHASE_D_PREWIND_MIN_DWELL_20S=NOT_FROZEN_NOT_REQUIRED`

## 3. Live prefix completeness

A control-inert measurement mechanism may be added to prove that the closed prewind prefix is accountable while writers remain active.

For every evaluation in the prefix, the live completeness proof must establish applicable canonical evidence including:

- evaluation identity;
- owner sequence accounting;
- required C1 evidence persistence/reconciliation;
- required E8 evidence persistence/reconciliation;
- source/reset/generation/session identity;
- no known duplicate/omission/contradiction;
- mandatory evidence has crossed its persistence/accounting boundary.

This mechanism may be named `PREWIND_PREFIX_SEAL` or canonical equivalent.

It is measurement/orchestration only and must not change control behavior.

## 4. Prefix seal vs end-of-row finalization

`PREWIND_PREFIX_SEAL` is not equivalent to E8 shutdown/finalization or complete row finalization.

Post-row finalization remains mandatory for:

- trace writer finalization;
- complete C1 callback/persistence accounting;
- drops/errors/gaps;
- E8 shutdown/finalization;
- collector completion;
- lifecycle cleanup;
- post-row correspondence;
- strict JSON;
- final result/report serialization.

Do not weaken finalization semantics to obtain an early PASS.

## 5. Canonical evaluator semantics

Use existing V3 predicates/dispositions for every evaluation in the prefix.

Do not replace them with V2 eligibility, TRACE_MEASUREMENT_READY, source counters, motor readiness, process liveness, or later observed sensor activity alone.

Required successful semantic disposition:

```text
PREWIND_SEMANTIC_DELTA=NONE_EXISTING_V3_PREDICATES_WITH_PREFIX_COMPLETENESS_BOUNDARY
```

If a V3 predicate/disposition meaning must change, stop at owner boundary.

## 6. Historical slots 1–5

Preserve:

```text
HISTORICAL_PREWIND=UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

The descriptive replay proves eventual accountability when finalized downstream evidence is supplied; it does not prove that the same evidence was available contemporaneously before historical F0.

Do not fabricate a historical prefix seal.

Do not change current accounting in this task.

## 7. Prospective implementation authorization

Luna Max may perform offline implementation and tests to:

1. identify the frozen prefix;
2. maintain incremental canonical V3 evaluation/accounting state;
3. establish prefix completeness;
4. persist the prewind decision;
5. place the gate directly on the native-F0 emission path;
6. block F0 for FAIL/UNKNOWN/infrastructure-invalid;
7. preserve post-row finalization.

No FAST/control modification is authorized.

## 8. Exact execution harness requirements

Before any scientific runtime require:

```text
PREWIND_POPULATION_MEMBERSHIP_EXACT=PASS
PREWIND_PREFIX_SEAL=PASS
PREWIND_GATE_TO_F0_PATH=PASS
F0_BEFORE_PREWIND_PASS_COUNT=0
FAIL_ALLOWED_F0_COUNT=0
UNKNOWN_ALLOWED_F0_COUNT=0
INCOMPLETE_PREFIX_ALLOWED_F0_COUNT=0
DYNAMIC_PREFIX_SHORTEN_COUNT=0
FAVORABLE_STATE_RETRY_COUNT=0
WINDOW_RESTART_COUNT=0
F0_SCHEDULE_SHIFT_COUNT=0
```

Also preserve qualification of runtime attestation, trace finalization, C1 accounting, E8 finalization, strict JSON, result serialization, and cleanup.

## 9. Current authorization boundary

Authorized now:

```text
offline implementation
-> tests
-> exact execution harness qualification
-> Astra independent audit
```

Not authorized yet:

```text
scientific Phase-D runtime
new refreeze/execution binding
FAST challenger
```
