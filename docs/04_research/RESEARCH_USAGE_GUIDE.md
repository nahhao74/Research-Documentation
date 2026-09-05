# Research Usage Guide

## Purpose

Research exists to answer a measured project question, not to accumulate algorithms.

Current use is limited to:

1. support the scientific design and causal-data pipeline;
2. improve FAST on the current simulator through bounded shadow/replay comparison;
3. design the minimum useful World Model/WISE path after valid causal data exists;
4. evaluate later latency/freshness/uncertainty/safety mechanisms only after a measured need appears.

The active research plan is defined only by:

```text
FUTURE_IMPLEMENTATION_ROADMAP.md
```

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

For current project state and execution permission, research sources have no authority; use `CURRENT_STATUS.md` and the latest execution ladder.

## Current active research questions

### FAST

```text
Can the current simulated vehicle reject wind faster and more accurately than
current -d_hat + T1/C1 while PX4 firmware control semantics remain unchanged?
```

Research order:

```text
measure latency/source age and response phases
→ characterize current FAST limitation
→ smallest justified challenger
→ repeat-supported shadow/replay benchmark
→ retain only if materially better
```

Do not add a second PI/PID tracking loop as an active direction.

Do not introduce cross-airframe adaptation, online gain tuning or commercial fleet adaptation into the current simulator phase.

### World Model / WISE

Canonical structure remains:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1
```

Current research may improve interfaces, state representation, evaluation and serving design, but final action-conditioned training remains blocked until a complete valid randomized root passes causal-dataset admission.

### Closed-loop identification

Use the literature to preserve these conclusions:

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

Use for randomized/probing identification while the deployed controller remains active, persistent excitation, identifiability and treatment-response interpretation.

### `micro_randomized_trials` / `small_sample_cluster_inference`

Use for repeated randomized decisions, causal excursion logic, assigned-vs-realized treatment, session-level independence and few-cluster uncertainty.

### `world_model_ml`

Use for temporal/action-conditioned prediction, rollout, bounded planning and uncertainty-aware candidate scoring. These references do not authorize model capacity or training.

### `disturbance_observer` / `uav_control` / `stability_robust_control`

Use to evaluate FAST challengers or later control mechanisms after a measured failure class exists. Source presence alone is not a reason to implement INDI, MPC, DOB, adaptive control or any other method.

## Rejected / inactive directions

The following are **not active main-pipeline research directions** and should not be reintroduced unless the owner explicitly reopens them after new evidence:

```text
residual PI / 2-DOF PI augmentation as FAST primary direction
manual per-airframe PI tuning
online gain adaptation for the current simulator phase
cross-airframe adaptive controller / commercial commissioning design
online neural-network controller adaptation
```

Git history preserves prior discussion; active documents should not present these as current candidates.

## Research adoption rule

```text
measured bottleneck
→ exact local evidence
→ targeted literature review
→ smallest credible mechanism
→ bounded benchmark
→ no important latency/robustness regression
→ only then promotion review
```

If the measured system does not need a mechanism, do not add it for novelty.

## Registry navigation

Use:

```text
../02_source_registry/CURRENT_REGISTRY_V9.md
../02_source_registry/SOURCE_REGISTRY_CURRENT.md
```

Older registry versions are Git history, not active indexes.
