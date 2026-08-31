# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, AEGIS and an action-conditioned World Model / WISE layer.

This repository is intentionally **architecture-first**. It is not a dump of runtime reports. Detailed raw evidence remains in the project workspace and Kingston storage; this repository keeps the stable system model, scientific contracts, research references, roadmap and compact milestone evidence needed to understand or resume the project.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — current blocker, completed closures and exact next gate.
2. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline and module responsibilities.
3. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, PX4 authority and AEGIS path.
4. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — native-time authority, dual-domain timestamp, T_D/T_A, H1000 and StateBank.
5. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — F_nominal / G_action decomposition and predictive planning role.
6. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — randomized-identification contract and causal-data gate.
7. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact audit trail of major closures and failed-root causes.
8. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — stable pointer to the active future roadmap.
9. [`docs/04_research/AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v2_20260831.md`](docs/04_research/AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v2_20260831.md) — full active v2 roadmap snapshot.
10. [`docs/04_research/RESEARCH_USAGE_GUIDE.md`](docs/04_research/RESEARCH_USAGE_GUIDE.md) — how to use literature and source classes when debugging or extending the pipeline.
11. [`docs/02_source_registry/README.md`](docs/02_source_registry/README.md) — Source Registry policy; current research authority is v7.

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
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The candidate is a bounded incremental treatment applied on top of the active baseline. Downstream FAST/PX4 reactions caused by the candidate are part of the realized closed-loop treatment response.

## Current status — Phase 0B.2

Structural cleanup, Phase 0A mechanism closure and the GUST-P1 0B.1 forensic are complete.

The current state is:

```text
PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED
OPTION_B_DIRECTION=PREFERRED
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

REFERENCE_STABILITY_PREDICATE=
GUST_PREOFFER_REFERENCE_STABILITY_V1

AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=NOT_IDENTIFIABLE
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

The old `PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT` narration is historical and is not the current blocker.

The closed 0B.1 root cause is:

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

Historical traces allow only **reference-side** delayed-scheduling characterization. They cannot identify the full no-GUST delayed-launch counterfactual because the native GUST had already occurred, and they do not contain an exact nominal launch timestamp in `PX4_BOOT_US`.

The next evidence gate is therefore a bounded **non-scientific Q1 no-launch shadow qualification**. Its contract is prepared, but runtime is not yet authorized or executed. Q1 records the nominal launch opportunity in `PX4_BOOT_US`, suppresses native GUST for the bounded observation window, and evaluates the full reference-stability predicate in shadow only.

Q1 does not authorize Option-B implementation, science, treatment, SEALED access or `W_MAX` runtime-feasibility claims.

## Phase-0 ladder

```text
0A mechanism / structural closure                         CLOSED
0B.1 GUST-P1 timing/design forensic                       CLOSED
0B.2a historical/reference-only characterization          COMPLETE TO EVIDENCE LIMIT
0B.2b Q1 no-launch shadow preparation                     PREPARED
0B.2c Q1 live no-launch characterization                  OWNER AUTHORIZATION REQUIRED
0B.2d owner numeric margin/policy freeze                  PENDING
0B.3 Option-B implementation                              BLOCKED
0B.4 deterministic regression                             BLOCKED
0B.5 delayed-launch nonscience qualification              BLOCKED
0B.6 owner scientific-pilot review                        BLOCKED
0B.7 fresh randomized science                             BLOCKED
0B.8 scientific analysis                                  BLOCKED
0B.9 causal dataset acceptance                            BLOCKED
```

World-Model action-response training remains blocked until causal dataset acceptance.

## Current research registry

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7
UPDATED=2026-08-29
SOURCES=141
RESOLVED=138
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

No unverified v7 JSON path or SHA should be inferred.

## Future roadmap

The active v2 roadmap is evidence-driven and orders work approximately as:

```text
Q1 no-launch characterization
-> owner M_STABLE / W_MAX / scheduling freeze
-> delayed-launch nonscience qualification
-> fresh randomized G_action identification
-> end-to-end latency + FFT/FRF characterization
-> AURA detector challengers
-> World Model v1 model ladder
-> history and T_D->T_A delay ablations
-> uncertainty
-> WISE candidate enumeration / event-triggered planning
-> TinyMPC / Koopman / RTI only if justified
-> low-dimensional online adaptation
-> formal AEGIS Lyapunov / CLF / CBF runtime assurance
```

The design principle is the **minimum complexity that produces a demonstrable closed-loop benefit**.

## Repository policy

This repository stores:

- canonical architecture and contracts;
- current readiness state;
- compact milestone/root-cause summaries;
- future implementation roadmap;
- Source Registry and research guidance;
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
- World Model / WISE: predictive refinement only; not first-response authority.
- SEALED: locked before approved final evaluation.
- `production_authority=false`.
- Failed scientific roots are immutable; no patch-and-continue or pooling of invalid partial roots.
- Large artifacts belong on Kingston, not `/home`.
