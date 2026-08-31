# AURA–WISE–WM–AEGIS Source Registry v7 — Current Retrieval Index

Updated: 2026-08-29

```text
REGISTRY_VERSION=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7
SOURCES=141
RESOLVED=138
UNRESOLVED=3
LIBRARY_ARTIFACT=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7.md
LIBRARY_FILE_ID=file_000000001ea082118dccd3b98f68b166
```

This is the current GitHub retrieval view for registry v7. The exact project/File-Library Markdown artifact is the authority for the v7 registry content. A v7 machine-readable JSON artifact/hash has not been promoted here, so this file does not invent one.

Historical coverage remains preserved as follows:

```text
SRC-001..SRC-087   archived v1/v2 Library snapshots
SRC-088..SRC-123   CURRENT_REGISTRY_V5.md retrieval index
SRC-124..SRC-127   v6 targeted additions, reproduced below
SRC-128..SRC-141   v7 targeted additions, reproduced below
```

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

## v7 dedup audit

The project-local v7 audit reports:

```text
Exact duplicate resolved locators = 0
Exact duplicate normalized titles = 0
Confirmed same-work duplicate IDs = 0
```

`SRC-016` and `SRC-087` are retained as a near-redundant PX4 documentation pair because they differ by documentation version/language. `SRC-020`/`027`/`069`, `SRC-028`/`034`/`086`, and `SRC-041`/`042`/`044` are distinct resources rather than duplicate source IDs.

## v6 targeted additions retained in v7

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-124 | Uncertainty-aware model predictive control with learning-based residual dynamics compensation for agile quadrotor trajectory tracking | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1016/j.cja.2026.104424 |
| SRC-125 | Online Dynamics Learning for Predictive Control with an Application to Aerial Robots | PRIMARY_CONFERENCE | https://proceedings.mlr.press/v205/jiahao23a.html |
| SRC-126 | Latency-aware attitude control of underactuated quadrotor UAVs using barrier Lyapunov and fuzzy Padé approximation | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1038/s41598-026-45781-x |
| SRC-127 | The Safety Filter: A Unified View of Safety-Critical Control in Autonomous Systems | AUTHORITATIVE_REVIEW | https://doi.org/10.1146/annurev-control-071723-102940 |

These additions support post-identification algorithm, latency and runtime-assurance research; they do not change the frozen scientific contract.

## v7 — latency / ROS 2 / DDS

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-128 | Latency Reduction and Packet Synchronization in Low-Resource Devices Connected by DDS Networks in Autonomous UAVs | PRIMARY_PUBLISHER_DOI | https://doi.org/10.3390/s23229269 |
| SRC-129 | PiCAS: New Design of Priority-Driven Chain-Aware Scheduling for ROS2 | PRIMARY_CONFERENCE_DOI | https://doi.org/10.1109/RTAS52030.2021.00028 |
| SRC-130 | Executors — ROS 2 Documentation (Executor, WaitSet, Callback Group Events Executor, scheduling semantics) | OFFICIAL_ROS2_DOCS | https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Executors.html |
| SRC-141 | Deterministic Execution of ROS 2 Applications via Lingua Franca | PRIMARY_PREPRINT | https://arxiv.org/abs/2606.09203 |

Applicability guardrail: `SRC-130` does not imply that the current project nodes use `rclcpp`, any specific executor, or any particular ROS 2 distribution. Verify exact local implementation first.

## v7 — AURA / online change detection

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-131 | Optimal Online Change Detection via Random Fourier Features | PRIMARY_CONFERENCE | https://papers.nips.cc/paper_files/paper/2025/hash/0e3b37f830edb6972bb1206324cb566f-Abstract-Conference.html |
| SRC-132 | Online multivariate changepoint detection: leveraging links with computational geometry | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1093/jrsssb/qkaf046 |
| SRC-133 | Multi-Stream Quickest Change Detection: Foundations and Recent Advances | PRIMARY_PUBLISHER_DOI | https://doi.org/10.3390/e28050566 |

## v7 — learned/adaptive disturbance and dynamics modeling

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-134 | Learned Incremental Nonlinear Dynamic Inversion for Quadrotors with and without Slung Payloads | PRIMARY_CONFERENCE | https://proceedings.mlr.press/v331/cobo-briesewitz26a.html |
| SRC-135 | Adaptive Augmentation of Incremental Nonlinear Dynamic Inversion for Microquadrotor Aerobatic Flight Control | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1109/TAES.2026.3663140 |
| SRC-136 | Neural-Fly enables rapid learning for agile flight in strong winds | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1126/scirobotics.abm6597 |
| SRC-137 | Learning Agile Quadrotor Flight in the Real World | PRIMARY_PREPRINT | https://arxiv.org/abs/2602.10111 |
| SRC-138 | KNODEOB-MPC: A Knowledge-Based Neural ODE With Online Observer MPC Framework for Quadrotor | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1002/rnc.70444 |
| SRC-139 | Gain-Scaled compensation function observer for enhanced adaptive backstepping control of QUAV attitude | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1016/j.conengprac.2026.106968 |

## v7 — runtime assurance

| ID | Source | Authority | Exact locator |
|---|---|---|---|
| SRC-140 | A Run Time Assurance Approach for Safe Control of a Quadrotor | PRIMARY_PUBLISHER_DOI | https://doi.org/10.1115/1.4071137 |

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

No similar internal Markdown file should be silently substituted.

## Current v7 research coverage

```text
closed-loop randomized identification
PX4/uXRCE/ROS end-to-end latency
ROS 2 callback-chain scheduling and determinism
AURA online/change-detection challengers
learned/adaptive disturbance estimation
structured residual / temporal World Models
T_D -> T_A delay-aware prediction
uncertainty-aware predictive control
bounded / embedded / event-triggered WISE
online low-dimensional adaptation
AEGIS safety filters / runtime assurance
```

For any implementation change, exact local code/build/runtime evidence remains authoritative over generic upstream documentation or literature.