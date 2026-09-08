# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical research documentation for the Moving-Mode UAV Detect & Response pipeline around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model, and WISE predictive refinement.

Large telemetry, runtime roots, datasets, and replay bundles remain outside GitHub under `/media/nahhao74/KINGSTON`. This repository stores architecture, scientific contracts, execution state, compact evidence history, audit reports, and roadmap documents.

## Read order

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state and blocker.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_PHASE_D_20260908.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_PHASE_D_20260908.md) — exact current Phase-D execution ladder.
3. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority and onboarding rules.
4. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — structural end-to-end pipeline.
5. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — control path and PX4 authority.
6. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — timing, causality, and StateBank.
7. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact audit trail.
8. [`docs/03_evidence/phase_d/README.md`](docs/03_evidence/phase_d/README.md) — Phase-D evidence index and lineage.
9. [`docs/05_scientific_contracts/phase_d/README.md`](docs/05_scientific_contracts/phase_d/README.md) — frozen Phase-D B0 science and qualification amendments.
10. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future implementation roadmap.
11. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen WM1 randomized-science contract.
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
production_authority=false
SEALED=LOCKED_PRE_EVALUATION
large artifacts=/media/nahhao74/KINGSTON
```

## Current state — 2026-09-08

The active mainline is **Phase-D B0 characterization closure** after formal D0 V3 readiness closed successfully.

```text
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
READY_FOR_FAST_CHALLENGER_SELECTION=false
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED

INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_REVIEW
```

## Current blocker — checkpoint-C qualification

Offline source/frontier audits established that the ordinary Phase-D path cannot use physical F0 itself as a live pre-emission prefix endpoint:

- ordinary Phase-D uses host-monotonic phase timing and immediate native v1 commands;
- exact physical F0 is only known when Gazebo applies the disturbance;
- a future scheduled target alone does not prove the final pre-target evaluations are already produced, persisted, flushed, and reconciled before an earlier authorization decision.

Therefore the earlier prospective `[V2_ELIGIBLE,F0)` admission rule has been superseded for prospective qualification.

Current approved rule:

```text
PREWIND_POPULATION_V2=[V2_ELIGIBLE,C)
```

where `C` is a fixed source-owned checkpoint that must be prospectively bound before runtime.

```text
[V2,C) = prewind admission
[C,F0)  = transition observation
[F0,...) = scientific response
```

Physical F0 remains native Gazebo application truth and all frozen F0–F4 metric semantics remain unchanged.

At the existing disturbance opportunity:

```text
sealed PASS over [V2,C) -> disturbance eligible
FAIL / UNKNOWN / incomplete prefix / infrastructure invalid -> no disturbance; retain root; stop
```

Forbidden:

```text
no 20 s dwell
no waiting
no retry-until-PASS
no F0 shift
no favorable-state selection
no dynamic C selection
no dynamic prefix shortening
no window restart
```

This is explicitly a qualification/admission contract revision, not a control/science rewrite:

```text
SCIENTIFIC_EXPERIMENT_DESIGN_CHANGED=false
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
PREWIND_QUALIFICATION_CONTRACT_DELTA=[V2,F0) -> [V2,C)
CONTROL_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
```

## Current execution plan

```text
bind exact canonical checkpoint C
→ implement owner upper-watermark through C
→ implement writer flush/accounting through C
→ reconcile C1/E8 through C
→ wire one-shot disturbance authorization
→ exact execution-harness qualification
→ Astra independent audit
→ determine historical comparability and actually missing conditions
→ execution binding
→ Phase-D runtime
→ 8 contract-qualified usable conditions
→ combined characterization and bottleneck classification
→ owner review
```

No DOB/INDI/MPC/FAST challenger is selected during this closure program.

## Scientific target / World Model boundary

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1 baseline
```

World-Model training remains separately gated by causal dataset admission. A future material FAST baseline change requires review/revalidation of action-conditioned `G_action` under the new baseline.
