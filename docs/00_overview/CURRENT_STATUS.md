# Current Status — 2026-09-11

## Executive state

The active engineering frontier is the World Model `G_action` acquisition path, specifically repair and requalification of the bounded-contiguous candidate exposure runtime under the approved `T_U/T_A` timing semantics.

```text
CURRENT_MODE=WM_G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
ENGINEERING_BUILD_MODE=ACTIVE
FORMAL_PHASE_D_QUALIFICATION=PAUSED_NOT_DELETED
CURRENT_BASELINE_B=PX4+AURA+FAST/T1/C1
FAST_ACTIVE_BASELINE=true
QUALIFIED_LIFECYCLE_MIGRATION_COMPLETE=true
CONTIGUOUS_MODE=BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
TREATMENT_ONSET=T_U
T_A_ROLE=EXACT_NATIVE_ACCEPTANCE_ACK
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
NEXT_TASK=G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

The architecture/refactor phase is closed. Two fresh engineering runtime smokes have now been executed. They exposed one material timing-contract conflict, which the owner resolved by introducing `T_U` as physical treatment onset, followed by a separate implementation/observability defect in the fresh `T_U` requalification root.

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

The current priority remains better causal/action-support acquisition, not increasing model capacity.

## Shared qualified lifecycle closure

The canonical transaction stack remains:

```text
QualifiedOffer
QualifiedAcceptance
QualifiedActionLinkLifecycle
```

Current migration state:

```text
QUALIFIED_TRANSACTION_HOOK_IMPLEMENTED=FULL
STAGE1PROBE_USES_SHARED_HOOK=true
CONTIGUOUS_RUNNER_USES_SHARED_HOOK=true
LIVE_RUNNER_ARBITRARY_SOURCE_ALLOWED=false
RELEASE_TRANSACTION_IMPLEMENTED=true
RELEASE_USES_SHARED_HOOK=true
LEGACY_EQUIVALENCE_PASS=true
LEGACY_EVENT_ONLY_BEHAVIOR_UNCHANGED=true
```

The migration previously passed 100 focused tests plus ROS imports, `py_compile`, and task-scoped diff validation.

## First fresh contiguous runtime smoke — material timing conflict

Root:

```text
/media/nahhao74/KINGSTON/g_action_contiguous_minimal_runtime_qualification_20260910_234803
```

The smoke executed one assigned ZERO plan and one `+E 0.012 m/s^2` bounded plan with FAST active, then landed and cleaned up normally.

Key nonzero timing:

```text
OFFER_FRONTIER_US=15652000
first observed candidate application=15656000
accepted T_A=15676000
requested hold=20000 us
```

The candidate was physically/source-bound in E8 before the exact native accepted ACK. Because PX4 must receive an ingress before it can emit the accepted status, the old contract `candidate=ZERO for all t<T_A` was incompatible with the actual ActionLink/E8 protocol.

The same root also showed useful partial evidence:

```text
active candidate records at 15676000, 15680000, 15688000, 15692000
candidate vector correct on recorded active cycles
first post-expiry source 15696000 was ZERO
release parent binding PASS
release ACK PASS at 15860000
release ACK did not extend physical dose
FAST remained active
```

But the root did not qualify the primitive:

```text
EXPECTED_QUALIFIED_C1_CYCLES=7
RECORDED_EXPOSURE_CYCLES=6
MISSING_EXPOSURE_CYCLES=1
STATUS=BLOCKED_MATERIAL_CONTROL_CONFLICT
```

The root remains immutable.

## Owner timing decision — `T_U/T_A`

The owner explicitly approved:

```text
APPROVE_G_ACTION_TREATMENT_ONSET_AT_FIRST_SOURCE_BOUND_APPLICATION
```

Canonical timing is now:

```text
T_D = causal decision / pre-treatment planning frontier
T_U = first source-bound applied candidate frontier
T_A = exact native accepted transaction ACK frontier
T_R_phys = physical candidate termination frontier
T_R_ack = release acceptance/ACK frontier
```

Semantics:

```text
TREATMENT_ONSET=T_U
T_A_SEMANTICS_MODIFIED=false
T_A_ROLE=TRANSACTION_CONFIRMATION
HOLD_EXPIRY_ORIGIN=T_U_PLUS_PLANNED_HOLD_DURATION_US
G_TARGET_ORIGIN_PROPOSAL=T_U
```

Expected valid ordering:

```text
T_D < T_U <= T_A
T_R_phys <= T_R_ack
```

The interval `[T_U,T_A)` is realized treatment exposure when present; it must not be discarded simply because ACK has not yet arrived.

For future scientific identification, the proposed physical action-relative outcome becomes `T_U`-relative, while F remains decision/frontier-relative. This timing delta is owner-approved but scientific V2R1 execution remains unfrozen and unexecuted.

## Fresh `T_U/T_A` requalification

Root:

```text
/media/nahhao74/KINGSTON/g_action_tu_ta_contiguous_requalification_20260911_001245
```

Fresh engineering-only ZERO/nonzero smoke executed successfully at the infrastructure level:

```text
RUNTIME_SMOKE_EXECUTED=true
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
EXPIRY_TARGET_US=16460000
```

This runtime did not reveal a new timing-semantic conflict. Instead, it exposed a control/observability consistency defect:

```text
at 16456000 px4_boot_us:
  C1 evaluation became invalid
  bounded candidate fail-closed correctly
  emitted ledger still reported stale pre-evaluation candidate as active
```

Current classification:

```text
PER_CYCLE_CANDIDATE_DECOMPOSITION=INVALID_STALE_ACTIVE_FLAG_AFTER_FAIL_CLOSED_C1
EXPOSURE_LEDGER_CONTINUITY=false
T_U_SOURCE_QUALIFIED=false
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
STATUS=INVALID_RUNTIME_IMPLEMENTATION
FIRST_MATERIAL_BLOCKER=NONE
```

Release behavior in this root remained correct:

```text
RELEASE_PARENT_BINDING_PASS=true
RELEASE_ACCEPTANCE_VALID=true
RELEASE_ACK_VALID=true
RELEASE_ACK_LATENCY_US=4000
RELEASE_ACK_EXTENDED_PHYSICAL_DOSE=false
```

The root remains immutable and cannot be repaired by post-processing into PASS evidence.

## Current implementation repair target

The next task is:

```text
G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

The required repair is implementation/observability only:

1. Evaluate current-cycle validity/fail-closed gates.
2. Produce one canonical effective candidate state after those gates.
3. Use that same post-gate state for control composition, diagnostic/status output and exposure ledger.
4. Eliminate stale pre-gate `candidate_active` emission.
5. Emit exactly one decomposition/ledger record for every qualified C1 evaluation, including ZERO/no-offer cycles.
6. Re-run a fresh ZERO + nonzero 20 ms engineering smoke in a new KINGSTON root.

No owner review is required unless repair would change control/scientific semantics.

## Current success gate

The bounded primitive remains unqualified until fresh evidence proves at least:

```text
T_U source-qualified from effective post-gate state
candidate ZERO before T_U
control candidate == ledger candidate every cycle
ZERO ledger complete
MISSING_EXPOSURE_CYCLES=0
DUPLICATE_EXPOSURE_CYCLES=0
planned T_U-relative exposure completed as planned
baseline + candidate composition PASS
physical termination source-bound
parent-linked release ACK PASS
release ACK does not extend dose
FAST active
```

If this passes:

```text
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=true
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=true
NEXT_TASK=WORLD_MODEL_G_ACTION_MRT_V2R1_EXECUTABLE_FREEZE
```

The 20 ms hold remains engineering-only and is not a frozen scientific MRT duration.

## MRT / scientific boundary

Future MRT structure is intended to use:

```text
assignment at T_D
physical treatment onset at T_U
transaction confirmation at T_A
bounded duration from T_U
parent-linked release
T_U-relative proximal outcomes
```

But no formal MRT campaign has run.

```text
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
MRT_RANDOMIZATION_LAW=NOT_FINAL_FROZEN
SCIENTIFIC_HOLD_DURATIONS=NOT_FINAL_FROZEN
```

## Hard invariants

```text
PX4 remains authoritative
FAST remains active immediate-response baseline
legacy EVENT_ONLY_V1 remains unchanged
bounded-contiguous mode remains explicit opt-in
historical failed/invalid roots remain immutable
large runtime/dataset artifacts=/media/nahhao74/KINGSTON
V1_MODEL_MODIFIED=false
V1_1_MODEL_MODIFIED=false
V1_2_MODEL_MODIFIED=false
CURRENT_V2_MODIFIED=false
PX4_FIRMWARE_MODIFIED=false
FAST_CONTROL_LAW_MODIFIED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

## Current checkpoint

See:

- `CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md` — latest complete handoff.
- `../03_evidence/world_model/G_ACTION_TU_TA_RUNTIME_20260911.md` — compact runtime evidence update.
- `../05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md` — owner-approved timing delta and current pre-freeze scientific boundary.

Earlier 2026-09-10 checkpoints, Phase-D documents and failed roots remain historical lineage.