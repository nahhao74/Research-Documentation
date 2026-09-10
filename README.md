# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical research documentation for the Moving-Mode UAV Detect & Response pipeline around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model, and WISE predictive refinement.

Large telemetry, runtime roots, datasets, replay bundles, and generated plots remain outside GitHub under `/media/nahhao74/KINGSTON`. This repository stores architecture, scientific contracts, current execution state, compact evidence history, and research roadmaps.

## Read order

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state.
2. [`docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910_G_ACTION_CONTIGUOUS_MRT_PREP.md`](docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910_G_ACTION_CONTIGUOUS_MRT_PREP.md) — latest complete World Model/G-action handoff.
3. [`docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910.md`](docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910.md) — earlier 2026-09-10 FAST/Phase-D checkpoint retained as lineage.
4. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority and onboarding rules.
5. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — structural end-to-end pipeline.
6. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — control path and PX4 authority.
7. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — timing, causality, and StateBank.
8. [`docs/03_evidence/world_model/G_ACTION_PROGRESS_20260910.md`](docs/03_evidence/world_model/G_ACTION_PROGRESS_20260910.md) — compact current G-action milestone trail.
9. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — historical audit trail.
10. [`docs/03_evidence/phase_d/README.md`](docs/03_evidence/phase_d/README.md) — retained Phase-D evidence index.
11. [`docs/05_scientific_contracts/G_ACTION_MRT_V2R1_PRE_FREEZE_STATUS_20260910.md`](docs/05_scientific_contracts/G_ACTION_MRT_V2R1_PRE_FREEZE_STATUS_20260910.md) — current MRT/G-action scientific boundary before executable freeze.
12. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — older frozen WM1 randomized-identification contract retained as lineage.
13. [`docs/05_scientific_contracts/phase_d/README.md`](docs/05_scientific_contracts/phase_d/README.md) — retained formal Phase-D contracts.
14. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future implementation roadmap.
15. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — source registry.

Superseded current-state and qualification decisions remain available as dated lineage; they are not competing current authority.

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

## Current state — 2026-09-10

The active engineering frontier is now **World Model `G_action` acquisition readiness**, not model-capacity expansion.

The latest diagnosis established that the dominant limitation is insufficient clean effective action horizon. Historical `3C/5C/7C` treatments were then source-audited and proven to be event-count semantics rather than continuous candidate holds.

A new explicit engineering mode has therefore been implemented:

```text
BOUNDED_CONTIGUOUS_CANDIDATE_EXPOSURE_V1
```

Legacy `EVENT_ONLY_V1` remains unchanged.

Current status:

```text
CURRENT_MODE=WM_G_ACTION_CONTIGUOUS_RUNTIME_QUALIFICATION_PREP
CURRENT_BASELINE_B=PX4+AURA+FAST/T1/C1
FAST_ACTIVE_BASELINE=true
QUALIFIED_TRANSACTION_HOOK_IMPLEMENTED=FULL
STAGE1PROBE_USES_SHARED_HOOK=true
CONTIGUOUS_RUNNER_USES_SHARED_HOOK=true
RELEASE_TRANSACTION_IMPLEMENTED=true
LEGACY_EQUIVALENCE_PASS=true
CONTIGUOUS_EXPOSURE_SOURCE_QUALIFIED=false
V2R1_TEMPORAL_LADDER_NOW_DERIVABLE=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
NEXT_TASK=G_ACTION_CONTIGUOUS_MINIMAL_RUNTIME_QUALIFICATION
```

The shared `QualifiedActionLinkLifecycle` now owns qualified C1 arming, transport retry identity, exact accepted-status matching and native `T_A` binding for both legacy Stage1 and the new contiguous runner. The migration passed 100 focused tests, ROS imports, `py_compile`, and task-scoped diff validation.

## World Model conclusion

Current model work supports a useful short-horizon `F` predictor, but not a useful/qualified `G` predictor under the historical action support.

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

The current order is therefore:

```text
qualify bounded contiguous action exposure live
    ↓
freeze MRT-style scientific acquisition
    ↓
collect clean prospectively randomized U/ZERO action-response data
    ↓
evaluate G identifiability and treatment signal
    ↓
only then reconsider model capacity/family
```

## MRT direction

Micro-Randomized Trial methodology is the intended future framework for `G_action` identification after live qualification of the bounded action primitive.

Conceptually:

```text
qualified decision point
    ↓
causal pre-treatment state fixed
    ↓
micro-randomize ZERO / +N / -N / +E / -E / approved duration profile
    ↓
exact accepted T_A
    ↓
bounded contiguous action exposure
    ↓
parent-linked release
    ↓
proximal T_A-relative outcome
```

No formal MRT campaign has run yet.

## Immediate next gate

Exactly one fresh non-scientific SITL qualification is next:

```text
1 bounded-mode ZERO plan
1 bounded-mode nonzero plan
planned_hold_duration_us=20000  # engineering-only qualification value
complete exposure ledger
parent-linked release
FAST active
landing / cleanup
```

The 20 ms hold is not a frozen scientific horizon and must not be promoted into the future MRT contract merely because the smoke passes.

## Qualification / Phase-D state retained

Formal Phase-D/Q1 qualification remains paused, not deleted. Earlier FAST/Phase-D documents and immutable failed roots remain historical authority for that branch.

Engineering runs are not formal scientific evidence and must not be retroactively promoted.

## Scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Current evidence does not authorize `G_ACTION_CAUSAL_VALID`, FAST removal, WISE control authority, or World Model control writes.

## Hard invariants

```text
PX4 remains authoritative
FAST remains the immediate disturbance-response baseline
legacy EVENT_ONLY_V1 remains unchanged
bounded-contiguous mode remains explicit opt-in
World Model must not block first response
candidate action remains bounded incremental augmentation
StateBank remains causal and always warm
historical failed roots remain immutable
large artifacts=/media/nahhao74/KINGSTON
PX4_FIRMWARE_MODIFIED=false
FAST_CONTROL_LAW_MODIFIED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```
