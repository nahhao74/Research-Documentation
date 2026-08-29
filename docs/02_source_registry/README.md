# Source Registry

This folder stores the source registry used to research, audit and extend the AURA–WISE–WM–AEGIS pipeline.

## Current registry identity

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v5
sources=123
resolved=120
unresolved=3
LOCAL_CANONICAL_JSON_SHA256=acc056ca6c475fb00c6173b54b7a4f779446c21588134bdb5ee23571fc1d16a2
LOCAL_CANONICAL_MD_SHA256=5d8221715ce87f61c089ed53e4c0666851551b4a1591c20d4fd2fc40d71df6b7
```

Use:

- [`SOURCE_REGISTRY_CURRENT.md`](SOURCE_REGISTRY_CURRENT.md) for the current authority/pointer policy.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) for the compact searchable v5 retrieval index and exact post-v2 locators.
- `library_snapshots/` for historical v1/v2 registry states.

## Registry lineage

### v1

87 sources. Locators had not yet been resolved and were intentionally left unresolved rather than guessed.

### v2

87 sources; 84 locators resolved, with three unresolved project-internal identities.

### v3

Added pinned PX4 v1.15/v1.15.4 uXRCE-DDS and eProsima transport/runtime sources (`SRC-088..SRC-094`).

### v4

Added PX4/Gazebo startup sources plus vetted research on uncertainty-aware MPC, closed-loop UAS identification, residual learning and world-model/planning methods (`SRC-095..SRC-103`). `SRC-075` was deduplicated by adding PMC/DOI/PubMed alternate locators rather than creating a second MRT entry.

### v5

Adds the latency/performance and low-compute predictive-control research layer (`SRC-104..SRC-123`):

```text
PX4 latency / uORB / work-queue scheduling
ROS 2 callback-chain response-time analysis
Linux PREEMPT_RT / cyclictest
acados / RTI-NMPC / TinyMPC / explicit MPC
MPPI / event-triggered robust MPC
TD-MPC2
PI-TCN quadrotor temporal dynamics
residual dynamics / sparse GP-MPC
SINDY-MPC / Koopman LMPC
quickest-change detection
real-time hierarchical quadrotor MPC
```

## Retrieval policy

Prefer sources in this order:

```text
exact local source/build + captured runtime evidence
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
uav_control
disturbance_observer
closed_loop_identification
micro_randomized_trials
small_sample_cluster_inference
world_model_ml
model_predictive_control
embedded_optimization
residual_dynamics
uncertainty_aware_control
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

Research references are design/audit inputs only. They do not authorize changes to frozen control/scientific semantics, treatment design, safety limits, acceptance criteria or production authority.