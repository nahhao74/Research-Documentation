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
9. [`docs/05_scientific_contracts/phase_d/README.md`](docs/05_scientific_contracts/phase_d/README.md) — frozen Phase-D B0 contract package.
10. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future implementation roadmap.
11. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — frozen WM1 randomized-science contract.
12. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — source registry.

Superseded current-state and execution-ladder documents are retained by Git history or dated filenames and are not competing current authority.

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

The active mainline is **Phase-D B0 characterization closure**, after formal D0 V3 readiness closed successfully.

```text
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
READY_FOR_FAST_CHALLENGER_SELECTION=false

INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
F0_REACHED_ACQUISITION_ATTEMPTS=6
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_AUDIT

CURRENT_BLOCKER=PHASE_D_PREWIND_EVIDENCE_BOUNDARY_AND_LIVE_PREFIX_COMPLETENESS
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
```

The frozen Phase-D contract requires the qualified V3/V2 readiness gate before native F0. A 20-second Phase-D prewind dwell is **not** frozen or required.

The unresolved issue is narrower: define and prove the canonical pre-F0 evaluation population and live completeness checkpoint while writers remain active, then wire that decision directly onto the native-F0 emission path without changing FAST/control semantics.

## Current approved Phase-D prewind rule

```text
population = every canonical V3 evaluation from first V2 eligibility
             up to but excluding scheduled native F0

decision boundary = one-shot checkpoint immediately before the frozen F0 opportunity

PASS    -> F0 eligible
FAIL    -> F0 forbidden
UNKNOWN -> F0 forbidden
INFRASTRUCTURE_INVALID -> F0 forbidden
```

Forbidden:

```text
no 20 s dwell
no moving F0
no retry-until-PASS
no favorable-state selection
no dynamic prefix shortening
no window restart
```

A control-inert prefix-completeness seal may be added to prove that the closed prewind prefix is fully persisted/reconciled before F0. End-of-row writer/C1/E8/collector/finalization obligations remain separate and mandatory.

## Phase-D evidence status

Historical slots 1–5 remain recorded but are currently classified:

```text
UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

Canonical replay proves their pre-F0 evaluation populations are eventually accountable when finalized downstream evidence is supplied. That does **not** yet prove the same evidence was complete at the live pre-F0 boundary.

The missing-marker finding therefore does not imply a FAST/control failure, and the five rows are not silently discarded or promoted.

## Current execution plan

```text
canonical V3 -> Phase-D prewind semantic mapping
→ predicate-by-predicate historical audit
→ exact prewind evidence boundary / prefix completeness definition
→ minimal control-inert wiring
→ exact execution-harness qualification
→ Astra independent audit
→ freeze only actually missing execution conditions
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
