# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model and WISE predictive refinement.

Large telemetry, replay roots, training datasets and runtime artifacts remain outside GitHub under Kingston storage. This repository keeps the canonical architecture, scientific contracts, current execution state, compact evidence trail, roadmap and source registry needed to understand or resume the project.

## Document authority

When documents disagree, use this order:

```text
1. docs/00_overview/CURRENT_STATUS.md
2. latest docs/00_overview/CURRENT_EXECUTION_LADDER_WM_*.md
3. docs/05_scientific_contracts/*
4. docs/01_architecture/*
5. docs/03_evidence/*
6. docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
7. dated / historical documents
```

Detailed policy:

[`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md)

A dated historical file may remain in `main` for provenance. It is not current runtime authority when it conflicts with `CURRENT_STATUS.md` or the latest execution ladder.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state and exact blocker.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md) — next executable forensic/qualification sequence.
3. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end system responsibilities and authority boundaries.
4. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, E8 and PX4 authority.
5. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — source-time authority, `T_D`, `T_A`, H1000 and StateBank causality.
6. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — `F_nominal` / `G_action` decomposition, WISE role and baseline sensitivity.
7. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen randomized-identification contract.
8. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact root-cause and qualification trail through `fresh_35`.
9. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future research direction; not current runtime authority.
10. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — active source registry.

## System structure

The project separates immediate disturbance response from predictive refinement:

```text
                                      PREDICTIVE PATH
Sensors / PX4 / Reference ────────┬────> StateBank (always warm)
                                  │              │
                                  │              v
                                  │       World Model / WISE
                                  │              │ bounded U_plan
                                  │              v
                                  │        AEGIS candidate path
                                  │              │
                                  v              v
                                AURA ─────> AEGIS FAST/T1/C1 ─────> PX4 ─────> UAV
                                                   ^                  │
                                                   └──────────────────┘
                                                   closed-loop response
```

Hard architectural invariants:

```text
World Model must not block first response
PX4 remains authoritative
candidate action is bounded incremental augmentation
FAST/T1/C1 baseline remains active during current Phase-0 science
StateBank is causal and always warm
unsupported/stale WM plan -> candidate ZERO/unavailable, baseline continues
```

## Scientific target

The action-conditioned branch estimates the incremental candidate effect inside the active closed loop:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Post-treatment FAST/PX4 reactions caused by the candidate are part of the realized treatment response. Pre-treatment context must remain causal and outcome-blind.

## Current state — 2026-09-05

```text
MAIN_PIPELINE=ACTIVE

PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID

OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
OPTION_B_LIVE_RUNTIME_QUALIFIED=true
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US

CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
NATIVE_EVENT_CLEAR_LIFECYCLE_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED

LATEST_FAILED_SCIENTIFIC_ROOT=fresh_35
FRESH35_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN

G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

AUTHORIZE_NEW_SCIENTIFIC_ROOT=false
AUTHORIZE_WM_TRAINING=false
AUTHORIZE_FAST_BASELINE_CHANGE=false
NEXT_STATE=OWNER_ACCEPTED_CYCLE_STATUS_VISIBILITY_RETENTION_FORENSIC_REVIEW_REQUIRED
```

Canonical current status is always [`CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md).

## Latest root — `fresh_35`

Immutable root:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm1_v2r1_within_run_randomized_action_20260905_fresh_35
```

The frozen preflight passed, but execution stopped in `WM1V2R1_RAND_A_CALM_R1` before a valid scientific block was admitted:

```text
sessions attempted/valid=1/0 of 8
blocks attempted/valid=1/0 of 96
terminal=RuntimeError:accepted_cycle_timeout
next_status lookups/timeouts=0/0
native GUST=0
candidate/T_D/ACK/exposure=0/0/0/0
manifest slots=0
```

A same-lineage valid native status exists `340000 us` after the requested source frontier, inside the existing `500000 us` match budget. Therefore producer absence is not proven. The observer retained `2000` statuses but no matching status; callback receipt versus later ring eviction was not persisted independently, so eviction is not proven either.

Current classification:

```text
INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN
```

## Current executable action

The next task is a separate non-scientific forensic/minimal-repair review of accepted-cycle status callback visibility and retention.

Required order:

```text
persist exact callback receipt + retention/index identity
→ distinguish not received vs received-then-evicted vs retained-but-unselectable
→ prove scientific semantic delta NONE
→ minimal canonical repair
→ deterministic regression
→ bounded non-scientific qualification
→ canonical reverse/Tarjan/peeling
→ owner review
```

Do not increase timeout or QoS merely to obtain PASS and do not launch another randomized root before this boundary is proven and separately qualified.

## Randomized pilot contract

```text
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354

8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

No incomplete root may produce `G_action` inference.

## FAST research status

The current FAST/T1/C1 baseline is frozen for Phase-0. FAST optimization is allowed only as a separate shadow/replay research track on the current simulator.

Current research question:

> Can a bounded FAST challenger materially improve immediate wind response, tracking and recovery over `-d_hat + T1/C1` without changing PX4 firmware control semantics?

No replacement algorithm is selected. The earlier residual-PI proposal is not the current main-pipeline direction and is not authorized to modify baseline `B`.

## World Model status

The canonical model structure remains:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

WM/WISE is predictive refinement, not first-response control. Action-conditioned WM training remains blocked until a complete valid randomized root passes causal-dataset admission and training receives separate authorization.

If a future FAST challenger is materially promoted, the action-conditioned model must be evaluated under that newly frozen baseline rather than assuming `G_action` transfers unchanged across different FAST semantics.

## Storage and authority

Large runtime/capture/dataset/training/intermediate artifacts belong under:

```text
/media/nahhao74/KINGSTON
```

Repository authority remains:

```text
Moving Mode only
PX4 authoritative
current FAST/T1/C1 baseline frozen for Phase-0
StateBank always warm
World Model / WISE predictive-refinement only
failed-root classifications immutable
SEALED locked pre-evaluation
production_authority=false
```
