# Current Status — 2026-09-10

## Executive state

The active engineering frontier is now the **World Model `G_action` acquisition path**, specifically qualification of the newly implemented bounded-contiguous candidate exposure primitive under the existing Moving-mode closed loop.

The previous FAST characterization branch remains retained as engineering history. FAST is still active in baseline `B`; no FAST removal or replacement has been qualified.

```text
CURRENT_MODE=WM_G_ACTION_CONTIGUOUS_RUNTIME_QUALIFICATION_PREP
ENGINEERING_BUILD_MODE=ACTIVE
FORMAL_PHASE_D_QUALIFICATION=PAUSED_NOT_DELETED
CURRENT_BASELINE_B=PX4+AURA+FAST/T1/C1
FAST_ACTIVE_BASELINE=true
QUALIFIED_LIFECYCLE_MIGRATION_COMPLETE=true
CONTIGUOUS_MODE=BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=false
SCIENTIFIC_MRT_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
NEXT_TASK=G_ACTION_CONTIGUOUS_MINIMAL_RUNTIME_QUALIFICATION
```

The architecture/refactor blocker is closed. The remaining gate is intentionally small: one fresh engineering SITL smoke containing one bounded ZERO plan and one bounded nonzero 20 ms plan, with complete source-bound exposure/release evidence.

## Pipeline boundary

```text
Sensors / PX4 / Reference
  ├─> AURA ─> FAST/T1/C1 ───────────────────────────────┐
  │                                                     │
  └─> StateBank ─> World Model / WISE ─> bounded U_plan│
                                                        v
                                      AEGIS candidate path ─> PX4 ─> UAV
```

PX4 remains final authority. FAST remains the immediate-response baseline path. World Model / WISE has no production control authority.

## World Model state

The current useful engineering conclusion remains:

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

### V1

`STRUCTURED_BILINEAR_RIDGE_WORLD_MODEL_V1`, five-member logical-session bootstrap ensemble, ridge `lambda=0.001`.

Representative DEV canonical action-relative results:

```text
H40 position MAE 0.002197184 m
H40 velocity MAE 0.001473705 m/s
H80 position MAE 0.003954757 m
H80 velocity MAE 0.002604581 m/s
```

Live SITL shadow inference was successful at low latency, but `G` showed no clear predictive gain and remained causal-unqualified.

### V1.1

`STRUCTURED_BILINEAR_RIDGE_WORLD_MODEL_V1_1` repaired training/runtime context mismatch through frozen cohort OOF Stage-A predictions and explicit missing runtime-delay masks. No model-capacity increase was justified.

### V1.2 explicit delay

`STRUCTURED_BILINEAR_RIDGE_WORLD_MODEL_V1_2_EXPLICIT_DELAY` corrected runtime timing semantics:

```text
prediction state frontier=T_P
T_D remains transaction/tau-estimator frontier
h_effective=max(0,h_query-tau_hat)
G=0 before action frontier
F remains valid when tau unavailable
```

DEV decision-frontier metrics:

```text
H40 position MAE 0.001375025 m
H40 velocity MAE 0.000689563 m/s
H80 position MAE 0.002867297 m
H80 velocity MAE 0.001530301 m/s
```

The timing semantics are materially better aligned, but `G` still has no predictive gain at current effective action support.

## G-action root-cause closure

The offline residual/effective-horizon diagnosis established:

```text
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
SECONDARY_G_LIMITATIONS=
  ACTION_PLAN_TEMPORAL_ALIASING
  INTERRUPTED_OR_UNPROVEN_CONTINUOUS_EXPOSURE
  SMALL_ACTION_SCALE_RELATIVE_TO_RESIDUAL
  CONDITIONAL_SESSION_SUPPORT
  TAU_ERROR_SECONDARY
```

Key support findings:

```text
H40_PRE_ACTION_FRACTION=1.0
DEV nonzero records=72
DEV nonzero actual post-action records=23
DEV clean post-action support >20 ms=none
H120/H160/H200 held-out G gain=NOT_ESTIMABLE
```

At H80, adding the current G term worsened aggregate DEV error relative to F-only. Increasing model capacity was therefore not justified.

## Acquisition V2R1 revision state

The original frozen Acquisition V2 remains parked and immutable:

```text
CURRENT_V2_STATUS=PARKED_FOR_FUTURE_G_ACTION_CAUSAL_QUALIFICATION
CURRENT_V2_IMMUTABLE=true
CURRENT_V2_FIT_TO_CURRENT_G_LIMITATION=PARTIAL
```

A separate `G_ACTION_ACQUISITION_V2R1_REVISION_CANDIDATE` was designed but not executed. It separates:

```text
ASSIGNED_PLAN
OFFERED_COMMAND_SEQUENCE
ACCEPTED_COMMAND_SEQUENCE
ACTUAL_OBSERVED_EXPOSURE
RELEASE_OFFER
RELEASE_ACCEPTANCE
RELEASE_ACK
```

F remains decision-frontier / `T_P` relative. G identification remains actual-acceptance / `T_A` relative.

Scientific duration values and G outcome horizons are not frozen.

## Critical source-semantics discovery

Static source tracing closed the previous ambiguity around historical `3C/5C/7C` profiles:

```text
INTER_CYCLE_PERSISTENCE=PROVEN_EVENT_ONLY
PULSE_3C=3 separately accepted generations
HOLD_SHORT_5C=5 separately accepted generations
HOLD_LONG_7C=7 separately accepted generations
EXPOSURE_DURATION_DERIVABLE=false
```

The old implementation did **not** continuously hold a candidate between accepted generations. Once a matching offer was accepted, the pending offer was retired; subsequent C1 callbacks without a new matching offer recomputed baseline with zero candidate contribution.

`OFFER_RETRY_INTERVAL_NS=20_000_000` is host transport retry behavior, not controller cadence or candidate hold duration.

Therefore the old 3C/5C/7C labels cannot be converted into a physical duration ladder.

## Owner-approved bounded contiguous exposure

Owner approved the engineering design direction:

```text
BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
```

The implementation is a separate explicit mode; legacy `EVENT_ONLY_V1` is preserved.

State-machine semantics:

```text
exact accepted native status
  -> bind T_A in px4_boot_us
  -> activate same bounded candidate vector
  -> reuse on every qualified C1/E8 callback while active
  -> expire at source-clock bound or fail closed
  -> candidate contribution becomes ZERO
  -> explicit parent-linked release completes lifecycle
```

Fail-closed conditions include source invalidity/staleness, session/reset mismatch and bounded expiry. Duplicate generation and transport retry do not restart or extend the dose.

## Release identity

Release semantics now require explicit parent identity rather than inferring lifecycle closure from numerical ZERO:

```text
explicit_zero=true
releases_plan_id=<active plan_id>
new generation
same session/reset lineage
new qualified source frontier
```

Physical candidate expiry is distinct from release ACK. Release ACK must not extend physical dose.

## ROS/runtime environment repair

The previous `diagnostic_msgs` import blocker was closed.

Root cause:

```text
.venv_world_model/bin/python3 remained first in PATH
and ROS PYTHONPATH was replaced after sourcing Jazzy
```

Qualified runtime environment:

```text
PYTHON=/usr/bin/python3
ROS_DISTRO=jazzy
rclpy import=PASS
diagnostic_msgs import=PASS
```

Project paths must be added without replacing ROS `PYTHONPATH`.

## Shared qualified transaction lifecycle

The project now has a complete shared lifecycle abstraction:

```text
QualifiedOffer
QualifiedAcceptance
QualifiedActionLinkLifecycle
```

`QualifiedActionLinkLifecycle` is the canonical owner of:

```text
qualified C1 frontier arming
immutable offer/retry identity
exact terminal-status/ACK matching
native T_A binding
```

Migration status:

```text
QUALIFIED_TRANSACTION_HOOK_IMPLEMENTED=FULL
STAGE1PROBE_USES_SHARED_HOOK=true
CONTIGUOUS_RUNNER_USES_SHARED_HOOK=true
LIVE_RUNNER_ARBITRARY_SOURCE_ALLOWED=false
RELEASE_TRANSACTION_IMPLEMENTED=true
RELEASE_USES_SHARED_HOOK=true
```

`Stage1Probe` now retains higher-level profile/snapshot/record orchestration only; the source-qualified transaction semantics are shared.

## Legacy equivalence closure

A deterministic frozen identity/terminal fixture matrix established full legacy equivalence:

```text
LEGACY_EQUIVALENCE_PASS=true
LEGACY_FRONTIER_SELECTION_EQUIVALENT=true
LEGACY_ACK_MATCH_EQUIVALENT=true
LEGACY_T_A_EQUIVALENT=true
LEGACY_RETRY_EQUIVALENT=true
LEGACY_TIMEOUT_EQUIVALENT=true
LEGACY_GENERATION_PROGRESSION_EQUIVALENT=true
LEGACY_3C_EQUIVALENT=true
LEGACY_5C_EQUIVALENT=true
LEGACY_7C_EQUIVALENT=true
LEGACY_EVENT_ONLY_BEHAVIOR_UNCHANGED=true
```

Latest migration regression:

```text
100 focused tests PASS
ROS imports PASS
py_compile PASS
task-scoped git diff --check PASS
```

No SITL/runtime smoke was executed during lifecycle migration.

## MRT direction

Micro-Randomized Trial methodology is considered the correct future experimental framework for `G_action` once contiguous exposure is qualified live.

The intended future decision-point structure is conceptually:

```text
causal StateBank state / readiness fixed
    -> micro-randomize bounded treatment
       ZERO / +N / -N / +E / -E
    -> exact accepted T_A
    -> bounded contiguous exposure Delta t
    -> parent-linked release
    -> proximal T_A-relative outcome
```

This is currently a design direction only:

```text
FORMAL_MRT_G_CAMPAIGN=NOT_EXECUTED
MRT_RANDOMIZATION_LAW=NOT_FINAL_FROZEN
SCIENTIFIC_HOLD_DURATIONS=NOT_FINAL_FROZEN
```

The existing V2/V2R1 randomized designs supplied useful prospective-assignment/probability-ledger structure, but no completed scientific MRT campaign exists yet.

## Current immediate task

```text
NEXT_TASK=G_ACTION_CONTIGUOUS_MINIMAL_RUNTIME_QUALIFICATION
```

Exactly one fresh engineering session should prove the live primitive with:

```text
1 bounded contiguous ZERO plan
1 bounded contiguous NONZERO plan
planned_hold_duration_us=20000  # engineering-only qualification value
complete source-bound exposure ledger
parent-linked release
FAST active
landing and cleanup
```

Success requires at least:

```text
exact accepted T_A
candidate nonzero on every qualified callback inside active window
no missing exposure proof
candidate ZERO at first qualified post-expiry callback
baseline + candidate composition valid
parent-linked release accepted/ACKed
FAST remains active
```

The 20 ms value is **not** a scientific horizon or proof of identifiability. It is only the minimal engineering semantics-smoke duration.

## Scientific target and boundary

The scientific objective remains:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Current evidence does not authorize:

```text
G_ACTION_CAUSAL_VALID
G_IDENTIFIED
FAST_REMOVAL
WISE_CONTROL_AUTHORITY
WM_CONTROL_WRITE
```

FAST dependence of the current World Model remains intrinsic to its training baseline.

## Global invariants

```text
PX4 remains authoritative
FAST remains active immediate-response baseline
legacy EVENT_ONLY path remains valid and unchanged
bounded contiguous mode is explicit opt-in
large runtime/dataset artifacts=/media/nahhao74/KINGSTON
historical failed roots remain immutable
V1/V1.1/V1.2 not modified by contiguous-lifecycle work
frozen V2 not modified
V2R1 proposal not executed
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

- `CURRENT_STATE_CHECKPOINT_20260910_G_ACTION_CONTIGUOUS_MRT_PREP.md` — detailed handoff covering World Model V1/V1.1/V1.2, G diagnosis, V2R1 redesign, source-semantics closure, bounded contiguous implementation and shared-lifecycle migration.
- `../03_evidence/world_model/G_ACTION_PROGRESS_20260910.md` — compact milestone/evidence trail for the current G-action branch.
- `../05_scientific_contracts/G_ACTION_MRT_V2R1_PRE_FREEZE_STATUS_20260910.md` — current scientific-contract boundary and MRT pre-freeze status.

Older Phase-D and FAST documents remain historical lineage and are not deleted or rewritten as if their evidence disappeared.
