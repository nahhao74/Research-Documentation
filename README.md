# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the Moving-Mode UAV Detect & Response pipeline around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model and WISE predictive refinement.

Large telemetry, runtime roots, datasets and replay bundles remain outside GitHub under Kingston storage. This repository tracks current architecture, qualification contracts, execution state, compact evidence history, task reports and active roadmap.

## Read order

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state and blocker.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_D0_V3_20260907.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_D0_V3_20260907.md) — exact current execution sequence.
3. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority/onboarding rules.
4. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — structural end-to-end pipeline.
5. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — control path and PX4 authority.
6. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — timing/causality/StateBank.
7. [`docs/01_architecture/WM_CAUSAL_VALIDITY_ENGINE.md`](docs/01_architecture/WM_CAUSAL_VALIDITY_ENGINE.md) — reverse indexing, Tarjan SCC and peeling.
8. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — World Model / WISE structure.
9. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact audit trail.
10. [`docs/03_evidence/d0_v3/REPORT_INDEX.md`](docs/03_evidence/d0_v3/REPORT_INDEX.md) — immutable D0 V3 task-report index.
11. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — active roadmap.
12. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen WM1 randomized-science contract.
13. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — source registry.

Superseded current-state/ladder content is retained by Git history rather than kept as competing authority in `main`.

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
                              AURA ─────> AEGIS FAST/T1/C1 ─────> PX4 ─────> UAV
```

Hard invariants:

```text
PX4 remains authoritative
FAST remains immediate disturbance-response path
World Model must not block first response
candidate action remains bounded incremental augmentation
StateBank remains causal and always warm
missing/stale/unsupported evidence fails closed
failed roots remain immutable
```

## Current state — 2026-09-07

```text
MAINLINE=D0_V3_CAUSAL_OBSERVABILITY_AND_READINESS
D0_INFRASTRUCTURE_CLOSED=false
READY_FOR_PHASE_D=false

LATEST_FORMAL_D0_ROOT=_04
FORMAL_RESULT=UNKNOWN_MISSING_EVIDENCE

V3_EXPLAINED_CAUSE_REGISTRY=V3_EXPLAINED_CAUSE_REGISTRY_V1
DERIVED_04_VALID=2879
DERIVED_04_EXPLAINED=1116
DERIVED_04_NOT_READY=5
DERIVED_04_UNKNOWN=1

CURRENT_BLOCKER=DATA0_PRECOLLECTOR_STARTUP_ATTESTATION_CONTRACT
NEXT_RUNTIME=ONE_PROVENANCE_PROBE_ONLY_AFTER_OFFLINE_STARTUP_CONTRACT_CLOSURE
```

Current remaining questions:

```text
CAUSAL_EXPECTED_AVAILABLE=false
→ final disposition pending fresh owner-time expected-setpoint provenance

motor_command_stale_or_missing
→ currently NOT_READY_CONTROL_UNAVAILABLE
→ root cause must be resolved before final D0 PASS
```

## What is already qualified

```text
V3 runtime binding / concrete DATA0 backend
startup trace-source state machine
AURA M3/runtime-gate/lookup owner ledger
W20 exact per-evaluation correspondence
C1 exact per-evaluation correspondence
E8 exact per-evaluation correspondence
E8 secondary append-only canonical ingestion
reference collector owned cleanup/finalization
explained-cause registry coverage for known runtime reasons
```

Root `_04` established complete 4001/4001 W20/C1/E8 correspondence with zero recorder drops/gaps/errors and a clean collector reap.

## Current execution plan

```text
offline DATA0 precollector lifecycle/attestation audit
→ deterministic measurement-only repair + offline qualification
→ one expected/motor provenance probe
→ resolve expected + motor-command semantics
→ freeze final registry/evaluator
→ one fresh D0 qualification root
→ D0 CLOSED only when UNKNOWN=0, NOT_READY=0, READINESS_FAILURE=0, INVARIANT_VIOLATION=0
→ freeze Phase-D metrics
→ measure actual FAST bottleneck
→ only then select smallest justified FAST challenger
```

## FAST research boundary

No replacement FAST algorithm is selected. Phase D will first characterize source age/AoI, F0→F4 latency, F5 diagnostic plant response, ONSET/SUSTAINED/CLEAR response, tracking error, recovery, effort, jerk and saturation/headroom.

## Scientific target / World Model boundary

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1 baseline
```

WM training remains blocked until a complete valid causal dataset is separately admitted. A future material FAST baseline change requires review/revalidation of action-conditioned `G_action` under the new baseline.

```text
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
large artifacts=/media/nahhao74/KINGSTON
```
