# G-action MRT V2R1 — Pre-Freeze Status — 2026-09-10

## Status

This file records the current scientific-contract boundary before any executable MRT freeze.

```text
STATUS=PRE_FREEZE_ENGINEERING_PRIMITIVE_NOT_YET_RUNTIME_QUALIFIED
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

This document is not execution authority.

## Scientific objective

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = PX4 + AURA + FAST/T1/C1
```

The target is closed-loop incremental treatment effect under an active FAST/T1/C1 baseline.

## Why the previous acquisition design is insufficient

Historical 3C/5C/7C treatment labels were source-audited and found to represent counts of separately accepted events rather than proven continuous candidate exposure.

```text
INTER_CYCLE_PERSISTENCE=PROVEN_EVENT_ONLY
EXPOSURE_DURATION_DERIVABLE=false
```

The World Model V1.2 residual diagnosis additionally established that current decision-relative action exposure is too short/sparse for reliable G identification.

```text
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

Therefore the next scientific campaign must not reuse 3C/5C/7C as if they were duration treatments.

## Qualified engineering direction

Owner approved a new project-side action semantic:

```text
BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
```

Intended primitive:

```text
(U, T_A, Delta_t, T_R)
```

where:

```text
T_A = exact accepted native action frontier
Delta_t = predeclared bounded hold in px4_boot_us
T_R = physical termination / release lifecycle boundary, tracked separately
```

Static architecture is implemented, but fresh runtime source qualification has not yet occurred.

## Current lifecycle architecture

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
RELEASE_TRANSACTION_IMPLEMENTED=true
RELEASE_USES_SHARED_HOOK=true
LEGACY_EQUIVALENCE_PASS=true
```

Legacy event-only behavior remains unchanged.

## MRT formulation — intended, not frozen

After live qualification of the bounded exposure primitive, the acquisition should be reviewed as a constrained Micro-Randomized Trial.

Conceptual decision point:

```text
pre-treatment causal state/readiness fixed
    ↓
prospective randomized assignment
    ↓
ZERO / +N / -N / +E / -E / approved temporal profile
    ↓
exact accepted T_A
    ↓
bounded contiguous exposure
    ↓
parent-linked release
    ↓
proximal T_A-relative outcome
```

Treatment assignment probability must be known prospectively for every eligible decision point.

## Required separation of variables

Pre-treatment/deployable plan namespace:

```text
U_PLAN_PRETREATMENT
plan_id
direction
magnitude
planned hold/profile
planned termination/release policy
```

Post-treatment diagnostic namespace:

```text
ACTUAL_EXPOSURE_POSTTREATMENT
actual T_A
actual accepted sequence
actual exposure duration
actual release ACK
future outcome
```

Post-treatment realization fields must not become deployable model inputs.

## Target separation

F engineering prediction remains decision-frontier relative:

```text
origin=T_P
existing H40/H80 engineering targets retained
```

G scientific identification remains action-relative:

```text
origin=actual accepted T_A
strict pre-T_A baseline
future same-session/reset native outcome
```

Do not silently replace one target with the other.

## MRT randomization state

The current V2R1 proposal contains a symbolic constrained allocation law, but it is not final scientific execution authority.

Current position:

```text
historical deterministic data=NOT MRT
old Acquisition V2=MRT-like, parked, never executed
V2R1 proposal=constrained MRT-like proposal
final MRT randomization law=NOT FROZEN
```

A future freeze should explicitly review positivity, depletion/forced assignment, carryover/history and independent session support rather than merely inheriting exact-count miniblock allocation.

## Duration state

No scientific duration ladder is currently approved.

The minimal runtime qualification is authorized to use:

```text
planned_hold_duration_us=20000
```

only as:

```text
ENGINEERING_ONLY_NOT_SCIENTIFIC
```

The value must not be promoted into the MRT contract because it happened to pass an engineering smoke.

Scientific hold durations must be selected only after source qualification proves that arbitrary bounded holds are measurable and after explicit experiment-design review.

## Required next gate

```text
NEXT_TASK=G_ACTION_CONTIGUOUS_MINIMAL_RUNTIME_QUALIFICATION
```

Live qualification must prove:

```text
source-qualified offer
exact accepted T_A
candidate present on every qualified active callback
no missing exposure proof
bounded source-clock expiry
candidate zero after expiry
baseline + candidate composition valid
parent-linked release accepted and closed
FAST remains active
```

Only after this passes may the project proceed to:

```text
WORLD_MODEL_G_ACTION_MRT_V2R1_EXECUTABLE_FREEZE
```

## Explicit non-authorizations

This status does not authorize:

```text
scientific acquisition
MRT runtime execution
new G training
SEALED access
candidate amplitude increase
FAST removal
FAST-off study
WISE control authority
WM control writes
PX4 firmware changes
```

## Invariants

```text
PX4 authoritative
FAST active baseline
V1/V1.1/V1.2 unchanged by current lifecycle work
current frozen V2 unchanged
V2R1 proposal not executed
legacy EVENT_ONLY_V1 unchanged
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```
