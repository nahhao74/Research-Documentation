# Current State Checkpoint — 2026-09-11 — T_U/T_A Runtime Repair

## Purpose

This checkpoint records the authoritative handoff after the first live bounded-contiguous qualification, the owner-approved `T_U/T_A` timing correction, and the fresh `T_U`-origin requalification that exposed a stale-ledger implementation defect.

Large runtime roots remain under `/media/nahhao74/KINGSTON`.

---

## 1. Current architecture

```text
Sensors / PX4 / Reference
  ├─> AURA ─> FAST/T1/C1 ───────────────────────────────┐
  │                                                     │
  └─> StateBank ─> World Model / WISE ─> bounded U_plan│
                                                        v
                                      AEGIS candidate path ─> PX4 ─> UAV
```

Baseline for current `G_action` work:

```text
B = PX4 + AURA + FAST/T1/C1
```

Scientific estimand remains:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

PX4 remains authoritative. FAST remains active. World Model / WISE has no control authority.

---

## 2. Model state retained

Current World Model conclusion:

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

V1/V1.1/V1.2 remain unchanged by the current action-exposure work.

The acquisition problem, not model capacity, remains the active bottleneck.

---

## 3. Historical event-only semantics

Source audit established:

```text
INTER_CYCLE_PERSISTENCE=PROVEN_EVENT_ONLY
PULSE_3C=3 accepted events
HOLD_SHORT_5C=5 accepted events
HOLD_LONG_7C=7 accepted events
EXPOSURE_DURATION_DERIVABLE=false
```

`OFFER_RETRY_INTERVAL_NS=20_000_000` is transport retry, not candidate cadence or hold duration.

Legacy `EVENT_ONLY_V1` remains unchanged.

---

## 4. Bounded contiguous engineering mode

Owner-approved engineering mode:

```text
BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
```

Shared transaction architecture:

```text
QualifiedOffer
QualifiedAcceptance
QualifiedActionLinkLifecycle
```

Migration closed:

```text
QUALIFIED_TRANSACTION_HOOK_IMPLEMENTED=FULL
STAGE1PROBE_USES_SHARED_HOOK=true
CONTIGUOUS_RUNNER_USES_SHARED_HOOK=true
RELEASE_TRANSACTION_IMPLEMENTED=true
RELEASE_USES_SHARED_HOOK=true
LEGACY_EQUIVALENCE_PASS=true
```

The shared lifecycle owns qualified C1 arming, immutable retry identity, exact terminal-status matching and native `T_A` binding.

---

## 5. First live contiguous smoke

Immutable root:

```text
/media/nahhao74/KINGSTON/g_action_contiguous_minimal_runtime_qualification_20260910_234803
```

Runtime:

```text
RUNTIME_SMOKE_EXECUTED=true
BASELINE_FLIGHT_STABLE=true
FAST_ACTIVE_DURING_SMOKE=true
LANDING_PASS=true
CLEANUP_PASS=true
```

Nonzero plan:

```text
+E 0.012 m/s^2
requested hold=20000 us
OFFER_FRONTIER_US=15652000
first source-bound candidate application=15656000
T_A=15676000
```

The candidate entered the E8/control path before exact accepted `T_A`.

Because accepted status is generated only after candidate ingress, the earlier contract requiring candidate contribution to remain zero before `T_A` was incompatible with the actual ActionLink/E8 protocol.

This was a genuine semantic conflict, not a logger bug.

The same run also showed:

```text
CYCLES_WITH_ACTIVE_CANDIDATE=4
CYCLES_WITH_ZERO_CANDIDATE_DURING_ACTIVE_WINDOW=0
BASELINE_PLUS_CANDIDATE_COMPOSITION_PASS=true
first post-expiry callback candidate=ZERO
release parent binding PASS
release ACK PASS
release ACK did not extend physical dose
```

But:

```text
EXPECTED_QUALIFIED_C1_CYCLES=7
RECORDED_EXPOSURE_CYCLES=6
MISSING_EXPOSURE_CYCLES=1
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
```

The root remains immutable and is not PASS evidence.

---

## 6. Owner-approved `T_U/T_A` timing model

Owner decision:

```text
APPROVE_G_ACTION_TREATMENT_ONSET_AT_FIRST_SOURCE_BOUND_APPLICATION
```

Canonical timing:

```text
T_D = causal decision / planning frontier
T_U = first source-bound application of assigned candidate
T_A = exact native accepted ACK frontier
T_R_phys = physical candidate termination frontier
T_R_ack = release ACK frontier
```

Roles:

```text
TREATMENT_ONSET=T_U
T_A_ROLE=ACCEPTANCE_CONFIRMATION
T_A_SEMANTICS_MODIFIED=false
HOLD_EXPIRY_ORIGIN=T_U_PLUS_PLANNED_HOLD_DURATION_US
G_TARGET_ORIGIN_PROPOSAL=T_U
```

Valid ordering:

```text
T_D < T_U <= T_A
T_R_phys <= T_R_ack
```

`[T_U,T_A)` is part of realized treatment exposure when non-empty.

If candidate applies at `T_U` and transaction later rejects/times out, physical exposure still occurred but accepted-treatment adherence failed; it must not be relabeled ZERO or deleted.

---

## 7. Fresh T_U-origin runtime requalification

Immutable root:

```text
/media/nahhao74/KINGSTON/g_action_tu_ta_contiguous_requalification_20260911_001245
```

Preflight:

```text
TU_STATIC_TESTS_PASS=PASS_65
LEGACY_EQUIVALENCE_REGRESSION=PASS
QUALIFIED_LIFECYCLE_REGRESSION=PASS
ROS_IMPORTS_PASS=PASS
```

Runtime infrastructure:

```text
RUNTIME_SMOKE_EXECUTED=true
BASELINE_FLIGHT_STABLE=true
FAST_ACTIVE_DURING_SMOKE=true
LANDING_PASS=true
CLEANUP_PASS=true
```

Nonzero timing:

```text
REQUESTED_HOLD_US=20000
OFFER_FRONTIER_US=16436000
T_U_US=16440000
T_A_US=16440000
PRE_ACK_EXPOSURE_US=0
FIRST_ACTIVE_SOURCE_US=16440000
LAST_ACTIVE_SOURCE_US=16440000
EXPIRY_TARGET_US=16460000
```

The fresh root did not invalidate the `T_U/T_A` timing model.

Instead, at:

```text
16456000 px4_boot_us
```

an invalid C1 evaluation caused the bounded latch to fail closed, but the emitted decomposition/ledger retained stale pre-evaluation candidate-active state.

Classification:

```text
PER_CYCLE_CANDIDATE_DECOMPOSITION=INVALID_STALE_ACTIVE_FLAG_AFTER_FAIL_CLOSED_C1
EXPOSURE_LEDGER_CONTINUITY=false
T_U_SOURCE_QUALIFIED=false
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
STATUS=INVALID_RUNTIME_IMPLEMENTATION
FIRST_MATERIAL_BLOCKER=
```

This is an implementation/observability defect, not a new timing-semantic conflict.

Release remained healthy:

```text
RELEASE_PARENT_BINDING_PASS=true
RELEASE_ACCEPTANCE_VALID=true
RELEASE_ACK_VALID=true
T_R_ACK_US=16600000
RELEASE_ACK_LATENCY_US=4000
RELEASE_ACK_EXTENDED_PHYSICAL_DOSE=false
```

The root is immutable.

---

## 8. Exact repair now required

Next task:

```text
G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

Required prospective repair:

```text
current C1 source
  -> evaluate validity/freshness/session/reset/expiry gates
  -> fail-close if required
  -> construct exactly one effective post-gate candidate evaluation
  -> compose baseline + effective candidate
  -> publish control/status diagnostics from that state
  -> emit ledger from the same state
```

Invariant:

```text
LEDGER_CANDIDATE == CONTROL_COMPOSITION_CANDIDATE
LEDGER_TOTAL == CONTROL_COMPOSED_TOTAL
```

The stale pre-gate state must never survive into the ledger after a fail-closed transition.

Every qualified C1 evaluation must emit exactly one decomposition record, including valid ZERO/no-offer evaluations.

---

## 9. Early termination semantics

If fail-close occurs before planned `T_U + hold_duration`:

```text
T_R_phys = first source-bound evaluation where candidate becomes ZERO
T_R_phys < planned expiry is allowed physically
EXPOSURE_COMPLETED_AS_PLANNED=false
```

This is incomplete realized exposure, not a successful 20 ms treatment.

Future scientific handling must preserve assigned treatment and mark adherence false; no redosing/relabeling/replacement is authorized.

---

## 10. Fresh requalification success gate

A new root may qualify the primitive only if:

```text
T_U_SOURCE_QUALIFIED=true
PRE_TU_CANDIDATE_ZERO_PASS=true
CONTROL_LEDGER_IDENTITY_PASS=true
ZERO_LEDGER_COMPLETE=true
MISSING_EXPOSURE_CYCLES=0
DUPLICATE_EXPOSURE_CYCLES=0
CYCLES_WITH_ACTIVE_CANDIDATE>0
CYCLES_WITH_ZERO_CANDIDATE_DURING_VALID_ACTIVE_WINDOW=0
EXPOSURE_COMPLETED_AS_PLANNED=true
BASELINE_PLUS_CANDIDATE_COMPOSITION_PASS=true
release parent binding/ACK PASS
release ACK does not extend dose
FAST active
```

Then:

```text
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=true
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=true
NEXT_TASK=WORLD_MODEL_G_ACTION_MRT_V2R1_EXECUTABLE_FREEZE
```

---

## 11. MRT scientific boundary

The intended future MRT timing is now:

```text
assignment at T_D
physical treatment onset T_U
ACK T_A
bounded duration from T_U
parent-linked release
T_U-relative proximal outcome
```

This is not yet executable scientific authority.

```text
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
MRT_RANDOMIZATION_LAW=NOT_FINAL_FROZEN
SCIENTIFIC_HOLD_DURATIONS=NOT_FINAL_FROZEN
```

The 20 ms runtime hold remains engineering-only.

---

## 12. Hard invariants

```text
LEGACY_EVENT_ONLY_BEHAVIOR_UNCHANGED=true
V1_MODEL_MODIFIED=false
V1_1_MODEL_MODIFIED=false
V1_2_MODEL_MODIFIED=false
CURRENT_V2_MODIFIED=false
PX4_FIRMWARE_MODIFIED=false
FAST_CONTROL_LAW_MODIFIED=false
T1_C1_SEMANTICS_MODIFIED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

---

## 13. Immediate handoff

```text
CURRENT_STATUS=INVALID_RUNTIME_IMPLEMENTATION
SEMANTIC_CONFLICT=T_U/T_A RESOLVED_BY_OWNER
NEW_MATERIAL_CONTROL_CONFLICT=NONE
NEXT_TASK=G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

Do not freeze or execute MRT until a fresh source-qualified bounded-contiguous runtime passes.