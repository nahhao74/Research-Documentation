# Source Registry

This folder stores the source registry used to research, audit and extend the AURA–WISE–WM–AEGIS pipeline.

## Current registry identity

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8
UPDATED=2026-08-31
sources=149
resolved=146
unresolved=3
LIBRARY_ARTIFACT=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8.md
```

Use:

- [`SOURCE_REGISTRY_CURRENT.md`](SOURCE_REGISTRY_CURRENT.md) for the current authority/pointer policy.
- [`CURRENT_REGISTRY_V8.md`](CURRENT_REGISTRY_V8.md) for the current v8 retrieval index and exact v8 additions `SRC-142..SRC-149`.
- [`CURRENT_REGISTRY_V7.md`](CURRENT_REGISTRY_V7.md) for the historical v7 retrieval index through `SRC-141`.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) for the historical retrieval index through `SRC-123`.
- `library_snapshots/` for historical v1/v2 registry states.

The exact v8 Markdown artifact is held in the Detect and Response Project/File Library. A v8 machine-readable JSON artifact/path/hash has not been promoted into this repository; do not invent one.

## Registry lineage

### v1
87 sources. Locators had not yet been resolved and were intentionally left unresolved rather than guessed.

### v2
87 sources; 84 locators resolved, with three unresolved project-internal identities.

### v3
Added pinned PX4 v1.15/v1.15.4 uXRCE-DDS and eProsima transport/runtime sources (`SRC-088..SRC-094`).

### v4
Added PX4/Gazebo startup sources plus vetted research on uncertainty-aware MPC, closed-loop UAS identification, residual learning and world-model/planning methods (`SRC-095..SRC-103`).

### v5
Added latency/performance and low-compute predictive-control sources (`SRC-104..SRC-123`).

### v6
Added `SRC-124..SRC-127`: uncertainty-aware learned residual MPC, online dynamics learning, latency-aware quadrotor control and unified safety-filter/runtime-assurance review.

### v7
Added `SRC-128..SRC-141` for DDS/ROS 2 latency, scheduling, change detection, learned/adaptive disturbance modeling and runtime assurance.

### v8
Added `SRC-142..SRC-149` for:

```text
static dependency correctness / Tarjan SCC
max-plus timed-event reasoning
three-valued runtime monitoring
sweep-line temporal interval validation
future conditional network-flow / matching allocation
```

No existing source ID was deleted or renumbered.

## v8 adoption policy

Do not add an algorithm merely because it appears in the registry.

```text
candidate algorithm
-> named project bottleneck / failure class
-> measurable KPI
-> simplest baseline comparison
-> shadow / offline / preflight evidence
-> retain only if benefit is demonstrated
```

Current active research direction after Q1 + owner freeze is to evaluate static Tarjan SCC preflight and the CTEE proposal (three-valued predicates + source-bound max-plus frontier + atomic recheck) against simpler timed-FSM/timed-guard baselines. Sweep Line remains offline validation; MCMF remains future/conditional.

CTEE is not a flight-control law and is not automatically promoted:

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

## Retrieval policy

Prefer sources in this order:

```text
exact local source/build + captured runtime evidence
-> explicitly frozen project contract
-> version-matched official source/docs
-> primary paper / canonical DOI / original repository
-> institutional repository
-> secondary synthesis for orientation
-> issue/forum for hypothesis generation only
```

For runtime implementation questions, local pinned evidence overrides generic upstream documentation when they diverge.

## Core categories

```text
px4_autopilot
runtime_timing
real_time_scheduling
causal_temporal_eligibility
runtime_verification
temporal_interval_validation
uav_control
disturbance_observer
change_detection
closed_loop_identification
micro_randomized_trials
small_sample_cluster_inference
world_model_ml
model_predictive_control
embedded_optimization
residual_dynamics
online_adaptation
uncertainty_aware_control
runtime_assurance
network_optimization
```

Use [`../04_research/RESEARCH_USAGE_GUIDE.md`](../04_research/RESEARCH_USAGE_GUIDE.md) for problem-to-source-family guidance.

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

Do not silently substitute a similar internal Markdown file for these identities.

## Maintenance rule

Do not invent a locator. Add or promote a source only when its identity is explicit or independently verified. Authority classification describes provenance, not truth of every claim in a source.

Research references are design/audit inputs only. They do not authorize changes to frozen control/scientific semantics, treatment design, safety limits, acceptance criteria, SEALED or production authority.