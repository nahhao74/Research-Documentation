# Phase-D B0 Campaign Status

## Scope

This document summarizes the executed Phase-D B0 campaign evidence without redefining the frozen scientific contract. Exact machine artifacts remain in the source repository and KINGSTON runtime roots.

## Original runtime root

```text
/media/nahhao74/KINGSTON/phase_d_b0_runtime_characterization_20260907_01
```

Original campaign result:

```text
PHASE_D_CAMPAIGN_RESULT=INCOMPLETE_INFRASTRUCTURE_STOP
```

Executed original rows: 5. Original rows 1–4 produced complete F0–F4 measurement outputs under the then-active tooling. Original row 5 was measurement-infrastructure invalid due to incomplete C1 mutation retention.

## Descriptive rows 1–4

| Slot | Profile / worker | L_AURA | L_AEGIS | L_ACCEPT | L_ACTUATOR | F0→F4 |
|---:|---|---:|---:|---:|---:|---:|
| 1 | STEADY_E / A | 64 ms | 0 ms | 36 ms | 0 ms | 100 ms |
| 2 | GUST_E / A | 44 ms | 0 ms | 32 ms | 4 ms | 80 ms |
| 3 | GUST_E / B | 8 ms | 0 ms | 24 ms | 0 ms | 32 ms |
| 4 | STEADY_E / B | 100 ms | 0 ms | 60 ms | 8 ms | 168 ms |

These remain descriptive evidence. Their final formal Phase-D admission is currently pending the prewind evidence audit; no row is retroactively relabeled here.

Observed descriptive properties from the original four rows:

```text
recovery = CENSOR_RECOVERY_NOT_ACHIEVED for all four
steady W20 valid coverage ≈ 0.12
gust W20 valid coverage ≈ 0.68
projection invalid = 0
allocator saturation = 0
headroom margin ≈ 0.3
bottleneck = unresolved
```

The small sample does not justify a final worker effect, scenario effect, or bottleneck classification.

## Original slot-5 failure

```text
FIRST_FAILED_PROOF_OBLIGATION=continuous_c1_mutation_trace_complete_for_each_row
CLASSIFICATION=INVALID_MEASUREMENT_INFRASTRUCTURE
```

The historical attempt remains immutable. The closure was prospective and is summarized in `../closures/CLOSURE_SUMMARY.md`.

## Current admission boundary

Historical slots 1–5 currently remain:

```text
UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

This status concerns contemporaneous pre-F0 evidence admission, not a claim that FAST/B0 control failed.

Current accounting is preserved:

```text
INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_AUDIT
```

For the current gate definition and next step, see:

- `../prewind/PREWIND_PREDICATE_AUDIT_20260908.md`
- `../prewind/PREWIND_CHECKPOINT_DECISION_20260908.md`
