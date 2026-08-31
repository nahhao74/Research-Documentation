# AURA–WISE–WM–AEGIS Source Registry — Current Pointer

## Current registry identity

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9
UPDATED=2026-08-31
sources=156
resolved=153
unresolved=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

GitHub retrieval index:

- [`CURRENT_REGISTRY_V9.md`](CURRENT_REGISTRY_V9.md) — active v9 view through `SRC-156`.
- [`CURRENT_REGISTRY_V8.md`](CURRENT_REGISTRY_V8.md) — historical v8 view through `SRC-149`.
- [`CURRENT_REGISTRY_V7.md`](CURRENT_REGISTRY_V7.md) — historical v7 view through `SRC-141`.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) — historical view through `SRC-123`.
- `library_snapshots/` — historical v1/v2 snapshots.

## Registry lineage

```text
v1  87 sources; locators initially unresolved
v2  87 sources; 84 resolved / 3 unresolved
v3  SRC-088..SRC-094: pinned PX4/uXRCE/eProsima runtime references
v4  SRC-095..SRC-103: PX4/Gazebo + uncertainty/planning/residual research
v5  SRC-104..SRC-123: latency, scheduling, embedded predictive control, WM research
v6  SRC-124..SRC-127: residual MPC, online dynamics, latency-aware control, safety filter
v7  SRC-128..SRC-141: DDS/ROS2 latency, change detection, learned/adaptive disturbance, RTA
v8  SRC-142..SRC-149: Tarjan, max-plus, three-valued monitoring, sweep-line, network-flow
v9  SRC-150..SRC-156: timed enforcement, AoI, Network Calculus, adaptive experiment design,
    conformal OOD MPC, set-membership uncertainty and ellipsoidal approximation
```

No existing source ID was deleted or renumbered.

## v9 research direction

```text
Timed Runtime Enforcement
→ stronger prior-art baseline for CTEE

CTEE-F
→ causal eligibility + information freshness + justified remaining-delay envelope

CIBES
→ post-Phase-0 information-efficient constrained excitation

WM uncertainty ladder
→ conformal and set-membership challengers after simpler baselines
```

These methods are research hypotheses, not current runtime requirements.

## Current Phase-0 authority boundary

```text
Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN
FRESH_SCIENCE=BLOCKED
production_authority=false
```

The v9 additions do not change the exact next action: owner authorization for Q1 no-launch nonscientific runtime.

## Evidence-gated adoption rule

```text
candidate method
→ named project bottleneck
→ measurable KPI
→ simplest credible baseline
→ shadow/offline/preflight evidence
→ retain only if benefit is demonstrated
```

## Authority hierarchy

```text
exact local source/build + captured raw evidence
> frozen project contract
> version-matched official source/docs
> primary scholarly source / DOI
> institutional source
> secondary synthesis
> issue/forum hypothesis
```

Research references do not authorize changes to scientific/control semantics, treatment design, safety limits, acceptance criteria, SEALED or production authority.
