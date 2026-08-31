# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical documentation for the **Moving-Mode UAV Detect & Response pipeline** built around PX4, AURA, FAST/T1/C1, AEGIS and an action-conditioned World Model / WISE predictive-refinement layer.

This repository is architecture-first. Large telemetry, replay roots, training datasets and runtime artifacts remain outside GitHub under Kingston storage; this repository keeps the current system model, scientific contracts, research registry, roadmap and compact milestone evidence required to understand or resume the project.

## Start here

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — canonical current blocker, authority state and next gate.
2. [`docs/00_overview/CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md`](docs/00_overview/CURRENT_EXECUTION_LADDER_PHASE0B2_20260831.md) — exact Phase-0 execution ladder; prevents research work from being mistaken for runtime authorization.
3. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — active **v5** roadmap.
4. [`docs/04_research/ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md`](docs/04_research/ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md) — detailed CTEE/CTEE-F/CIBES/uncertainty research note.
5. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — active Source Registry v9 retrieval/index view.
6. [`docs/02_source_registry/README.md`](docs/02_source_registry/README.md) — registry policy and lineage.
7. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — end-to-end pipeline responsibilities.
8. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — FAST/T1/C1, candidate composition, AEGIS and PX4 authority.
9. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — native-time authority, T_D/T_A, H1000 and StateBank.
10. [`docs/01_architecture/WORLD_MODEL_WISE.md`](docs/01_architecture/WORLD_MODEL_WISE.md) — F_B/G_action decomposition and predictive planning role.
11. [`docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md`](docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md) — randomized-identification contract and causal-data gate.
12. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — compact milestone/root-cause evidence trail.

## Canonical pipeline

```text
                                      predictive path
Sensors / PX4 / Reference ────────┬────> StateBank (always warm)
                                  │              │
                                  │              v
                                  │       World Model / WISE
                                  │              │ bounded U_plan
                                  │              v
                                  │        AEGIS ActivePlan
                                  │              │
                                  v              v
                                AURA ─────> AEGIS FAST/T1/C1 ─────> PX4 ─────> UAV
                                                   ^                  │
                                                   └──────────────────┘
                                                   closed-loop response
```

The fast path must work without the World Model. The predictive path refines the active baseline; it never blocks first response. PX4 inner loops remain authoritative.

## Scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The candidate is a bounded incremental treatment on top of the active baseline. Post-treatment FAST/PX4 reactions caused by the candidate remain part of the realized closed-loop treatment response.

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
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=NOT_IDENTIFIABLE
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

Closed 0B.1 root-cause class:

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

The immediate next gate is **owner authorization for Q1 no-launch nonscientific runtime**.

Q1 must record the nominal GUST opportunity in `PX4_BOOT_US`, suppress native GUST for the bounded observation window, observe reference/AURA/C1/session-reset/continuity/provenance and evaluate the future reference-stability predicate in shadow only.

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

Q1 cannot freeze `M_STABLE_US`, `W_MAX_US`, prove delayed-launch runtime feasibility, estimate `G_action`, consume scientific manifest slots or open SEALED.

## Current Phase-0 ladder

```text
Q1 no-launch observation
→ offline full-predicate characterization
→ owner M_STABLE/W_MAX/policy freeze
→ Tarjan structural preflight
→ CTEE vs timed FSM vs timed runtime-enforcer benchmark
→ chosen minimal Option-B implementation
→ deterministic regression
→ delayed-launch nonscience qualification
→ owner scientific review
→ fresh randomized G_action science
→ causal dataset acceptance
```

World-Model action-response training remains blocked until causal dataset acceptance.

## Active roadmap — v5

Roadmap v5 keeps the current Phase-0 authority boundary but adds four evidence-gated research tracks.

### 1. Stronger CTEE prior-art benchmark

CTEE must now compete against:

```text
ad-hoc state machine
conventional timed FSM
timed-automata/runtime-enforcement baseline
CTEE
```

Timed runtime enforcement already supports delaying/suppressing events, so CTEE cannot claim novelty merely for delay-until-admissible behavior.

Potential CTEE contribution remains narrower:

```text
pre-treatment causal eligibility
+
source-time/session/reset/generation provenance
+
arm neutrality
+
max-plus earliest admissible frontier
+
atomic generation-safe commit
+
bounded fail-closed execution
```

```text
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
```

If a simpler timed solution is equivalent or better, use it and retire CTEE.

### 2. CTEE-F — future freshness-aware extension

```text
CTEE-F = Causal Temporal Eligibility and Freshness Engine
```

CTEE-F combines causal eligibility with information freshness and a justified remaining-delay envelope:

```text
CTEE frontier
+
Age of Information
+
remaining-delay model/bound
+
atomic identity recheck
```

Goal:

> do not merely ask whether a state/plan is admissible now; ask whether it will still be fresh when PX4 consumes it.

CTEE-F is a future Phase-1+/shadow hypothesis and does not unblock Q1.

### 3. CIBES — future experiment-design track

```text
CIBES = Causal Information-Budgeted Excitation Scheduler
```

After the current frozen Phase-0 science closes, CIBES may investigate safe probing actions that maximize expected information under operational and causal constraints.

It is **not permitted inside the current frozen randomized pilot** because adaptive excitation changes the experimental design.

### 4. World-Model uncertainty ladder

```text
residual quantiles
→ heteroscedastic variance
→ ensemble/bootstrap
→ conformal error bounds
→ set-membership uncertainty
→ tractable ellipsoidal outer approximation if justified
```

Retain advanced uncertainty only if it improves calibration/OOD behavior and downstream WISE/AEGIS utility over simpler baselines.

## Source Registry v9

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9
UPDATED=2026-08-31
SOURCES=156
RESOLVED=153
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

v9 adds:

```text
SRC-150 timed runtime enforcement
SRC-151 Age of Information
SRC-152 Network Calculus
SRC-153 adaptive constrained experiment design
SRC-154 conformalized robust OOD MPC / 12D quadcopter
SRC-155 set-membership uncertainty learning
SRC-156 ellipsoidal set-membership approximation
```

These sources support hypotheses only; they do not authorize a scientific/control change.

## Long-term progression

```text
Phase-0 causal dataset acceptance
→ end-to-end latency + FFT/FRF
→ AoI / age-at-application / remaining-delay characterization
→ CTEE-F shadow benchmark
→ AURA detector challengers
→ World Model model ladder
→ history + T_D→T_A delay ablations
→ uncertainty calibration ladder
→ WISE candidate enumeration / event-triggered planning
→ TinyMPC / Koopman / RTI only if justified
→ low-dimensional online adaptation
→ formal AEGIS Lyapunov/CLF/CBF runtime assurance
```

## Design principle

```text
measure
→ identify bottleneck
→ introduce smallest credible improvement
→ compare against simpler baseline
→ validate causality/timing/runtime cost
→ integrate only if benefit is real
```

The project optimizes for measurable closed-loop value, not algorithm count.

## Storage and authority

Large runtime/capture/dataset/training/intermediate artifacts belong under:

```text
/media/nahhao74/KINGSTON
```

Repository authority remains:

```text
Moving Mode only
FAST/T1/C1 baseline active
StateBank always warm
PX4 inner loops authoritative
World Model / WISE predictive-refinement only
SEALED locked pre-evaluation
failed scientific roots immutable
production_authority=false
```
