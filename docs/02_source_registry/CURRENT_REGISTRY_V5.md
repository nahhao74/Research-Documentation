# AURA–WISE–WM–AEGIS Source Registry v5 — Current Retrieval Index

Updated: 2026-08-29

```text
REGISTRY_VERSION=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v5
SOURCES=123
RESOLVED=120
UNRESOLVED=3
LOCAL_CANONICAL_JSON_SHA256=acc056ca6c475fb00c6173b54b7a4f779446c21588134bdb5ee23571fc1d16a2
LOCAL_CANONICAL_MD_SHA256=5d8221715ce87f61c089ed53e4c0666851551b4a1591c20d4fd2fc40d71df6b7
```

This file is the compact GitHub retrieval view of registry v5. Historical source entries `SRC-001..SRC-087` remain preserved in the archived v1/v2 registry snapshots. The entries below record the exact locators added after v2 and therefore make the current project research set directly searchable from GitHub without duplicating the large historical tables.

Authority rule:

```text
exact local source/build + captured runtime evidence
> version-matched official source/docs
> primary scholarly source / DOI
> institutional source
> secondary synthesis
> issue/forum hypothesis
```

Do not silently substitute a similar source for an unresolved identity. Research/design sources do not authorize changes to frozen scientific/control semantics.

## PX4 / uXRCE / Gazebo runtime sources

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-088 | uXRCE-DDS (PX4-ROS 2/DDS Bridge), PX4 v1.15 | OFFICIAL_PX4_DOCS | https://docs.px4.io/v1.15/en/middleware/uxrce_dds |
| SRC-089 | PX4 v1.15.4 `dds_topics.h.em` | OFFICIAL_PX4_REPO | https://github.com/PX4/PX4-Autopilot/blob/v1.15.4/src/modules/uxrce_dds_client/dds_topics.h.em |
| SRC-090 | eProsima Micro XRCE-DDS Client — Streams | OFFICIAL_EPROSIMA_DOCS | https://micro-xrce-dds.docs.eprosima.com/en/stable/client.html |
| SRC-091 | eProsima Micro XRCE-DDS Client API | OFFICIAL_EPROSIMA_DOCS | https://micro-xrce-dds.docs.eprosima.com/en/stable/client_api.html |
| SRC-092 | eProsima Micro XRCE-DDS Agent CLI / trace | OFFICIAL_EPROSIMA_DOCS | https://micro-xrce-dds.docs.eprosima.com/en/latest/agent.html |
| SRC-093 | PX4 issue #25873 — uXRCE ping blocking hypothesis | OFFICIAL_PX4_ISSUE | https://github.com/PX4/PX4-Autopilot/issues/25873 |
| SRC-094 | PX4 v1.15.4 `uxrce_dds_client.cpp` | OFFICIAL_PX4_REPO | https://github.com/PX4/PX4-Autopilot/blob/v1.15.4/src/modules/uxrce_dds_client/uxrce_dds_client.cpp |
| SRC-095 | Gazebo Models Repository, PX4 v1.15 | OFFICIAL_PX4_DOCS | https://docs.px4.io/v1.15/en/sim_gazebo_gz/gazebo_models |
| SRC-096 | Gazebo Simulation, PX4 v1.15 | OFFICIAL_PX4_DOCS | https://docs.px4.io/v1.15/en/sim_gazebo_gz/index |
| SRC-097 | PX4 v1.15.4 `GZBridge.cpp` | OFFICIAL_PX4_REPO | https://github.com/PX4/PX4-Autopilot/blob/v1.15.4/src/modules/simulation/gz_bridge/GZBridge.cpp |

## Uncertainty, closed-loop identification, residual learning and planning

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-098 | A Risk-Aware Adaptive Robust MPC with Learned Uncertainty Quantification | PRIMARY_PREPRINT | https://arxiv.org/abs/2507.11420 |
| SRC-099 | Chance-Constrained MPPI under State and Dynamic Object Prediction Uncertainty and the Evaluation of Collision Risk Calibration | PRIMARY_PREPRINT | https://arxiv.org/abs/2605.28330 |
| SRC-100 | Efficient Frequency Response Identification for Small Fixed-Wing UAS Using Closed-Loop Flight Data | PRIMARY_PUBLISHER_DOI | https://doi.org/10.2514/6.2023-0629 |
| SRC-101 | Learn Structure, Adapt on the Fly: Multi-Scale Residual Learning and Online Adaptation for Aerial Manipulators | PRIMARY_PREPRINT | https://arxiv.org/abs/2603.11638 |
| SRC-102 | Model-Based Diffusion Optimal Control for Multi-Robot Motion Planning | PRIMARY_CONFERENCE | https://roboticsconference.org/program/papers/41/ |
| SRC-103 | Unifying Object-Centric World Models and Diffusion Policy | PRIMARY_PREPRINT | https://arxiv.org/abs/2606.08775 |

## Latency / runtime / scheduling

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-104 | MC Filter Tuning & Control Latency, PX4 v1.15 | OFFICIAL_PX4_DOCS | https://docs.px4.io/v1.15/en/config_mc/filter_tuning |
| SRC-105 | uORB Messaging, PX4 v1.15 | OFFICIAL_PX4_DOCS | https://docs.px4.io/v1.15/en/middleware/uorb |
| SRC-106 | PX4 Architectural Overview, v1.15 | OFFICIAL_PX4_DOCS | https://docs.px4.io/v1.15/en/concept/architecture |
| SRC-107 | Response-Time Analysis of ROS 2 Processing Chains Under Reservation-Based Scheduling | PRIMARY_CONFERENCE_DOI | https://doi.org/10.4230/LIPIcs.ECRTS.2019.6 |
| SRC-108 | Real-time preemption | OFFICIAL_LINUX_KERNEL_DOCS | https://docs.kernel.org/core-api/real-time/ |
| SRC-109 | Cyclictest | OFFICIAL_LINUX_FOUNDATION_RT_DOCS | https://wiki.linuxfoundation.org/realtime/documentation/howto/tools/cyclictest/start |

## Low-latency / embedded predictive control

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-110 | acados: a modular open-source framework for fast embedded optimal control | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1007/s12532-021-00208-8 |
| SRC-111 | A Real-Time Iteration Scheme for Nonlinear Optimization in Optimal Feedback Control | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1137/S0363012902400713 |
| SRC-112 | TinyMPC: Model-Predictive Control on Resource-Constrained Microcontrollers | PRIMARY_CONFERENCE_DOI | https://doi.org/10.1109/ICRA57147.2024.10610987 |
| SRC-113 | The explicit solution of model predictive control via multiparametric quadratic programming | PRIMARY_CONFERENCE_DOI | https://doi.org/10.1109/ACC.2000.876624 |
| SRC-114 | Model Predictive Path Integral Control: From Theory to Parallel Computation | PRIMARY_PUBLISHER_DOI | https://doi.org/10.2514/1.G001921 |
| SRC-115 | Dynamic Event-Triggered Robust Model Predictive Control for Quadrotor Trajectory Tracking | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1002/rnc.70351 |
| SRC-123 | Real-time Model Predictive Control for Quadrotors | PRIMARY_PUBLISHER_DOI | https://doi.org/10.3182/20140824-6-ZA-1003.00203 |

## World model / temporal / residual / reduced-order dynamics

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-116 | TD-MPC2: Scalable, Robust World Models for Continuous Control | PRIMARY_CONFERENCE | https://proceedings.iclr.cc/paper_files/paper/2024/hash/cf73d57b6dcda32b293df7c2d5341f49-Abstract-Conference.html |
| SRC-117 | Physics-Inspired Temporal Learning of Quadrotor Dynamics for Accurate Model Predictive Trajectory Tracking | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1109/LRA.2022.3192609 |
| SRC-118 | Residual dynamics learning for trajectory tracking for multi-rotor aerial vehicles | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1038/s41598-024-51822-0 |
| SRC-119 | Sparse identification of nonlinear dynamics for model predictive control in the low-data limit | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1098/rspa.2018.0335 |
| SRC-120 | Real-Time Linear MPC for Quadrotors on SE(3): An Analytical Koopman-Based Realization | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1109/LRA.2025.3626234 |
| SRC-121 | Quickest Change Detection With Post-Change Density Estimation | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1109/TIT.2024.3418379 |
| SRC-122 | Efficient sparse GP-MPC with accurate mean and variance propagation applied for quadcopter flight control | PRIMARY_PREPRINT | https://arxiv.org/abs/2605.08903 |

## Existing MRT locator reconciliation

`SRC-075` remains one scholarly work rather than creating a duplicate entry. The current registry also recognizes these canonical alternate locators:

- PMC: https://pmc.ncbi.nlm.nih.gov/articles/PMC9276848/
- DOI: https://doi.org/10.1037/met0000283
- PubMed: https://pubmed.ncbi.nlm.nih.gov/35025583/

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

No similar internal Markdown should be silently substituted.

## Current research priorities for the pipeline

```text
P0  PX4/uORB/work-queue/filter/control end-to-end latency
P1  ROS/Linux scheduling jitter and callback-chain latency
P1  lightweight residual/temporal world models
P1  event-triggered / embedded MPC for WISE predictive refinement
P1  closed-loop identification and treatment SNR
P2  uncertainty-aware GP-MPC / sampling-based planning
```

Changes to AURA detector semantics, treatment design, control mathematics, safety limits or production authority still require the existing owner-review boundary.