# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical research documentation for the Moving-Mode UAV Detect & Response pipeline around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model, and WISE predictive refinement.

Large telemetry, runtime roots, datasets, replay bundles, and generated plots remain outside GitHub under `/media/nahhao74/KINGSTON`. This repository stores architecture, scientific contracts, current execution state, compact evidence history, and research roadmaps.

## Read order

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state.
2. [`docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md`](docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md) — latest complete G-action/T_U/T_A handoff.
3. [`docs/03_evidence/world_model/G_ACTION_TU_TA_RUNTIME_20260911.md`](docs/03_evidence/world_model/G_ACTION_TU_TA_RUNTIME_20260911.md) — latest compact runtime evidence.
4. [`docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md`](docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md) — current MRT/G-action pre-freeze timing boundary.
5. [`docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910_G_ACTION_CONTIGUOUS_MRT_PREP.md`](docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910_G_ACTION_CONTIGUOUS_MRT_PREP.md) — prior contiguous/MRT-preparation checkpoint retained as lineage.
6. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority and onboarding rules.
7. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — structural pipeline.
8. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — control path and PX4 authority.
9. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — timing, causality, and StateBank.
10. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — historical audit trail.
11. [`docs/03_evidence/phase_d/README.md`](docs/03_evidence/phase_d/README.md) — retained Phase-D evidence index.
12. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — older frozen randomized-identification contract retained as lineage.
13. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future implementation roadmap.
14. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — source registry.

Superseded states remain available as dated lineage; they are not competing current authority.

## Pipeline

```text
                              PREDICTIVE PATH
Sensors / PX4 / Reference ──────┬────> StateBank (always warm)
                                │              │
                                │              v
                                │       World Model / WISE
                                │              │ bounded U_plan
                                │              v
                                │        AEGIS candidate path
                                │              │
                                v              v
                              AURA ─────> FAST/T1/C1 ─────> PX4 ─────> UAV
```

## Current state — 2026-09-11

The active engineering task is now:

```text
G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
```

Current canonical state:

```text
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
```

Two fresh engineering runtime roots now exist.

First root:

```text
/media/nahhao74/KINGSTON/g_action_contiguous_minimal_runtime_qualification_20260910_234803
```

proved that candidate application can precede exact accepted `T_A`, creating a real conflict with the old assumption that `T_A` was physical treatment onset.

Owner then approved:

```text
T_U = first source-bound candidate application
T_A = exact native accepted ACK
physical treatment onset = T_U
hold expiry origin = T_U + planned duration
G target origin proposal = T_U
```

Fresh requalification root:

```text
/media/nahhao74/KINGSTON/g_action_tu_ta_contiguous_requalification_20260911_001245
```

showed `T_U=T_A=16440000` for the nonzero plan, but did not qualify the primitive because an invalid C1 evaluation failed closed while the emitted ledger retained stale pre-gate candidate-active state.

```text
STATUS=INVALID_RUNTIME_IMPLEMENTATION
FIRST_MATERIAL_BLOCKER=NONE
```

This is an implementation/observability defect, not a new timing-semantic conflict.

## Immediate repair target

The next prospective repair must make one post-gate effective candidate state the single truth for both control composition and ledger output:

```text
C1 gates
  -> effective candidate state
  -> baseline + candidate composition
  -> status/diagnostics
  -> exposure ledger
```

Required runtime invariants include:

```text
LEDGER_CANDIDATE == CONTROL_COMPOSITION_CANDIDATE
exactly one ledger row per qualified C1 evaluation
MISSING_EXPOSURE_CYCLES=0
DUPLICATE_EXPOSURE_CYCLES=0
candidate ZERO before T_U
planned T_U-relative exposure completed as planned
release ACK does not extend physical dose
FAST remains active
```

## World Model and MRT boundary

Current model conclusion remains:

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

Future MRT design is intended to use:

```text
assignment at T_D
physical treatment onset T_U
transaction confirmation T_A
bounded duration from T_U
parent-linked release
T_U-relative proximal outcome
```

But the scientific campaign is not frozen or executed.

## Hard invariants

```text
PX4 remains authoritative
FAST remains active immediate-response baseline
legacy EVENT_ONLY_V1 remains unchanged
bounded-contiguous mode remains explicit opt-in
historical invalid roots remain immutable
large artifacts=/media/nahhao74/KINGSTON
V1/V1.1/V1.2 unchanged
current V2 unchanged
PX4_FIRMWARE_MODIFIED=false
FAST_CONTROL_LAW_MODIFIED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```
