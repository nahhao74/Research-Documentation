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
8. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — stable pointer to the active v4 CTEE roadmap.
9. [`docs/04_research/AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_GITHUB_INDEX_20260831.md`](docs/04_research/AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_GITHUB_INDEX_20260831.md) — GitHub retrieval/index view of the active roadmap; exact full v4 artifact remains in the Detect and Response Library.
10. [`docs/04_research/RESEARCH_USAGE_GUIDE.md`](docs/04_research/RESEARCH_USAGE_GUIDE.md) — how to use literature and source classes when debugging or extending the pipeline.
11. [`docs/02_source_registry/README.md`](docs/02_source_registry/README.md) — Source Registry policy; current research authority is v8.
12. [`docs/02_source_registry/CURRENT_REGISTRY_V8.md`](docs/02_source_registry/CURRENT_REGISTRY_V8.md) — v8 retrieval index through `SRC-149`.

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

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

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
W_MAX_FULL_PREDICATE_OFFLINE_SUPPORT=UNRESOLVED
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
MANIFEST_SLOTS_CONSUMED=0
SCIENTIFIC_EXECUTION=NOT_RUN
production_authority=false
```

The old `PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT` narration is historical and is not the current blocker.

The closed 0B.1 root cause is:

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

Historical GUST traces identify only reference-side delayed-scheduling chronology. They cannot identify the full no-GUST delayed-launch predicate because the native GUST already occurred and they lack an exact nominal launch timestamp in `PX4_BOOT_US`.

The next evidence gate is therefore a bounded **non-scientific Q1 no-launch shadow qualification**. Its contract is prepared, but runtime is not authorized or executed.

Q1 must:

```text
identify nominal GUST launch opportunity
-> record nominal_requested_launch_source_us in PX4_BOOT_US
-> suppress native GUST for the bounded observation window
-> observe reference / AURA / C1 / session-reset / continuity / provenance
-> evaluate GUST_PREOFFER_REFERENCE_STABILITY_V1 in SHADOW ONLY
-> terminate without GUST launch, treatment, scientific T_D or manifest consumption
```

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

Q1 can create evidence for full-predicate offline timing and a candidate `W_MAX_US` offline coverage envelope. It cannot establish delayed-launch runtime feasibility.

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
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8
UPDATED=2026-08-31
SOURCES=149
RESOLVED=146
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

v8 adds methodological references for Tarjan SCC, max-plus temporal reasoning, three-valued runtime monitoring, sweep-line scientific timeline validation and conditional network-flow allocation. These sources do not authorize changes to the frozen scientific/control contract.

No unverified v8 JSON path or SHA should be inferred.

## Active roadmap — v4 CTEE formalization

The active roadmap keeps Q1 and owner freeze ahead of implementation. After explicit Option-B freeze, the candidate implementation sequence becomes:

```text
Q1 observed no-GUST waiting trajectory
-> owner freezes M_STABLE_US / W_MAX_US / scheduling / source-time policy
-> static dependency graph extraction
-> Tarjan SCC preflight
-> CTEE benchmark / eligibility qualification
   - three-valued predicates
   - source-bound max-plus frontier
   - readiness potential
   - atomic recheck
-> bounded delayed-launch nonscience qualification
-> fresh randomized G_action science
```

CTEE is a **project proposal**, not a flight-control law:

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

It must be benchmarked against the current/ad-hoc state machine and a conventional timed FSM/timed-guard implementation. If no material correctness, tail-latency/jitter or engineering-complexity benefit is demonstrated, use the simpler implementation and retire CTEE.

Algorithm roles remain separated:

```text
Tarjan SCC = static dependency validation
CTEE       = candidate live temporal/causal eligibility
Sweep Line = offline scientific timeline validation
MCMF       = future conditional resource allocation
```

Long-term progression remains evidence-driven:

```text
causal dataset acceptance
-> end-to-end latency + FFT/FRF characterization
-> AURA detector challengers
-> World Model v1 model ladder
-> history / T_D->T_A delay ablations
-> uncertainty-aware prediction
-> WISE candidate enumeration / event-triggered planning
-> TinyMPC / Koopman / RTI only if justified
-> low-dimensional online adaptation
-> formal AEGIS safety/runtime assurance
```

The design principle is the **minimum complexity that produces a demonstrable closed-loop benefit** and **better causal data > larger model**.

## Repository policy

This repository stores canonical architecture/contracts, current readiness state, compact milestone/root-cause summaries, roadmap, Source Registry and research guidance. It does **not** store large telemetry, capture bundles, replay roots, training datasets, scientific runtime roots or high-volume intermediate artifacts. Those remain under:

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