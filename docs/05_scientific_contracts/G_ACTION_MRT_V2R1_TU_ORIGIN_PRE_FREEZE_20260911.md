# G-action MRT V2R1 — T_U-Origin Pre-Freeze Status — 2026-09-11

## Status

This document records the owner-approved timing semantic update after fresh bounded-contiguous runtime evidence.

```text
STATUS=PRE_FREEZE_TU_ORIGIN_RUNTIME_REPAIR_REQUIRED
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=false
```

This file is not execution authority for a scientific campaign.

## Scientific objective

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = PX4 + AURA + FAST/T1/C1
```

The target remains closed-loop incremental action effect under an active FAST/T1/C1 baseline.

## Owner-approved timing delta

Owner approved:

```text
APPROVE_G_ACTION_TREATMENT_ONSET_AT_FIRST_SOURCE_BOUND_APPLICATION
```

The old physical-onset interpretation:

```text
physical treatment onset = T_A
hold expiry = T_A + planned duration
G outcome origin = T_A
```

is superseded for future bounded-contiguous G-action design by:

```text
T_U = first source-bound applied candidate frontier
T_A = exact native accepted ACK frontier
physical treatment onset = T_U
hold expiry = T_U + planned duration
G outcome origin proposal = T_U
```

`T_A` itself is not redefined.

```text
T_A_SEMANTICS_MODIFIED=false
T_A_ROLE=TRANSACTION_CONFIRMATION
```

## Why the timing delta was required

Fresh runtime root:

```text
/media/nahhao74/KINGSTON/g_action_contiguous_minimal_runtime_qualification_20260910_234803
```

showed:

```text
first source-bound candidate application=15656000
T_A=15676000
```

The candidate must ingress through E8 before PX4 can produce the accepted ACK. Therefore a treatment contract that requires candidate contribution to remain zero until exact `T_A` cannot represent the existing protocol without changing the protocol itself.

The owner chose to preserve the ActionLink/E8 acceptance mechanism and distinguish physical onset from acknowledgement.

## Canonical timing variables

```text
T_D = causal decision / pre-treatment planning frontier
T_U = physical/source-bound treatment onset
T_A = exact native accepted ACK
T_R_phys = physical candidate termination
T_R_ack = release acceptance/ACK
```

Prospective valid ordering:

```text
T_D < T_U <= T_A
T_R_phys <= T_R_ack
```

The interval `[T_U,T_A)` is realized treatment exposure when non-empty.

## Future G target proposal

For future scientific freeze, the physical action-relative target should be reviewed as:

```text
Y_G(h) = X(first valid same-session/reset native state >= T_U + h) - X(t_base)
```

with a strictly pre-treatment baseline satisfying:

```text
T_D < t_base < T_U
```

No future leakage or interpolation is authorized.

F engineering prediction remains decision/frontier-relative; the T_U delta does not redefine F targets.

## Planned duration semantics

For bounded-contiguous treatment:

```text
planned exposure window = [T_U, T_U + planned_hold_duration_us)
```

A fail-closed termination before planned expiry means:

```text
EXPOSURE_COMPLETED_AS_PLANNED=false
```

Future scientific treatment assignment must be retained; incomplete realized exposure must not be relabeled ZERO, redosed until success, or silently discarded.

## Acceptance after application

Because `T_U` may precede `T_A`, future scientific contracts must distinguish:

```text
assigned treatment
physical application
accepted transaction confirmation
realized exposure/adherence
```

If physical application occurs but the transaction later rejects or times out, the block has physical exposure but no accepted-treatment confirmation. This must be preserved as a nonadherent/failed transaction outcome rather than rewritten.

## Current runtime qualification state

Fresh T_U-origin root:

```text
/media/nahhao74/KINGSTON/g_action_tu_ta_contiguous_requalification_20260911_001245
```

showed:

```text
T_U=16440000
T_A=16440000
PRE_ACK_EXPOSURE_US=0
```

but did not qualify the primitive because an invalid C1 evaluation at `16456000` failed closed while the emitted ledger retained stale pre-gate candidate-active state.

```text
STATUS=INVALID_RUNTIME_IMPLEMENTATION
T_U_SOURCE_QUALIFIED=false
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
```

This is an implementation/observability defect, not a new timing-semantic conflict.

## Required engineering gate before MRT freeze

```text
NEXT_TASK=G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

A fresh source-qualified runtime must prove:

```text
post-gate effective candidate state is canonical
control composition and ledger use the same state
candidate ZERO before T_U
exactly one decomposition row per qualified C1 evaluation
MISSING_EXPOSURE_CYCLES=0
DUPLICATE_EXPOSURE_CYCLES=0
planned T_U-relative exposure realized as intended
release parent binding/ACK valid
FAST active
```

Only after that may:

```text
WORLD_MODEL_G_ACTION_MRT_V2R1_EXECUTABLE_FREEZE
```

become the next task.

## MRT direction — intended, not frozen

Conceptually:

```text
eligible decision point at T_D
    ↓
prospective randomized assignment A_t=(U_t,d_t)
    ↓
physical treatment onset T_U
    ↓
transaction ACK T_A
    ↓
T_U-relative bounded exposure
    ↓
parent-linked release
    ↓
T_U-relative proximal outcome
```

No final randomization law or scientific duration ladder is frozen.

```text
MRT_RANDOMIZATION_LAW=NOT_FINAL_FROZEN
SCIENTIFIC_HOLD_DURATIONS=NOT_FINAL_FROZEN
FORMAL_MRT_G_CAMPAIGN=NOT_EXECUTED
```

The 20 ms hold used in engineering smoke remains `ENGINEERING_ONLY_NOT_SCIENTIFIC`.

## Explicit non-authorizations

This status does not authorize:

```text
scientific MRT execution
new G training
SEALED access
candidate amplitude increase
FAST removal
FAST-off acquisition
WISE control authority
WM control write
PX4 firmware change
```

## Invariants

```text
PX4 authoritative
FAST active baseline
legacy EVENT_ONLY_V1 unchanged
V1/V1.1/V1.2 unchanged
current V2 unchanged
historical runtime roots immutable
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```
