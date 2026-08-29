# Research Usage Guide

## Purpose

The research folder and source registry exist to answer two kinds of questions:

1. **Is the scientific design defensible?**
2. **How should the control / identification / world-model architecture evolve when evidence exposes a new issue?**

They are references for reasoning and audit, not substitutes for exact local runtime evidence.

## Authority hierarchy

When sources disagree, use this order:

```text
1. exact local source/build identity + raw runtime evidence
2. official version-matched PX4 / eProsima documentation or source
3. original scholarly paper / DOI / institutional repository
4. secondary technical synthesis
5. issue/forum/blog material for hypothesis generation
```

A GitHub issue or forum post may suggest a mechanism, but it does not prove that mechanism occurred in the pinned runtime.

## Source categories and what they are for

### `px4_autopilot`

Use for:

- actual controller topology;
- acceleration setpoint semantics;
- PositionControl behavior;
- allocator behavior;
- parameters and version-specific interfaces;
- message/reset semantics.

Prefer exact v1.15/v1.15.4 source or pinned local SHA over PX4 `main` when implementation details matter.

### `closed_loop_identification`

Use for:

- whether identification is valid with feedback active;
- input design under closed-loop operation;
- persistent excitation / identifiability questions;
- experiment design where the deployed controller remains part of the plant-controller system.

Current project interpretation: the target is the incremental response of the **controlled** system, not an open-loop plant recovered by disabling FAST/PX4.

### `micro_randomized_trials`

Use for:

- randomized repeated treatment decisions;
- causal excursion effects;
- treatment moderation by pre-treatment context;
- distinction between assigned treatment and realized/available treatment;
- planning sample size / proximal outcomes.

The UAV pilot is not a literal mHealth trial, but the repeated randomized intervention logic is methodologically useful.

### `small_sample_cluster_inference`

Use for:

- few-session uncertainty;
- cluster bootstrap design;
- avoiding pseudo-replication from many blocks within only a few sessions;
- interpreting weak evidence with small independent-cluster count.

Current pilot has only four sessions per context, so large-cluster asymptotics should not be overclaimed.

### `world_model_ml`

Use for:

- temporal world-action models;
- action-conditioned rollout structure;
- receding-horizon prediction;
- uncertainty-aware candidate scoring;
- efficient serving architectures.

Key conceptual references include FlowPilot, WorldFly and DroneDreamer.

These references inform future design; they do not override frozen project causal/timing contracts.

### `disturbance_observer`

Use for:

- disturbance estimation architecture;
- observer bandwidth / separation from feedback control;
- interaction of disturbance estimates with feedforward or feedback paths.

Useful when reviewing AURA or future AEGIS disturbance-handling changes.

### `uav_control` / `stability_robust_control`

Use for:

- MPC/NMPC;
- L1 adaptive control;
- INDI / incremental dynamic inversion;
- robust/stability concepts;
- gain scheduling and constrained control.

These sources are design references, not permission to replace the already qualified FAST/T1/C1 path without an explicit semantic redesign.

## Current closed-loop identification conclusions

### Feedback remaining active is not automatically a confounder

The current estimand explicitly conditions the treatment on the active baseline:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

Randomization identifies the incremental effect under that baseline.

### Post-treatment FAST response is downstream treatment response

If the candidate changes state and FAST reacts afterward, that reaction is causally downstream of treatment. Removing it as a covariate would change the effect being estimated.

Pre-treatment FAST/AURA/controller state may be used as context/moderation because it exists before assignment.

### H1000 is not proof of physical washout

H1000 defines candidate-history/refractory semantics. Physical carryover still requires empirical diagnostics using pre-treatment state and randomized outcomes.

### Treatment SNR is an output-space question

Do not estimate identifiability by comparing `0.008` or `0.012 m/s^2` candidate amplitude directly to the magnitude of the baseline controller command.

Use randomized outcome contrasts, ZERO variability, session variation, carryover, FAST interaction and constraint activity.

## When to research before changing code

Research is particularly useful when a proposed change would alter:

- scientific estimand;
- randomization / treatment definition;
- causal timing semantics;
- controller authority or safety boundary;
- World Model representation / planner architecture;
- statistical inference with few sessions;
- treatment amplitude or duration.

Pure implementation failures should first be grounded in local source/runtime evidence. Do not launch broad literature searches to explain a mechanical call-site bug.

## Registry navigation

The full source lists are under:

```text
../02_source_registry/
```

Historical registry snapshots preserve how locator verification evolved. Use the most recent authoritative registry for lookup, while older versions remain useful for provenance.

The registry records source authority classes so that secondary sources are not silently treated as primary evidence.