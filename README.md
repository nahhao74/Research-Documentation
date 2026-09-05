# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model and WISE predictive refinement.

This repository is architecture-first. Large telemetry, replay roots, training datasets and runtime artifacts stay outside GitHub under Kingston storage; this repository keeps the canonical system model, scientific contracts, current execution state, compact evidence trail, roadmap and source registry needed to understand or resume the project.

## Document authority — read this first

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

A date-stamped historical file may remain in `main` for provenance. It is **not** current runtime authority when it conflicts with `CURRENT_STATUS.md` or the latest execution ladder.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — exact current blocker, qualified closures, authority state and next gate.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md) — exact executable sequence from the current `fresh_33` state.
3. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline and module responsibilities.
4. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, E8 and PX4 authority.
5. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — source-time authority, `T_D`, `T_A`, H1000 and StateBank causality.
6. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — `F_nominal` / `G_action` decomposition and WISE planning role.
7. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen randomized-identification contract and causal-data gate.
8. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact root-cause and qualification trail through `fresh_33`.
9. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — active v6 research/development direction after current Phase-0 scientific closure.
10. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — active source registry.

## What the system does

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
PX4 inner loops remain authoritative
candidate action is bounded incremental augmentation
FAST/T1/C1 baseline remains active
StateBank is causal and always warm
unsupported/stale WM plan -> candidate ZERO/unavailable, baseline continues
```

## Scientific target

The action-conditioned branch does **not** identify a controller-free airframe. It estimates the incremental candidate effect inside the active closed loop:

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
PHASE_0B5_QUALIFICATION=VALID_BOUNDED_NOSCIENCE

OPTION_B_CONTRACT_READY=true
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
OPTION_B_LIVE_RUNTIME_QUALIFIED=true

RESET_AUTHORITY=AURA_C1_SOURCE_RESET
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US

WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED
STATUS_OBSERVER_SOURCE_FRONTIER_REPAIR=CLOSED_QUALIFIED
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
POST_RESET_E8_SOURCE_CAUSAL_HANDOFF_QUALIFICATION=VALID_NONSCIENTIFIC

FRESH_RANDOMIZED_G_ACTION_PILOT=INCOMPLETE_INVALID_INFRASTRUCTURE_ROOT
FRESH_RANDOMIZED_G_ACTION_PILOT_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

NEXT_STATE=INFRASTRUCTURE_REPAIR_REQUIRED_BEFORE_FRESH_RANDOMIZED_PILOT
```

Canonical current status is always [`CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md).

## Current blocker — `fresh_33`

`fresh_33` passed the qualified preflight and completed four CALM session rows, then stopped fail-closed in the first `GUST_E` row.

First invalid implementation boundary:

```text
block 2 native event still ACTIVE
→ block 3 arm request
→ PREVIOUS_EVENT_STILL_ACTIVE
→ block 3 native event not launched
→ downstream event-window timeout
→ immutable infrastructure-invalid root
```

This is not a scientific negative result. Partial rows are not pooled or analyzed as treatment evidence.

## Immediate next gate

The next implementation task is to synchronize inter-block native-event lifecycle using the canonical previous-event `CLEAR` / retirement state, not an arbitrary sleep:

```text
previous exact native event ACTIVE
→ observe matching CLEAR / retirement
→ next block becomes eligible to arm
```

Required sequence:

```text
lifecycle ownership forensic
→ prove inter-block wait preserves frozen within-block timing
→ minimal implementation-preserving CLEAR gate repair
→ deterministic regression
→ bounded non-scientific consecutive-event qualification
→ reverse processing + peeling
→ require PRE_RETRY_VALID_CAUSAL_CORE=true
→ owner review
→ only then authorize a new fresh 8-session / 96-block randomized root
```

## Randomized pilot contract

Current authoritative pilot identity:

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

## After a complete valid pilot

```text
complete 96/96 valid randomized root
→ causal-dataset admission audit
→ G_action identification
→ action-conditioned World Model
→ WISE predictive refinement
→ uncertainty/calibration ladder
→ causal-learning/adaptation research (CALE)
→ stronger AEGIS runtime assurance
```

World-Model training remains blocked until causal dataset acceptance.

## Active development/research direction

The active long-term roadmap is v6. Its priority order is:

```text
causal estimand + exact treatment identity
>
causal learning-admission invariants
>
minimal numerical backend
>
higher model capacity
```

Key future tracks:

```text
CTEE     -> temporal/causal event eligibility
CTEE-F   -> eligibility + prospective freshness / AoI
CALE     -> causal admission for action-response learning
CIBES    -> future constrained information-efficient experiment design
WM ladder -> F_nominal + G_action model families
uncertainty -> quantile / heteroscedastic / ensemble / conformal / set-membership
WISE     -> bounded predictive candidate selection
AEGIS    -> stronger safety / assurance envelope
```

These research tracks do not override current scientific/runtime authority.

## Storage and authority

Large runtime/capture/dataset/training/intermediate artifacts belong under:

```text
/media/nahhao74/KINGSTON
```

Repository authority remains:

```text
Moving Mode only
FAST/T1/C1 baseline active
StateBank always warm
PX4 inner loops authoritative
World Model / WISE predictive-refinement only
failed scientific roots immutable
SEALED locked pre-evaluation
production_authority=false
```
