# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model and WISE predictive refinement.

Large telemetry, replay roots, training datasets and runtime artifacts remain outside GitHub under Kingston storage. This repository keeps the canonical system model, scientific contracts, current execution state, compact evidence trail, roadmap and source registry required to understand or resume the project.

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

A dated historical file may remain in `main` for provenance. It is not current runtime authority when it conflicts with `CURRENT_STATUS.md` or the latest execution ladder.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — exact current infrastructure/science/authority state.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md) — executable sequence for the owner-authorized next fresh pilot.
3. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline and module responsibilities.
4. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, E8 and PX4 authority.
5. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — source-time authority, `T_D`, `T_A`, H1000 and StateBank causality.
6. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — `F_nominal` / `G_action` decomposition and WISE planning role.
7. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen randomized-identification contract and causal-data gate.
8. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact root-cause and qualification trail through `fresh_34` and its repair closure.
9. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — active post-Phase-0 research/development direction.
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
PX4 inner loops remain authoritative
candidate action is bounded incremental augmentation
FAST/T1/C1 baseline remains active
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
PHASE_0B5_QUALIFICATION=VALID_BOUNDED_NOSCIENCE

OPTION_B_CONTRACT_READY=true
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
OPTION_B_LIVE_RUNTIME_QUALIFIED=true

RESET_AUTHORITY=AURA_C1_SOURCE_RESET
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US

WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
NATIVE_EVENT_CLEAR_LIFECYCLE_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
PRE_RETRY_VALID_CAUSAL_CORE=true

LATEST_FAILED_SCIENTIFIC_ROOT=fresh_34
FRESH34_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

OWNER_FRESH_RANDOMIZED_PILOT_AUTHORIZED=true
NEW_FRESH_RANDOMIZED_PILOT_EXECUTED=false
NEXT_STATE=OWNER_AUTHORIZED_NEW_FRESH_RANDOMIZED_PILOT_READY_TO_EXECUTE
```

Canonical current status is always [`CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md).

## Latest closure — `fresh_34` next-status blocker

`fresh_34` passed preflight but stopped fail-closed in the first CALM row with:

```text
NEXT_STATUS_SOURCE_FRONTIER_UNAVAILABLE
RuntimeError:next_status_timeout
```

Forensic proved that valid strict-future native statuses existed. The old E8 mirror filtered them using additional source-health/authority flags before observer publication even though those flags are not part of the canonical `next_status` successor predicate.

Repair:

```text
native status
→ mirror immediately
→ existing ingress/source-health/ACK gate
```

No timeout/QoS/scientific/control semantic was changed.

Bounded qualification root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_next_status_frontier_qualification_20260905_03
```

Key result:

```text
689 strict-future successor lookups
0 timeouts
800 native callbacks
794 mirror callbacks
C1 missing lifecycle=0
writer errors/drops/gaps=0/0/0
graph_valid=true
forbidden_cycles=0
PRE_RETRY_VALID_CAUSAL_CORE=true
```

Historical `fresh_34` remains infrastructure-invalid and contributes no scientific data.

## Current executable action

The owner has authorized one new fresh scientific root using the frozen manifest and all qualified infrastructure unchanged.

```text
new immutable root
→ full preflight
→ exact 8-session / 96-block randomized matrix
→ stop on first invalid block
→ no retry/skip/replace/resample/hot-fix/pooling
→ canonical reverse index / graph / Tarjan / peeling
```

If the root does not complete 8/8 sessions and 96/96 blocks validly:

```text
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

If it completes 96/96 validly, freeze it and perform a separate causal-dataset admission audit before `G_action` identification.

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

## After a complete valid pilot

```text
complete 96/96 valid randomized root
→ causal-dataset admission audit
→ G_action identification
→ action-conditioned World Model
→ WISE predictive refinement
→ uncertainty/calibration ladder
→ causal-learning/adaptation research
→ stronger AEGIS runtime assurance
```

World-Model training remains blocked until causal dataset acceptance and separate owner authorization.

## Post-Phase-0 FASTv2 research direction

A future FAST challenger is being scoped from the main AURA path plus lessons from the independent PID benchmark branch. It does **not** modify the current Phase-0 baseline or PX4 firmware PID.

Working architecture:

```text
PX4 firmware PID/inner loops unchanged
+
AURA disturbance feedforward (-d_hat)
+
T1/C1 temporal continuation
+
bounded residual 2-DOF PI at the qualified acceleration-correction boundary
```

Initial research rule:

```text
fixed robust PI first
anti-windup required
bounded output
source/session/reset/lifecycle-aware integrator
no gain scheduling until a deployable causal scheduling state is demonstrated
INDI only as a later ablation challenger
DOB/ESO/ANN only after a measured residual failure class remains
```

This research is post-Phase-0 only and requires a versioned baseline/control contract before runtime promotion.

## Active long-term research tracks

```text
CTEE     -> temporal/causal event eligibility
CTEE-F   -> eligibility + prospective freshness / AoI
CALE     -> causal admission for action-response learning
CIBES    -> future constrained information-efficient experiment design
FASTv2   -> AURA/T1C1 + bounded residual 2-DOF PI; later INDI by ablation
WM ladder -> F_nominal + G_action model families
uncertainty -> quantile / heteroscedastic / ensemble / conformal / set-membership
WISE     -> bounded predictive candidate selection
AEGIS    -> stronger safety / assurance envelope
```

Research tracks do not override current scientific/runtime authority.

## Storage and authority

Large runtime/capture/dataset/training/intermediate artifacts belong under:

```text
/media/nahhao74/KINGSTON
```

Repository authority remains:

```text
Moving Mode only
FAST/T1/C1 current baseline active
StateBank always warm
PX4 inner loops authoritative
World Model / WISE predictive-refinement only
failed-root scientific conclusions immutable
SEALED locked pre-evaluation
production_authority=false
```
