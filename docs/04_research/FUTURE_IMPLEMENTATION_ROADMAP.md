# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap
### Moving-Mode UAV Detect & Response Pipeline

**Status:** Design / research roadmap  
**Scope:** Moving Mode only  
**Date:** 2026-08-29  
**Research reference:** project roadmap currently references `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7`.

> This roadmap does **not** authorize changes to the frozen randomized-identification, timing, control-authority, safety or production contract. GitHub's registry folder should only promote a v7 authoritative pointer when the exact v7 registry artifact/identity is archived; do not infer missing v7 counts or hashes from this roadmap.

## 1. Core architecture to preserve

```text
FAST PATH:
AURA -> FAST/T1/C1 -> PX4

PREDICTIVE PATH:
StateBank -> delay-aware state -> World Model -> WISE
          -> bounded candidate -> AEGIS -> PX4
```

Hard invariants:

- the FAST path must remain independently functional if WM/WISE is unavailable, stale, late, uncertain or rejected;
- PX4 inner loops remain authoritative;
- candidate authority stays bounded at the qualified acceleration-level insertion path;
- the World Model refines the closed-loop baseline rather than replacing AURA/FAST/PX4;
- `production_authority=false` until explicitly changed by a later deployment contract.

The scientific action target remains:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Post-treatment FAST/PX4 reactions are part of the realized closed-loop treatment effect.

## 2. Phase 0 — finish causal identification first

Before selecting a major predictive controller or large model:

```text
runtime/source/causal-contract closure
-> complete valid randomized pilot
-> P1/P2 output-space treatment SNR
-> carryover diagnostics
-> FAST-candidate interaction diagnostics
-> projection/saturation review
-> causal dataset acceptance
```

If P1/P2 are valid but weak, perform experiment-design/excitation review. Do not try to compensate for weak causal signal with a larger neural network.

### Current engineering gate before another pilot

The latest pilot stopped before science at `PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT`. Before another randomized root, the real runner/probe must pass the full integrated corridor:

```text
startup
-> source attestation
-> StateBank 7/7
-> bootstrap_only
-> first-block snapshot
-> C1 valid-offer frontier
-> pre-offer source/clock/provenance
-> intentional stop before FIRST_SCIENTIFIC_T_D
```

The scientific pilot must not be used as an integration test.

## 3. Phase 1 — end-to-end latency decomposition

Do this before changing major algorithms.

Instrument:

```text
physical/source event
-> PX4 publication
-> uXRCE serialization
-> Agent receive
-> ROS callback ready
-> ROS callback scheduled
-> AURA start/end
-> AEGIS start/end
-> DDS transmit
-> PX4 receive
-> accepted control cycle
-> actuator output
-> measured plant response
```

Decompose:

```text
T_total = T_sensor/source
        + T_transport
        + T_callback_wait
        + T_AURA_compute
        + T_AEGIS_compute
        + T_PX4
        + T_actuation
        + T_plant
```

Always separate callback waiting from callback computation.

Measure at minimum:

```text
p50, p95, p99, max, jitter, deadline misses,
source age, CPU utilization, context switches,
queue depth, overwrite/drop counts
```

Decision rule:

- if callback waiting dominates, optimize executor/scheduling/affinity/realtime policy/transport contention;
- if detector computation/decision delay dominates, then open AURA detector redesign.

## 4. Phase 2 — AURA detector shadow research

W20 remains the baseline until a challenger wins on the same causal traces.

Candidate bench:

```text
same causal trace
|- W20 baseline
|- CUSUM / quickest change detection
|- RFF sequential detector
`- multivariate detector
```

Compare:

```text
onset delay
direction-change delay
clear delay
false positives / false negatives
amplitude and direction error
phase delay
CPU/sample
memory
tail execution time
```

Select only from the latency/accuracy Pareto frontier. A faster detector is not automatically better.

## 5. Phase 3 — World Model v1 with a model ladder

Preferred decomposition:

```text
Y_future = F_B(X,h) + G_action(X,U,h)
```

Use minimum complexity first:

```text
M0 ZERO/constant
-> M1 linear/ridge
-> M2 local-linear/LPV
-> M3 SINDY sparse dynamics
-> M4 Koopman/lifted reduced-order model
-> M5 tiny residual MLP
-> M6 causal temporal model / TCN
```

Model selection must include:

```text
future-state prediction
incremental-action prediction
p50/p95/p99 inference latency
memory
sample efficiency
interpretability
uncertainty calibration
closed-loop planning utility
```

A more complex model must earn its additional compute.

## 6. Delay-aware state should be promoted early

`T_D` and `T_A` are distinct:

```text
T_D = causal decision frontier
T_A = actual accepted-action frontier
```

Online code cannot use future actual `T_A`. Instead estimate application delay at decision time:

```text
X(T_D), history, tau_apply_hat
        -> X_hat(T_A)
        -> World Model + candidate U
        -> Y_hat(T_A + h)
```

Recommended ablation:

```text
A. ignore application delay
B. fixed mean delay
C. context-conditioned delay predictor
```

This should be evaluated before attributing model error to lack of neural capacity.

## 7. StateBank history ablation

Scientific retention history and serving-model history are not the same thing.

Test:

```text
H0, H20, H40, H80, H160, H320, H1000
```

Keep longer history only if it materially improves prediction, `G_action`, generalization or uncertainty calibration enough to justify latency/memory/model complexity.

## 8. Uncertainty-aware prediction

Eventually prefer calibrated predictive uncertainty rather than point estimates alone:

```text
(Y_hat, sigma_Y)
```

or predictive sets.

WISE may then score:

```text
tracking
+ control effort
+ smoothness
+ uncertainty
+ constraint margin
```

Uncertainty must be calibrated before being used as a safety proxy.

## 9. Phase 4 — WISE solver ladder

Treat WISE as a planning/scoring role, not a commitment to heavy NMPC.

Recommended progression:

```text
WISE-0 bounded candidate enumeration/library search
-> WISE-1 small convex QP / TinyMPC
-> WISE-2 linear / Koopman MPC
-> WISE-3 acados + RTI
-> WISE-4 full NMPC / MPPI only if earlier methods are insufficient
```

WISE-0 is a serious baseline. For a small bounded acceleration-plan space, finite candidate rollout may provide near-MPC performance with much lower and more deterministic latency.

## 10. Event-triggered WISE

StateBank may update at high rate without forcing a high-rate optimizer.

Preferred pattern:

```text
compute plan
reuse
reuse
reuse
recompute on event
```

Potential triggers:

- new disturbance event;
- reference change;
- prediction residual growth;
- uncertainty increase;
- plan horizon expiry;
- cross-track error growth;
- constraint-margin reduction;
- model validity loss;
- stale/freshness boundary.

Goal: reduce CPU load, tail latency and contention while protecting the FAST path.

## 11. Phase 5 — bounded online adaptation

Do not default to full online neural-network retraining.

Preferred pattern:

```text
frozen representation/base model
+
small adaptive parameter vector alpha_t
+
confidence / validity / rollback state
```

A useful bandwidth decomposition is:

```text
fast unpredictable disturbance -> AURA / FAST
predictable closed-loop dynamics -> F_B + G_action
slow model mismatch -> small adaptive residual R_slow
```

This keeps the World Model from being forced to replace AURA.

## 12. Safety envelope before live adaptation authority

Before online adaptation receives meaningful live authority, define the AEGIS safety envelope even if the formal filter is implemented later.

Specify:

```text
allowed action envelope
maximum authority
rate/change bounds
freshness requirements
constraint reserve
confidence failure behavior
rollback condition
model-invalid fallback
```

A future projection may solve:

```text
min ||U - U_WISE||^2
subject to qualified safety constraints
```

The safety layer must preserve PX4 authority, remain bounded in runtime, fail closed and expose intervention diagnostics.

## 13. Full integrated corridor is a permanent engineering gate

For every new live feature:

```text
unit/deterministic tests
-> offline/replay
-> component qualification
-> FULL INTEGRATED CORRIDOR
-> scientific/performance experiment
```

Example for future WM/WISE integration:

```text
StateBank
-> WM inference
-> WISE scoring/planning
-> AEGIS parsing/freshness/bounds
-> PX4 acceptance boundary
-> intentional stop before live authority
```

Passing each module separately is not evidence that the integrated chain is correct.

## 14. Priority by expected ROI

### Very high

1. valid causal `G_action` identification;
2. true end-to-end latency decomposition;
3. `T_D -> T_A` state compensation;
4. lightweight structured residual World Model.

### High

5. WISE-0 candidate enumeration;
6. event-triggered WISE;
7. AURA detector shadow bake-off;
8. uncertainty calibration.

### Medium / after proof

9. TinyMPC / Koopman MPC;
10. low-dimensional adaptation;
11. formal AEGIS projection.

### Only if justified

12. acados / RTI NMPC;
13. MPPI;
14. large neural World Models.

## 15. Three generations

### vNext-1 — scientific system

Goal: establish whether incremental `G_action` is causally identifiable under the active controller baseline.

### vNext-2 — performance system

Goal: improve real closed-loop tracking without making first response depend on WM/WISE.

Likely ingredients:

```text
latency optimization
delay-aware state
lightweight F_B + G_action
history ablation
uncertainty
WISE candidate search
event-triggered planning
TinyMPC / Koopman only if useful
```

### vNext-3 — adaptive / safety-oriented system

Goal: handle nonstationarity and move toward robust real-world deployment.

Likely ingredients:

```text
low-dimensional online adaptation
adaptive disturbance estimation
formal safety filter/runtime assurance
hardware stress and tail-latency validation
rollback/degradation testing
```

## 16. Recommended experiment sequence after current scientific closure

Recommended order:

```text
E1  end-to-end latency baseline
E2  T_D -> T_A delay/state baseline
E3  AURA detector shadow bake-off
E4  F_B model ladder
E5  G_action model ladder
E6  history ablation
E7  uncertainty calibration
E8  WISE-0 candidate enumeration
E9  event-triggered WISE
E10 TinyMPC / Koopman / RTI benchmark
E10.5 AEGIS safety-envelope specification
E11 low-dimensional online adaptation
E12 formal AEGIS safety filter
```

Each experiment requires a frozen baseline and explicit go/no-go criteria.

## 17. Research implementation policy

Use evidence hierarchy:

```text
1. exact local source/build + raw runtime evidence
2. current internal scientific/architecture contract
3. version-matched official PX4/eProsima/ROS source/docs
4. primary scholarly literature
5. secondary technical sources
6. inference/hypothesis
```

A paper proposes a hypothesis, not authority:

```text
paper
-> shadow implementation
-> replay/offline ablation
-> non-scientific qualification
-> integrated corridor
-> semantic/control review when required
-> controlled integration
```

Open additional literature search only when a concrete evidence gap appears: unresolved temporal modeling, poor uncertainty calibration, unexplained latency, unstable adaptation or excessive WISE planning cost.

## 18. Primary design principle

```text
minimum complexity
that produces a demonstrable
closed-loop benefit
```

Preferred decision order:

```text
measure
-> identify bottleneck
-> introduce smallest credible improvement
-> shadow/replay
-> ablate
-> validate causality + latency
-> integrate only when benefit is real
```

A strong final system may therefore be relatively compact:

```text
AURA challenger
+ FAST/T1/C1
+ delay-aware state
+ SINDY/Koopman/small residual model
+ finite candidate rollout
+ event-triggered replanning
+ bounded AEGIS safety layer
+ PX4 authoritative inner loops
```

The value of the system should come from architecture, causality, timing and evidence—not model size alone.
