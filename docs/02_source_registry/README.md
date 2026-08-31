# Source Registry

This folder stores the source registry used to research, audit and extend the AURA–WISE–WM–AEGIS pipeline.

## Current registry identity

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7
sources=141
resolved=138
unresolved=3
LIBRARY_ARTIFACT=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7.md
LIBRARY_FILE_ID=file_000000001ea082118dccd3b98f68b166
```

Use:

- [`SOURCE_REGISTRY_CURRENT.md`](SOURCE_REGISTRY_CURRENT.md) for the current authority/pointer policy.
- [`CURRENT_REGISTRY_V7.md`](CURRENT_REGISTRY_V7.md) for the current v7 retrieval index and exact v6/v7 additions through `SRC-141`.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) for the historical post-v2 retrieval index through `SRC-123`.
- `library_snapshots/` for historical v1/v2 registry states.

The exact v7 Markdown artifact is held in the project/File Library. A v7 machine-readable JSON artifact/hash has not been promoted into this repository; do not invent one.

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

Added the latency/performance and low-compute predictive-control layer (`SRC-104..SRC-123`): PX4 latency/uORB, ROS/Linux scheduling, acados/RTI/TinyMPC/explicit MPC, MPPI/event-triggered MPC, TD-MPC2, temporal/residual/sparse/Koopman dynamics, quickest-change detection and real-time quadrotor MPC.

### v6

Added `SRC-124..SRC-127`: uncertainty-aware learned residual MPC, online dynamics learning for aerial robots, latency-aware quadrotor control and a unified safety-filter/runtime-assurance review.

### v7

Added `SRC-128..SRC-141` for:

```text
DDS/UAV latency measurement and synchronization
ROS 2 chain-aware scheduling and executor semantics
deterministic ROS 2 callback execution
online multivariate change detection
multi-stream quickest change detection
learned INDI / L1 augmentation
Neural-Fly and online residual adaptation
neural-ODE + disturbance-observer MPC
adaptive disturbance observation
quadrotor runtime assurance
```

The v7 dedup audit reports zero exact duplicate resolved locators, zero exact duplicate normalized titles and zero confirmed same-work duplicate IDs. Existing IDs were not renumbered.

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
