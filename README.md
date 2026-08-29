# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, AEGIS and an action-conditioned World Model / WISE layer.

This repository is intentionally **architecture-first**. It is not a dump of runtime reports. Detailed raw evidence remains in the project workspace and Kingston storage; this repository keeps the stable system model, scientific contracts, research references, roadmap and compact milestone evidence needed to understand or resume the project.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — latest blocker, completed closures and exact next integration gate.
2. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline and module responsibilities.
3. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, PX4 authority and AEGIS path.
4. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — native-time authority, dual-domain timestamp, T_D/T_A, H1000 and StateBank.
5. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — F_nominal / G_action decomposition and predictive planning role.
6. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen randomized identification contract and pre-science corridor gate.
7. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact audit trail of the major closures and failed-root causes.
8. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — long-term latency, World Model, WISE, adaptation and runtime-assurance roadmap.
9. [`docs/04_research/RESEARCH_USAGE_GUIDE.md`](docs/04_research/RESEARCH_USAGE_GUIDE.md) — how to use literature and source classes when debugging or changing the pipeline.
10. [`docs/02_source_registry/README.md`](docs/02_source_registry/README.md) — source-registry policy and archived registry snapshots.

## Canonical pipeline

```text
                                      predictive path
Sensors / PX4 / Reference ────────┬────> StateBank (always warm)
                                  │              │
                                  │              v
                                  │       World Model / WISE
                                  │              │ candidate U_plan
                                  │              v
                                  │        AEGIS ActivePlan
                                  │              │
                                  v              v
                                AURA ─────> AEGIS FAST/T1/C1 ─────> PX4 ─────> UAV
                                                   ^                  │
                                                   └──────────────────┘
                                                   closed-loop response
```

The **fast path must work without the World Model**. The World Model is a predictive refinement layer; it never blocks first response. PX4 inner loops remain authoritative.

## Scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

with:

```text
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The candidate is a bounded incremental treatment applied on top of the active baseline. Downstream FAST/PX4 reactions caused by the candidate are part of the realized closed-loop treatment response.

## Current status

The infrastructure stack is close to scientific readiness, but the latest randomized-pilot attempt still stopped **before `FIRST_SCIENTIFIC_T_D`** at:

```text
PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT
```

Gazebo/world identity, source/provenance, StateBank 7/7 bootstrap and H1000 session-start lifecycle are closed. The current unresolved issue is that C1 callbacks continue while the canonical/published C1 source frontier remains zero. A stale `epoch` vs native PX4 `boot` domain expectation is a strong hypothesis but has not yet been proven as the deep root cause.

The next gate is therefore **not another randomized pilot**. It is a full integrated pre-science corridor using the real pilot path:

```text
startup
-> source attestation
-> StateBank bootstrap
-> first-block snapshot
-> C1 valid-offer frontier
-> pre-offer source/clock/provenance
-> intentional stop before scientific T_D/candidate
```

Only after this corridor passes should a fresh randomized scientific root be executed.

No complete valid 8-session / 96-block randomized pilot has yet been accepted, so action-conditioned WM training is not yet scientifically authorized.

## Repository policy

This repository stores:

- canonical architecture and contracts;
- current readiness state;
- compact milestone/root-cause summaries;
- future implementation roadmap;
- source registry and research guidance;
- stable hashes/identities only when needed to understand a frozen contract.

It does **not** store large telemetry, capture bundles, replay roots, training datasets, scientific runtime roots or high-volume intermediate artifacts. Those remain under:

```text
/media/nahhao74/KINGSTON
```

## Current authority

- Scope: **Moving Mode only**.
- FAST/T1/C1 baseline: active.
- StateBank: always warm.
- PX4 inner loops: authoritative.
- Canonical scientific world: plugin-bearing `sim_world_a` with frozen native-disturbance semantics.
- SEALED: locked before approved final evaluation.
- `production_authority=false`.
- Failed scientific roots are immutable; no patch-and-continue or pooling of invalid partial roots.
- Large artifacts belong on Kingston, not `/home`.
