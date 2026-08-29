# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, AEGIS and an action-conditioned World Model / WISE layer.

This repository is intentionally **architecture-first**. It is not a dump of runtime reports. Detailed raw evidence remains in the project workspace and Kingston storage; this repository keeps the stable system model, scientific contracts, research references, registry snapshots, and compact milestone evidence needed to understand or resume the project.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — current state, completed gates, immediate next gate.
2. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline and module responsibilities.
3. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, PX4 authority and AEGIS path.
4. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — native-time authority, dual-domain timestamp, T_D/T_A, H1000 and StateBank.
5. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — F_nominal / G_action decomposition and predictive planning role.
6. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen randomized identification contract.
7. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact audit trail of the major closures.
8. [`docs/04_research/RESEARCH_USAGE_GUIDE.md`](docs/04_research/RESEARCH_USAGE_GUIDE.md) — how to use literature and source classes when debugging or changing the pipeline.
9. [`docs/02_source_registry/README.md`](docs/02_source_registry/README.md) — source-registry policy and archived registry snapshots.

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

The action-conditioned research target is:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

where:

```text
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The candidate is a bounded incremental treatment applied on top of the active baseline. Downstream FAST/PX4 reactions caused by the candidate are part of the realized closed-loop treatment response.

## Repository policy

This repository stores:

- canonical architecture and contracts;
- current readiness state;
- compact milestone/root-cause summaries;
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
- SEALED: locked before approved final evaluation.
- `production_authority=false`.
- Failed scientific roots are immutable; no patch-and-continue or pooling of invalid partial roots.
- Large artifacts belong on Kingston, not `/home`.

## Current status

The bootstrap readiness race and specialized `bootstrap_only` call-site defect are both closed and exact-preflight qualified. The system is currently at:

```text
READY_FOR_OWNER_FRESH_RANDOMIZED_PILOT_RETRY_REVIEW
```

No complete valid 8-session / 96-block randomized pilot has yet been accepted, therefore action-conditioned WM training is not yet scientifically authorized.