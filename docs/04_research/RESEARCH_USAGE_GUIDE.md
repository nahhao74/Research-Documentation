# Research Usage Guide

## Purpose

Research exists to answer measured project questions, not to accumulate algorithms.

Current use is limited to:

1. supporting the causal/scientific design;
2. improving FAST on the current simulator through bounded shadow/replay comparison;
3. designing the minimum useful World Model/WISE path after valid causal data exists;
4. evaluating later timing/freshness/uncertainty/safety mechanisms only when a measured need appears.

The active research plan is defined only by:

```text
FUTURE_IMPLEMENTATION_ROADMAP.md
```

Anything not present there is background reference only.

## Authority hierarchy

For technical claims:

```text
1. exact local source/build + raw runtime evidence
2. frozen project contract
3. version-matched PX4/eProsima source or docs
4. original scholarly paper / DOI / original repository
5. institutional repository
6. secondary synthesis
7. issue/forum/blog hypothesis
```

For current project state and execution permission, use `CURRENT_STATUS.md` and the latest execution ladder. Research sources have no execution authority.

## Current active research questions

### FAST

```text
Can the current simulated vehicle reject wind faster and more accurately than
current -d_hat + T1/C1 while PX4 firmware control semantics remain unchanged?
```

Research order:

```text
measure latency/source age and response phases
→ characterize the actual limitation
→ test the smallest justified challenger
→ repeat-supported shadow/replay benchmark
→ retain only if materially better
```

No replacement algorithm is selected before the measured failure class is known.

### World Model / WISE

Canonical structure:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1
```

Current research may improve interfaces, state representation, evaluation and serving design. Final action-conditioned training remains blocked until a complete valid randomized root passes causal-dataset admission.

### Closed-loop identification

Preserve these conclusions:

```text
feedback remaining active is compatible with the current estimand
post-treatment FAST/PX4 response is part of realized treatment
H1000 is a candidate-history contract, not proof of physical washout
treatment SNR is an output-space question
```

## Source categories

### `px4_autopilot`

Use for actual controller topology, acceleration-setpoint semantics, PositionControl behavior, allocation, messages and version-specific implementation.

Prefer pinned release/source over generic PX4 `main` when exact behavior matters.

### `closed_loop_identification`

Use for randomized/probing identification while deployed feedback remains active, persistent excitation, identifiability and treatment-response interpretation.

### `micro_randomized_trials` / `small_sample_cluster_inference`

Use for repeated randomized decisions, causal excursion logic, assigned-vs-realized treatment, session-level independence and few-cluster uncertainty.

### `world_model_ml`

Use for temporal/action-conditioned prediction, rollout, bounded planning and uncertainty-aware candidate scoring. These references do not authorize model capacity or training.

### `disturbance_observer` / `uav_control` / `stability_robust_control`

Use only after a measured FAST/control limitation exists. Source presence alone is not an implementation reason.

## Research adoption rule

```text
measured bottleneck
→ exact local evidence
→ targeted literature review
→ smallest credible mechanism
→ bounded benchmark
→ no important latency/robustness regression
→ promotion review only if benefit is repeat-supported
```

Do not add a mechanism for novelty or because it appears in the registry.

## Historical/rejected research

Superseded or rejected directions are intentionally absent from active research documents. Git history preserves provenance.

An AI must not reconstruct an old direction from Git history or source-registry entries unless the owner explicitly reopens it.

## Registry navigation

Use:

```text
../02_source_registry/CURRENT_REGISTRY_V9.md
../02_source_registry/SOURCE_REGISTRY_CURRENT.md
```

The registry is a methodological index, not an active-algorithm list.
