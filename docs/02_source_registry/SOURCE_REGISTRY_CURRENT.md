# AURA–WISE–WM–AEGIS Source Registry — Current Pointer

## Current registry identity

The current project research registry is:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7
sources=141
resolved=138
unresolved=3
LIBRARY_ARTIFACT=AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7.md
LIBRARY_FILE_ID=file_000000001ea082118dccd3b98f68b166
```

GitHub retrieval index:

- [`CURRENT_REGISTRY_V7.md`](CURRENT_REGISTRY_V7.md) — current v7 retrieval view, including exact v6/v7 additions through `SRC-141`.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) — historical retrieval index covering post-v2 additions through `SRC-123`.
- `library_snapshots/` — historical v1/v2 snapshots.

The exact v7 Markdown artifact exists in the project/File Library and is the authority for v7 registry content. A v7 machine-readable JSON artifact/hash has not been promoted into this GitHub repository, so no JSON/hash identity is inferred here.

## Registry lineage

```text
v1  87 sources; locators initially unresolved
v2  87 sources; 84 resolved / 3 unresolved
v3  SRC-088..SRC-094: pinned PX4/uXRCE/eProsima runtime references
v4  SRC-095..SRC-103: PX4/Gazebo + uncertainty/planning/residual research
v5  SRC-104..SRC-123: latency, scheduling, embedded predictive control, World Model research
v6  SRC-124..SRC-127: learned residual MPC, online dynamics, latency-aware control, safety filter
v7  SRC-128..SRC-141: DDS/ROS 2 latency, change detection, learned/adaptive disturbance modeling, runtime assurance
```

No existing source ID was deleted or renumbered in v7.

## Current research directions

The v7 set informs work on:

```text
closed-loop randomized identification
PX4/uXRCE/ROS end-to-end latency
executor and scheduling determinism
AURA change-detection challengers
learned/adaptive disturbance estimation
structured residual / temporal World Models
T_D -> T_A delay-aware prediction
uncertainty-aware predictive control
bounded candidate enumeration
TinyMPC / RTI / event-triggered WISE
online low-dimensional adaptation
AEGIS safety filters / runtime assurance
```

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

No similar file should be silently substituted for these unresolved identities.

## Retrieval / authority rule

Prefer:

```text
exact local source/build identity
+ captured raw telemetry/counters
> version-matched official source/docs
> primary scholarly source / DOI
> institutional source
> secondary synthesis
> issue/forum hypothesis
```

Issue/forum evidence is hypothesis-generation only unless exact local source/timing evidence matches. Project-internal Markdown is context/evidence and becomes normative only when explicitly frozen/promoted by project governance.

Research references do not authorize changes to frozen scientific/control semantics, safety limits, treatment design or production authority.
