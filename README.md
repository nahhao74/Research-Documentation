# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical research documentation for the Moving-Mode UAV Detect & Response pipeline around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model, and WISE predictive refinement.

Large telemetry, runtime roots, datasets, replay bundles, and generated plots remain outside GitHub under `/media/nahhao74/KINGSTON`. This repository stores architecture, scientific contracts, current execution state, compact evidence history, and research roadmaps.

## Read order

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state.
2. [`docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910.md`](docs/00_overview/CURRENT_STATE_CHECKPOINT_20260910.md) — latest complete handoff/checkpoint.
3. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority and onboarding rules.
4. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — structural end-to-end pipeline.
5. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — control path and PX4 authority.
6. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — timing, causality, and StateBank.
7. [`docs/03_evidence/phase_d/README.md`](docs/03_evidence/phase_d/README.md) — Phase-D evidence index and lineage.
8. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact historical audit trail.
9. [`docs/05_scientific_contracts/phase_d/README.md`](docs/05_scientific_contracts/phase_d/README.md) — retained formal Phase-D contracts.
10. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future implementation roadmap.
11. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — WM1 randomized-science contract.
12. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — source registry.

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

The immediate FAST/T1/C1 path is already functional. The active engineering objective is now **FAST characterization and challenger selection**, not further qualification-first closure.

```text
CURRENT_MODE=ENGINEERING_FAST_CHARACTERIZATION
CURRENT_FAST_BASELINE=PX4_AURA_FAST_T1_C1_CURRENT
FAST_BASELINE_FUNCTIONAL=true
FAST_STRUCTURE_SELECTION=NOT_COMPLETED
TAKEOFF_OFFBOARD_RUNTIME_BASELINE=PASS
FORMAL_PHASE_D_QUALIFICATION=PAUSED_NOT_DELETED
READY_FOR_ENGINEERING_FAST_CHARACTERIZATION=true
READY_FOR_FRESH_Q1_COMPONENT_QUALIFICATION=true
READY_FOR_PHASE_D_RUNTIME=false
FULL_B0_V3_READY=false
```

The latest bounded runtime smoke proved PX4/Gazebo/DDS/offboard/takeoff/hover are functioning after an implementation-preserving Gazebo DART loader repair. No PX4 firmware or control/scientific semantics were changed.

Latest closure note:

[`docs/03_evidence/phase_d/closures/TAKEOFF_OFFBOARD_RUNTIME_SMOKE_20260910.md`](docs/03_evidence/phase_d/closures/TAKEOFF_OFFBOARD_RUNTIME_SMOKE_20260910.md)

## Active engineering ladder

```text
TAKEOFF/OFFBOARD FUNCTIONAL BASELINE       CLOSED
        ↓
TKINTER RESPONSE MONITOR                   NEXT
        ↓
CURRENT FAST REFERENCE RESPONSE            NEXT
        ↓
BOUNDED FAST CHALLENGER COMPARISONS        NEXT
        ↓
SELECT / FREEZE BEST FAST STRUCTURE
        ↓
FORMAL B0 CHARACTERIZATION                 RESUME LATER
        ↓
WORLD MODEL / WISE RANDOMIZED U/ZERO ID
        ↓
AEGIS PREDICTIVE REFINEMENT
```

The Tkinter monitor is intended as a passive engineering observer showing position/velocity setpoint versus actual response, applied disturbance magnitude/direction, and candidate identity. It must remain outside the control loop.

## Qualification state retained

The reduced Q1 component qualification contract is implemented and passed offline qualification. Owner closure remains mandatory, pre-decision exact trace-barrier evidence is auxiliary for the limited Q1 scope, and inactive E8 is explicitly not applicable only for that Q1 component branch.

Formal qualification is paused, not removed. If resumed, the existing Q1/Phase-D contracts and historical immutable roots remain authoritative. Engineering runs are not formal scientific evidence and must not be retroactively promoted.

## FAST selection objective

The current FAST/T1/C1 implementation is the comparison reference, not the final selected structure.

Candidate comparison should focus on identical disturbance profiles and evaluate at least:

```text
response latency
peak position error
peak velocity error
RMS tracking error
recovery time
overshoot / oscillation
control effort
headroom / saturation
trajectory deviation
```

A candidate is not preferred solely because it minimizes position error if it increases velocity spikes, oscillation, attitude excursion, or saturation.

## Scientific target / World Model boundary

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + selected FAST baseline
```

The World Model should be identified against a stable selected FAST baseline. Prototype engineering data may support debugging and architecture development; formal dataset admission remains governed by the retained scientific contracts.

## Hard invariants

```text
PX4 remains authoritative
FAST remains the immediate disturbance-response path
World Model must not block first response
candidate action remains bounded incremental augmentation
StateBank remains causal and always warm
historical failed roots remain immutable
large artifacts=/media/nahhao74/KINGSTON
PX4_FIRMWARE_MODIFIED=false
READY_FOR_PHASE_D_RUNTIME=false
```
