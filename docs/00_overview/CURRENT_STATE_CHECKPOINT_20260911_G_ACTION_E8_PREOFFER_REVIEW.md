# Current State Checkpoint — 2026-09-11 — G-action E8 pre-offer review boundary

## Purpose

This checkpoint supersedes the earlier 2026-09-11 T_U/T_A runtime-repair checkpoint as the latest complete handoff for the active G-action engineering path. Historical checkpoints remain immutable lineage.

## Canonical architecture

```text
Sensors / PX4 / Reference
  ├─> AURA ─> FAST/T1/C1 ───────────────────────────────┐
  │                                                     │
  └─> StateBank ─> World Model / WISE ─> bounded U_plan│
                                                        v
                                      AEGIS candidate path ─> PX4 ─> UAV
```

PX4 remains final authority. FAST remains part of baseline `B`.

```text
B = PX4 + AURA + FAST/T1/C1
```

Scientific target remains:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

Scientific status remains:

```text
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
```

## World Model result retained

The corrected physical-target and timing work through V1, V1.1 and V1.2 remains closed.

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

V1.2 explicit-delay corrected timing semantics but did not recover G-action gain; oracle-delay checks did not establish a general G gain. Current priority remains action-support / treatment-identification quality, not model-capacity escalation.

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

Observed ordering in individual runs is not promoted into a universal timing invariant unless source semantics establish it.

## Freshness / treatment support retained

Protected AURA contract:

```text
ATTITUDE_MAX_AGE_US=10000
```

Retained conditional source-history support from the minimum-realizable-exposure characterization:

| Diagnostic duration | Retained support |
|---:|---:|
| >=4 ms | ~99.96–100% |
| >=8 ms | ~80.2% |
| >=12 ms | ~51.5% |
| >=16 ms | ~13.5% |
| >=20 ms | ~7.0% |
| >=40 ms | ~0.35–0.42% |
| >=80 ms | ~0–0.06% |

These are not scientific completion probabilities, confidence intervals or physical actuator-hold guarantees.

## AURA execution-phase result retained

The experimental V2.1 `MultiThreadedExecutor(2)` path is closed for the current architecture.

Exact hash-guarded ready-time instrumentation and sparse/full perturbation closure established large repeatable V2.1 ready-to-handler tails while wrapper-only behavior remained externally compatible.

```text
INSTRUMENTATION_CAUSES_MATERIAL_V2_TAIL=false
V2_READY_TO_HANDLER_TAIL_INTRINSICALLY_SUPPORTED=true
STATUS=V2_CONCURRENCY_ARCHITECTURE_NOT_SUITABLE_CONFIRMED
AURA_EXECUTION_PHASE_V1=CANONICAL
```

Do not reopen V2/V2.1 without a genuinely new owner-approved architecture.

## Source-rate path retained

The retained PX4 runtime identity is bound to the local compiled uXRCE-DDS attitude export throttle near 10 ms, but upstream native `vehicle_attitude` production rate remains unidentified.

```text
EXPORT_THROTTLE_BOUND_TO_RUNTIME=true
NATIVE_PRODUCER_RATE_IDENTIFIED=false
RATE_PATH_PRIMARY_LIMIT=RATE_PATH_MIXED
SOURCE_RATE_INTERVENTION_CURRENTLY_JUSTIFIED=false
```

No source-rate change is authorized.

## Short-duration G-action identification result

Offline targeted identification selected 8 ms as the primary investigation candidate and 12 ms as the secondary candidate, but retained data did not identify treatment signal, response latency, SNR, minimum detectable effect, or a scientific trial count.

```text
DURATION_8MS_SIGNAL_STATUS=UNIDENTIFIED
DURATION_12MS_SIGNAL_STATUS=UNIDENTIFIED
RESPONSE_LATENCY_IDENTIFIED=false
SNR_PROXY_8MS=UNAVAILABLE
SNR_PROXY_12MS=UNAVAILABLE
SCIENTIFIC_SAMPLE_SIZE_DERIVABLE=false
```

This justified a finite engineering-only ZERO/8/12 response campaign before any source-rate intervention.

## Consumed 12-session engineering response campaign

A frozen 12-session campaign was executed once under V1 + FAST with unchanged 10 ms freshness and `+E 0.012 m/s^2` candidate magnitude.

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

Observed exposure evidence:

```text
ZERO accepted/executed evidence=2; completed=1
8MS accepted=3; completed=2; early source-invalid=1; realized exposure 4–12 ms
12MS accepted=3; completed=0; early source-invalid=3; realized exposure 4,4,8 ms
```

The campaign is immutable and runtime-invalid for response interpretation because four assignments published offers before exact native acceptance and one ZERO release acceptance was not bound by the then-current validator.

```text
STATUS=ENGINEERING_CAMPAIGN_RUNTIME_INVALID
DURATION_8MS_SIGNAL_STATUS=NO_CLEAR_SEPARATION
DURATION_12MS_SIGNAL_STATUS=INSUFFICIENT_COMPLETED_EXPOSURE
8MS_VS_12MS_RESPONSE=UNRESOLVED
RESPONSE_LATENCY_IDENTIFIED=false
```

These signal labels are not causal/statistical conclusions.

## Runtime implementation repair — latest result

Task:

```text
G_ACTION_8_12_RUNTIME_IMPLEMENTATION_REPAIR
```

Result:

```text
TASK_RESULT=RUNTIME_IMPLEMENTATION_REPAIR_PARTIALLY_QUALIFIED
STATUS=BLOCKED_MATERIAL_RUNTIME_SEMANTIC_CHANGE
```

Historical campaign remains immutable:

```text
HISTORICAL_MANIFEST_IMMUTABLE=true
HISTORICAL_ASSIGNMENTS_REPLACED=false
```

### Repair 1 — admission-before-offer

The runner now waits for a current prospective C1/E8 admission witness before arming/submitting its single offer.

```text
ADMISSION_TRIGGER_REPAIR_IMPLEMENTED=true
QUALIFIED_ADMISSION_OBSERVED_BEFORE_OFFER=true
COMMON_ARM_ADMISSION_SEMANTICS_PASS=true
ONE_OFFER_PER_ASSIGNED_SESSION_PASS=true
```

The owner chain is:

```text
ContiguousEngineeringRunner.run_assigned
→ QualifiedActionLinkLifecycle
→ Stage1Probe._c1_callback
```

Current prospective admission owner:

```text
ContiguousEngineeringRunner.wait_for_qualified_admission
```

### Repair 2 — release validator

Historical SESSION_09 source evidence proves that an accepted ZERO/release lifecycle transaction exists. The old validator incorrectly required a duplicated E8 diagnostic acceptance event.

The validator was repaired to bind the exact accepted lifecycle transaction.

```text
SESSION09_ACCEPTED_RELEASE_SOURCE_PROVEN=true
RELEASE_VALIDATOR_REPAIR_IMPLEMENTED=true
RELEASE_EXACT_BINDING_PASS=true
RELEASE_ACK_VALID=true
RELEASE_ACK_EXTENDED_PHYSICAL_DOSE=false
```

Historical runtime evidence was not rewritten.

### Static regression

Focused regression after repair:

```text
84 focused tests PASS
```

### Fresh lifecycle-only qualification

Three fresh implementation-qualification sessions were executed:

```text
ZERO  = PASS
8MS   = PASS
12MS  = RETAINED_NATIVE_ACCEPTANCE_TIMEOUT_AFTER_QUALIFIED_ADMISSION
```

For accepted qualification transactions:

```text
T_A_SOURCE_BINDING_PASS=true
T_U_SOURCE_BINDING_PASS=true
CONTROL_LEDGER_IDENTITY_PASS=true
BRIDGE_EFFECTIVE_STATE_IDENTITY_PASS=true
FAIL_CLOSED_SEMANTICS_PASS=true
```

No `T_U` or `T_A` was fabricated for the rejected 12 ms transaction.

## Current material boundary

The old offer-before-C1 race is closed. The remaining issue is different:

```text
current qualified C1 admission witness
→ one pending assigned offer
→ E8 can still reject that pending offer before native accepted ACK
```

Preventing this rejection would require the runner to wait for a stronger E8 prospective condition than the currently approved C1 admission predicate.

That stronger condition is not yet published as a canonical prospective eligibility contract. Adding it would materially change the opportunity/eligibility population and therefore requires owner review.

Current classification:

```text
PRE_ACCEPTANCE_TIMEOUT_DEFECT_CLOSED=false
FIRST_MATERIAL_BLOCKER=
A qualified C1 admission witness can still reach E8 as a pending offer and be rejected before native ACK.
```

This is not permission to use future treatment completion, future freshness, favorable source phase, retries, or arm-specific eligibility.

## Current owner question

The next decision must establish whether:

1. a source-proven, arm-independent, future-free E8 condition may become part of common prospective pre-offer eligibility; or
2. pre-acceptance E8 rejection should remain a legitimate assigned engineering outcome; or
3. the project should explicitly separate conditional G-response identification from C1→E8 practical admission/support characterization.

Current next task:

```text
NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
```

No runtime campaign should execute before that decision.

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

## Current one-line state

```text
V1 + FAST/T1/C1 remains canonical; F prediction is useful and G remains causally unqualified.
The 12-session ZERO/8/12 campaign is retained as runtime-invalid evidence.
Admission-before-offer and release-validator implementation defects are repaired,
but a C1-qualified offer can still be rejected by E8 before native acceptance.
That remaining issue is now an owner-level prospective-eligibility contract decision.

NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
```
