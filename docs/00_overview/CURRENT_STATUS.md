# Current Status — 2026-09-11

## Executive state

The active frontier is no longer the original T_U/T_A implementation repair. That work progressed through treatment-onset correction, exposure-ledger repair, executor/source-delivery characterization, finite ZERO/8/12 engineering response execution, and a fresh runtime implementation repair.

Current canonical state:

```text
CURRENT_MODE=G_ACTION_E8_PREOFFER_ADMISSION_OWNER_REVIEW
ENGINEERING_BUILD_MODE=ACTIVE
FORMAL_PHASE_D_QUALIFICATION=PAUSED_NOT_DELETED
CURRENT_BASELINE_B=PX4+AURA+FAST/T1/C1
FAST_ACTIVE_BASELINE=true
AURA_EXECUTION_PHASE_V1=CANONICAL
ATTITUDE_MAX_AGE_US=10000
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
```

The current blocker is an owner-level prospective-eligibility definition, not a generic implementation bug and not a World Model capacity problem.

## Canonical pipeline boundary

```text
Sensors / PX4 / Reference
  ├─> AURA ─> FAST/T1/C1 ───────────────────────────────┐
  │                                                     │
  └─> StateBank ─> World Model / WISE ─> bounded U_plan│
                                                        v
                                      AEGIS candidate path ─> PX4 ─> UAV
```

PX4 remains final authority. FAST remains active in baseline `B`. World Model / WISE has no production control authority.

## World Model conclusion retained

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

The corrected physical-target and timing work through V1/V1.1/V1.2 is closed. V1.2 explicit-delay corrected timing semantics but did not recover general G-action gain; oracle-delay checks also failed to establish a general gain. The current priority remains causal/action-support quality, not model-capacity escalation.

## Canonical treatment timing

Owner-approved timing remains:

```text
T_D = decision / pre-treatment planning frontier
T_U = first source-bound effective E8 candidate application
T_A = exact native accepted ActionLink ACK frontier
T_R_phys = physical candidate termination frontier
T_R_ack = release acceptance / ACK frontier
```

```text
TREATMENT_ONSET=T_U
T_A_ROLE=TRANSACTION_CONFIRMATION
HOLD_EXPIRY_ORIGIN=T_U_PLUS_ASSIGNED_DURATION
```

Observed ordering in one run must not be promoted into a universal timing invariant unless source semantics establish it.

## Freshness / treatment support

Protected freshness remains:

```text
ATTITUDE_MAX_AGE_US=10000
```

Retained conditional source-history support from the minimum-realizable-exposure characterization:

```text
>=4 ms   ~99.96–100%
>=8 ms   ~80.2%
>=12 ms  ~51.5%
>=16 ms  ~13.5%
>=20 ms  ~7.0%
>=40 ms  ~0.35–0.42%
>=80 ms  ~0–0.06%
```

These are not scientific completion probabilities, confidence intervals or physical actuator-hold guarantees.

## AURA executor/source-delivery result retained

The experimental V2.1 `MultiThreadedExecutor(2)` path is closed for the current architecture. Exact hash-guarded ready-time instrumentation plus sparse/full perturbation closure established large repeated V2.1 ready-to-handler tails while wrapper-only behavior remained externally compatible.

```text
INSTRUMENTATION_CAUSES_MATERIAL_V2_TAIL=false
V2_READY_TO_HANDLER_TAIL_INTRINSICALLY_SUPPORTED=true
STATUS=V2_CONCURRENCY_ARCHITECTURE_NOT_SUITABLE_CONFIRMED
AURA_EXECUTION_PHASE_V1=CANONICAL
```

Do not reopen V2/V2.1 without a genuinely new owner-approved architecture.

## Source-rate result retained

The retained PX4 runtime identity is bound to the local compiled uXRCE-DDS attitude export throttle near 10 ms, but upstream native `vehicle_attitude` production rate remains unidentified.

```text
EXPORT_THROTTLE_BOUND_TO_RUNTIME=true
NATIVE_PRODUCER_RATE_IDENTIFIED=false
RATE_PATH_PRIMARY_LIMIT=RATE_PATH_MIXED
SOURCE_RATE_INTERVENTION_CURRENTLY_JUSTIFIED=false
```

No source-rate change is authorized.

## Short-duration G-action identification

Offline targeted identification selected 8 ms as the primary investigation candidate and 12 ms as the secondary candidate. Retained evidence did not identify treatment signal, response latency, SNR, minimum detectable effect, or scientific sample size.

```text
DURATION_8MS_SIGNAL_STATUS=UNIDENTIFIED
DURATION_12MS_SIGNAL_STATUS=UNIDENTIFIED
RESPONSE_LATENCY_IDENTIFIED=false
SNR_PROXY_8MS=UNAVAILABLE
SNR_PROXY_12MS=UNAVAILABLE
SCIENTIFIC_SAMPLE_SIZE_DERIVABLE=false
```

This justified a finite engineering-only ZERO/8/12 response campaign before any source-rate intervention.

## Consumed ZERO / 8 ms / 12 ms engineering campaign

A frozen 12-session engineering response campaign was executed once under V1 + FAST with unchanged freshness and `+E 0.012 m/s^2` candidate magnitude.

Consumed manifest SHA256:

```text
9cf311644423ab1c65bd52977ef014db1eaeb0e184cd7a1c0c0e3efb3cb13486
```

Assignments:

```text
ZERO=4
8MS=4
12MS=4
```

Retained exposure observations:

```text
ZERO accepted/executed evidence=2; completed=1
8MS accepted=3; completed=2; early source-invalid=1; realized exposure 4–12 ms
12MS accepted=3; completed=0; early source-invalid=3; realized exposure 4,4,8 ms
```

The campaign is immutable and runtime-invalid for response interpretation.

```text
STATUS=ENGINEERING_CAMPAIGN_RUNTIME_INVALID
DURATION_8MS_SIGNAL_STATUS=NO_CLEAR_SEPARATION
DURATION_12MS_SIGNAL_STATUS=INSUFFICIENT_COMPLETED_EXPOSURE
8MS_VS_12MS_RESPONSE=UNRESOLVED
RESPONSE_LATENCY_IDENTIFIED=false
```

These signal labels are not causal/statistical conclusions.

## Runtime implementation repair — latest engineering result

Task:

```text
G_ACTION_8_12_RUNTIME_IMPLEMENTATION_REPAIR
```

Result:

```text
TASK_RESULT=RUNTIME_IMPLEMENTATION_REPAIR_PARTIALLY_QUALIFIED
STATUS=BLOCKED_MATERIAL_RUNTIME_SEMANTIC_CHANGE
```

Historical response evidence remains immutable:

```text
HISTORICAL_MANIFEST_IMMUTABLE=true
HISTORICAL_ASSIGNMENTS_REPLACED=false
```

### Admission-before-offer repair

The runner now waits for a current prospective C1 admission witness before arming/submitting its single assigned offer.

```text
ADMISSION_TRIGGER_REPAIR_IMPLEMENTED=true
QUALIFIED_ADMISSION_OBSERVED_BEFORE_OFFER=true
COMMON_ARM_ADMISSION_SEMANTICS_PASS=true
ONE_OFFER_PER_ASSIGNED_SESSION_PASS=true
```

Current owner chain:

```text
ContiguousEngineeringRunner.run_assigned
→ QualifiedActionLinkLifecycle
→ Stage1Probe._c1_callback
```

Prospective admission owner:

```text
ContiguousEngineeringRunner.wait_for_qualified_admission
```

### Release-validator repair

Historical SESSION_09 source evidence proves that an accepted ZERO/release lifecycle transaction exists. The old validator incorrectly required a duplicated E8 diagnostic acceptance event.

The validator now binds the exact accepted lifecycle transaction.

```text
SESSION09_ACCEPTED_RELEASE_SOURCE_PROVEN=true
RELEASE_VALIDATOR_REPAIR_IMPLEMENTED=true
RELEASE_EXACT_BINDING_PASS=true
RELEASE_ACK_VALID=true
RELEASE_ACK_EXTENDED_PHYSICAL_DOSE=false
```

Historical runtime evidence was not rewritten.

### Regression and fresh lifecycle-only qualification

```text
FOCUSED_TESTS_PASS=84
ZERO_QUALIFICATION_RESULT=PASS
8MS_QUALIFICATION_RESULT=PASS
12MS_QUALIFICATION_RESULT=RETAINED_NATIVE_ACCEPTANCE_TIMEOUT_AFTER_QUALIFIED_ADMISSION
```

For accepted qualification transactions:

```text
T_A_SOURCE_BINDING_PASS=true
T_U_SOURCE_BINDING_PASS=true
CONTROL_LEDGER_IDENTITY_PASS=true
BRIDGE_EFFECTIVE_STATE_IDENTITY_PASS=true
FAIL_CLOSED_SEMANTICS_PASS=true
```

No T_U or T_A was fabricated for the rejected 12 ms transaction.

## Current material boundary

The old offer-before-C1 race is closed. The remaining issue is now:

```text
current qualified C1 admission witness
→ single pending assigned offer
→ E8 can still reject that pending offer before native accepted ACK
```

Preventing this rejection would require a stronger E8 prospective condition than the currently approved C1 admission predicate. That condition is not currently exposed as a canonical prospective eligibility contract.

Adding it changes the opportunity/eligibility population and therefore requires owner review.

Current classification:

```text
PRE_ACCEPTANCE_TIMEOUT_DEFECT_CLOSED=false
FIRST_MATERIAL_BLOCKER=
A qualified C1 admission witness can still reach E8 as a pending offer and be rejected before native ACK.
```

## Current owner decision

The next task must decide whether:

1. a source-proven, arm-independent, future-free E8 condition becomes part of common prospective pre-offer eligibility; or
2. pre-acceptance E8 rejection remains a legitimate assigned engineering outcome; or
3. the project explicitly separates conditional G-response identification from C1→E8 practical admission/support characterization.

```text
NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
```

No new response campaign should execute before this decision.

## Hard invariants

```text
PX4 remains authoritative
FAST remains active immediate-response baseline
AURA_EXECUTION_PHASE_V1=CANONICAL
ATTITUDE_MAX_AGE_US=10000
legacy EVENT_ONLY_V1 remains unchanged
historical failed/invalid roots remain immutable
consumed response manifest remains immutable
large artifacts=/media/nahhao74/KINGSTON
V1/V1.1/V1.2 model lineage unchanged
PX4_FIRMWARE_MODIFIED=false
FAST_CONTROL_LAW_MODIFIED=false
T1_C1_MATH_MODIFIED=false
SOURCE_RATE_CHANGED=false
CONTROL_AUTHORITY_CHANGED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

## Latest complete handoff

See:

- `CURRENT_STATE_CHECKPOINT_20260911_G_ACTION_E8_PREOFFER_REVIEW.md` — latest complete checkpoint.
- `CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md` — prior T_U/T_A implementation-repair checkpoint retained as lineage.
- `../05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md` — current pre-freeze scientific timing boundary; still not execution authority.

Earlier checkpoints and failed roots remain historical lineage.