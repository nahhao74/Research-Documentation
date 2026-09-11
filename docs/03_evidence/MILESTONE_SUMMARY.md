# Milestone and Root-Cause Summary

This is the compact canonical audit trail. Large runtime roots and telemetry remain under `/media/nahhao74/KINGSTON`; detailed D0 and Phase-D evidence are indexed under `docs/03_evidence/`.

For current authority use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_STATE_CHECKPOINT_20260911_G_ACTION_E8_PREOFFER_REVIEW.md
../00_overview/DOCUMENT_AUTHORITY.md
```

Older Phase-D ladders and dated checkpoints remain lineage unless current authority explicitly reactivates them.

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

The randomized WM1 `G_action` scientific campaign remains separately gated; it is not currently qualified scientific evidence.

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

Current owner-approved prospective rule at that historical Phase-D checkpoint:

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

This was explicitly a qualification/admission contract revision:

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

## Phase-D current disposition

Phase-D is retained but not the active execution priority:

```text
ENGINEERING_BUILD_MODE=ACTIVE
FORMAL_PHASE_D_QUALIFICATION=PAUSED_NOT_DELETED
```

The older Phase-D immediate sequence must not be mistaken for the current project `NEXT_TASK`.

---

# Current World-Model / G-action engineering line — 2026-09-10 to 2026-09-11

## World Model timing / target closure

Physical-target semantics were corrected and the structured World Model progressed through V1, V1.1 and V1.2.

Retained conclusion:

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

V1.2 explicit-delay corrected decision/application timing semantics but did not recover general G gain. Oracle-delay checks did not justify capacity escalation.

## Event-only action semantics closed

Historical `3C/5C/7C` semantics were proven to represent accepted-event counts, not source-guaranteed contiguous treatment duration.

Result:

```text
CONTINUOUS_EXPOSURE_REQUIRES_EXPLICIT_BOUNDED_CONTROL_SEMANTICS
```

## Bounded treatment timing closure

Owner-approved canonical treatment timing:

```text
T_U = first source-bound effective E8 candidate application
T_A = exact native accepted ACK frontier
TREATMENT_ONSET=T_U
```

`T_A` remains transaction-confirmation semantics. Run-specific observed order/equality is not a universal invariant.

## T_U/T_A implementation closure

Runtime repair closed multiple implementation defects including rejected-status misclassification, stale bounded candidate cache, bounded zero/fail-close suppression and false invalid-source FAST zero semantics.

The repaired path reached a 328-test regression pass during closure.

A 20 ms engineering exposure still failed to complete under unchanged freshness because attitude context became stale after about 4 ms in the key fresh root.

## Minimum realizable exposure characterization

Retained conditional source-history support:

```text
>=4 ms   ~99.96–100%
>=8 ms   ~80.2%
>=12 ms  ~51.5%
>=16 ms  ~13.5%
>=20 ms  ~7.0%
>=40 ms  ~0.35–0.42%
>=80 ms  ~0–0.06%
```

These are engineering support diagnostics, not treatment completion probabilities.

## AURA execution-phase V2.1 closure

Experimental `MultiThreadedExecutor(2)` sharply reduced stale-attitude incidence but introduced repeatable large ready-to-handler dispatcher tails.

Exact hash-guarded instrumentation and sparse/full perturbation closure established:

```text
INSTRUMENTATION_CAUSES_MATERIAL_V2_TAIL=false
V2_READY_TO_HANDLER_TAIL_INTRINSICALLY_SUPPORTED=true
STATUS=V2_CONCURRENCY_ARCHITECTURE_NOT_SUITABLE_CONFIRMED
AURA_EXECUTION_PHASE_V1=CANONICAL
```

V2/V2.1 is closed for the current runtime architecture.

## Source-rate path

Runtime binary provenance binds the retained PX4 uXRCE attitude export path to the local compiled ~10 ms default poll interval, while upstream native producer rate remains unidentified.

```text
EXPORT_THROTTLE_BOUND_TO_RUNTIME=true
NATIVE_PRODUCER_RATE_IDENTIFIED=false
RATE_PATH_PRIMARY_LIMIT=RATE_PATH_MIXED
SOURCE_RATE_INTERVENTION_CURRENTLY_JUSTIFIED=false
```

## Short-duration targeted identification

Retained evidence could not identify 8 ms or 12 ms treatment signal, response latency, SNR, minimum detectable effect, or scientific sample size.

A finite ZERO/8/12 engineering response campaign was therefore justified before any source-rate intervention.

## Consumed 12-session ZERO/8/12 campaign

Frozen manifest SHA256:

```text
9cf311644423ab1c65bd52977ef014db1eaeb0e184cd7a1c0c0e3efb3cb13486
```

Observed exposure evidence:

```text
8MS accepted=3; completed=2; one early source-invalid termination
12MS accepted=3; completed=0; three early source-invalid terminations
```

The campaign is retained as:

```text
STATUS=ENGINEERING_CAMPAIGN_RUNTIME_INVALID
```

because four assigned offers failed before exact native acceptance and one historical release acceptance was not bound by the then-current validator. No assignment was replaced or replayed.

## Runtime implementation repair

Two implementation defects were repaired:

```text
1. runner now observes a current prospective C1 admission witness before its one offer
2. release validator binds the exact accepted lifecycle transaction
```

Regression and fresh lifecycle-only qualification:

```text
FOCUSED_TESTS_PASS=84
ZERO_QUALIFICATION_RESULT=PASS
8MS_QUALIFICATION_RESULT=PASS
12MS_QUALIFICATION_RESULT=RETAINED_NATIVE_ACCEPTANCE_TIMEOUT_AFTER_QUALIFIED_ADMISSION
```

For accepted transactions:

```text
T_A_SOURCE_BINDING_PASS=true
T_U_SOURCE_BINDING_PASS=true
CONTROL_LEDGER_IDENTITY_PASS=true
BRIDGE_EFFECTIVE_STATE_IDENTITY_PASS=true
FAIL_CLOSED_SEMANTICS_PASS=true
```

No T_U/T_A was fabricated for the rejected 12 ms transaction.

## Current milestone gate

The old offer-before-C1 race is closed. The remaining issue is a material opportunity-definition question:

```text
current C1-qualified admission witness
→ one pending assigned offer
→ E8 can still reject before native accepted ACK
```

Preventing the rejection requires a stronger E8 prospective condition than the current C1 admission predicate. Adding that condition changes the eligible opportunity population and therefore requires owner review.

Current authority:

```text
CURRENT_STATUS=BLOCKED_MATERIAL_RUNTIME_SEMANTIC_CHANGE
NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

The owner must choose whether stronger source-proven E8 pre-offer eligibility is allowed, whether pre-acceptance E8 rejection remains an assigned outcome, or whether conditional response and C1→E8 admission/support are handled as separate engineering questions.

## Evidence retention rule

Formal roots, failed attempts, historical audits, consumed manifests, and superseded owner decisions remain immutable lineage. New prospective qualification decisions do not rewrite historical classifications.
