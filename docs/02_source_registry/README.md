# Source Registry

This folder stores the research registry used to audit and extend the AURA–WISE–WM–AEGIS pipeline.

## Current registry identity

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v9
UPDATED=2026-08-31
sources=156
resolved=153
unresolved=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

Use:

- [`CURRENT_REGISTRY_V9.md`](CURRENT_REGISTRY_V9.md) — active v9 retrieval/index view and exact `SRC-150..SRC-156` additions.
- [`SOURCE_REGISTRY_CURRENT.md`](SOURCE_REGISTRY_CURRENT.md) — stable registry pointer/policy.
- [`CURRENT_REGISTRY_V8.md`](CURRENT_REGISTRY_V8.md) — historical v8 index through `SRC-149`.
- [`CURRENT_REGISTRY_V7.md`](CURRENT_REGISTRY_V7.md) — historical v7 index through `SRC-141`.
- [`CURRENT_REGISTRY_V5.md`](CURRENT_REGISTRY_V5.md) — historical index through `SRC-123`.
- `library_snapshots/` — older registry states.

No source ID was deleted or renumbered.

## v9 additions

```text
SRC-150  timed runtime enforcement by delaying/suppressing events
SRC-151  Age of Information / freshness
SRC-152  deterministic Network Calculus
SRC-153  adaptive constrained experiment design
SRC-154  conformalized robust OOD MPC / 12D quadcopter benchmark
SRC-155  set-membership uncertainty learning
SRC-156  ellipsoidal approximation for set-membership uncertainty
```

These additions support four research tracks:

```text
Timed Automata / Runtime Enforcement
    → stronger prior-art baseline for CTEE

CTEE-F
    → CTEE + freshness + justified remaining-delay envelope

CIBES
    → post-Phase-0 information-efficient constrained excitation

World-Model uncertainty ladder
    → conformal / set-membership challengers after simpler baselines
```

## Adoption policy

Do not add an algorithm merely because it appears in the registry.

```text
candidate method
→ named project bottleneck / failure class
→ measurable KPI
→ simplest credible baseline
→ shadow / offline / preflight evidence
→ retain only if benefit is demonstrated
```

Current Phase-0 execution is unchanged:

```text
Q1 no-launch evidence
→ owner Option-B margin/policy freeze
→ Tarjan + CTEE/timed-enforcer benchmark
→ delayed-launch nonscience
→ fresh randomized G_action science
```

CTEE-F, CIBES and advanced uncertainty methods do not unblock Q1.

## Retrieval hierarchy

```text
exact local source/build + captured runtime evidence
→ explicitly frozen project contract
→ version-matched official source/docs
→ primary paper / canonical DOI / original repository
→ institutional repository
→ secondary synthesis
→ issue/forum for hypothesis generation only
```

For runtime implementation questions, exact pinned local evidence overrides generic upstream documentation when they diverge.

## Core categories

```text
px4_autopilot
runtime_timing
real_time_scheduling
causal_temporal_eligibility
runtime_verification
information_freshness
network_calculus
temporal_interval_validation
closed_loop_identification
experiment_design
world_model_ml
uncertainty_quantification
model_predictive_control
embedded_optimization
online_adaptation
runtime_assurance
network_optimization
```

Use [`../04_research/RESEARCH_USAGE_GUIDE.md`](../04_research/RESEARCH_USAGE_GUIDE.md) and [`../04_research/ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md`](../04_research/ALGORITHMIC_RESEARCH_EXPANSION_CTEE_F_CIBES_UNCERTAINTY_20260831.md) for source-to-problem guidance.

## Unresolved internal identities

```text
SRC-024
SRC-040
SRC-053
```

Do not silently substitute another internal document for these identities.

Research references are methodological/design inputs only. They do not authorize Q1, Option B, scientific execution, control-semantic changes, SEALED access or production authority.
