# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model and WISE predictive refinement.

Large telemetry, replay roots, datasets and runtime artifacts live outside GitHub under Kingston storage. This repository contains the current architecture, scientific contract, execution state, compact evidence trail, active roadmap and source registry.

## Read order for humans and AI agents

Use this order. Do not infer current state from research references or Git history.

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state and blocker.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md) — exact next executable sequence.
3. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority and AI onboarding rules.
4. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline.
5. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition and PX4 authority.
6. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — native timing, `T_D`, `T_A`, H1000 and StateBank.
7. [`docs/01_architecture/WM_CAUSAL_VALIDITY_ENGINE.md`](docs/01_architecture/WM_CAUSAL_VALIDITY_ENGINE.md) — reverse indexing, Tarjan and fixed-point peeling.
8. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — `F_nominal`, `G_action`, WISE and baseline dependency.
9. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen randomized scientific contract.
10. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact evidence/root-cause history.
11. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — active future direction only.
12. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — methodological source index only.

Superseded execution ladders, roadmap versions, rejected research directions and old registry snapshots are retained by **Git history**, not as competing current documents in `main`.

## Pipeline structure

The project deliberately separates immediate disturbance response from predictive refinement.

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
                              AURA ─────> AEGIS FAST/T1/C1 ─────> PX4 ─────> UAV
                                                 ^                  │
                                                 └──────────────────┘
                                                 closed-loop response
```

Hard invariants:

```text
PX4 remains authoritative
World Model must not block first response
current FAST/T1/C1 baseline remains active during Phase-0 science
candidate action is bounded incremental augmentation
StateBank is causal and always warm
stale/unsupported WM plan -> candidate ZERO/unavailable
```

## Scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1 baseline
```

This is **closed-loop incremental action identification**. The experiment does not disable PX4/FAST to recover an open-loop airframe model.

Post-treatment FAST/PX4 reactions caused by a candidate are part of the realized treatment response. Pre-treatment state must remain causal and outcome-blind.

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

## Latest root — `fresh_35`

Immutable root:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm1_v2r1_within_run_randomized_action_20260905_fresh_35
```

The frozen preflight passed, then the first CALM row stopped with:

```text
RuntimeError:accepted_cycle_timeout
sessions attempted/valid=1/0 of 8
blocks attempted/valid=1/0 of 96
next_status lookups/timeouts=0/0
native GUST=0
candidate/T_D/ACK/exposure=0/0/0/0
manifest slots=0
```

A same-lineage valid native status exists `340000 us` after the requested source frontier, inside the existing `500000 us` source-match budget. Therefore producer absence is not proven.

The observer had `status_count=2000` but no matching status. Callback receipt versus bounded-retention loss was not independently persisted, so eviction is not proven either.

## Current executable action

```text
accepted-cycle callback visibility / retention forensic
→ prove earliest implementation boundary
→ minimal implementation-preserving repair if required
→ deterministic regression
→ bounded non-scientific qualification
→ canonical reverse / Tarjan / peeling audit
→ owner review
```

Do not increase timeout or QoS merely to obtain PASS. Do not start another scientific root before this boundary is proven and separately qualified.

## Frozen randomized pilot identity

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

## Active research direction

### FAST

The current FAST/T1/C1 baseline stays frozen for Phase-0. FAST improvement runs only as a separate simulator shadow/replay track.

Research sequence:

```text
measure source age / end-to-end latency / onset-sustained-clear response
→ identify the actual FAST limitation
→ test the smallest justified challenger
→ retain only repeat-supported improvement
```

No replacement FAST algorithm is currently selected.

### World Model / WISE

Canonical structure:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

Action-conditioned WM training remains blocked until a complete valid randomized root passes causal-dataset admission and training receives separate authorization.

If FAST semantics are materially changed later, `G_action` must be re-evaluated under the newly frozen baseline before production model use.

After the best FAST baseline is established, WM/WISE must prove incremental value over that baseline; it is not automatically a required production dependency.

## Storage and authority

```text
large artifacts -> /media/nahhao74/KINGSTON
failed-root classifications -> immutable
SEALED -> LOCKED_PRE_EVALUATION
production_authority=false
```
