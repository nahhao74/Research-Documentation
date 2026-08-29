# World Model and WISE Design

## 1. Role in the vNext pipeline

The World Model / WISE layer is a predictive refinement path layered on top of the already active PX4 + AURA + FAST/T1/C1 controller. It is not a replacement for immediate disturbance rejection and must not become a prerequisite for first response.

The fast path handles **now**. The predictive path estimates **what happens next** and selects bounded refinements when confidence and validity are sufficient.

## 2. Current model decomposition

The target decomposition is:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

### F_nominal

`F_nominal(X,h)` models future behavior under the existing active baseline:

```text
B = PX4 + AURA + FAST/T1/C1
```

It represents the controlled system, not an isolated open-loop airframe.

### G_action

`G_action(X,U_plan,h)` models the incremental effect of a candidate action plan relative to ZERO candidate under that same baseline:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

This decomposition is the reason action-conditioned data must be generated with the baseline left active.

## 3. Why randomized action data is required

Ordinary baseline flight data can train useful controlled dynamics, but it does not by itself isolate the incremental causal effect of a small candidate that is followed by feedback-controller reactions.

The action-conditioned branch therefore uses predeclared randomized ZERO/P1/P2 interventions in a continuous canonical runtime. Randomization supplies the experimental variation needed to estimate an incremental closed-loop treatment effect.

The current pilot is a mechanism/SNR/carryover calibration experiment, not a final efficacy trial.

## 4. State representation

World Model input state `X` should be assembled from StateBank using causal records available at `T_D` only.

Relevant state classes include:

- local position/velocity and reference error;
- attitude / motion state;
- AURA disturbance state and validity;
- current controller/FAST/T1/C1 context;
- recent candidate history;
- actuator/control state required for context;
- source/session/reset identity;
- native timing and clock-alignment provenance.

The exact training representation may evolve, but future samples or post-assignment variables must not leak into the decision-state representation.

## 5. Action representation

A point action is often insufficient for the actual mechanism because the candidate is held for multiple accepted controller cycles.

The preferred serving representation is therefore temporal:

```text
U_plan = bounded short action sequence / temporal plan
```

The current frozen pilot uses simple fixed-duration plans to identify whether the system contains enough signal before moving to richer candidate sequences.

## 6. Stage A / Stage B temporal structure

Current causal logic distinguishes:

```text
T_D = decision frontier
T_A = actual accepted action frontier
```

A useful two-stage formulation is:

### Stage A

Predict the pre-action accepted-frontier state from causal information available at `T_D` and a causal application-latency estimate.

```text
X(T_D) -> X_hat(T_A)
```

The actual future `X(T_A)` is training/audit supervision, not online decision input.

### Stage B

Predict future state/outcome from the accepted-frontier state plus candidate plan:

```text
X_hat(T_A), U_plan -> Y(T_A+h)
```

This structure avoids silently giving the online model access to the true future state at action acceptance.

## 7. Existing corpus interpretation

Earlier action-conditioned datasets remain useful mainly for baseline dynamics/state representation where their causal contracts are satisfied. Historical target bugs or aliased action representations must not be treated as definitive evidence that candidate action has no physical effect.

A model failing to improve DEV can mean several different things:

- treatment signal too small relative to noise;
- representation does not capture the action effect;
- target construction is weak;
- training procedure is underpowered;
- true incremental effect is negligible.

Only a valid randomized identification experiment can distinguish these possibilities with appropriate evidence.

## 8. WISE planning role

WISE consumes state and model predictions to choose a bounded future correction. Conceptually:

```text
1. read causal StateBank state
2. generate candidate U_plan set
3. roll out F_nominal + G_action
4. score candidate trajectories
5. reject stale/uncertain/infeasible plans
6. select bounded acceptable plan
7. hand plan to AEGIS
```

Potential scoring terms include:

```text
trajectory / cross-track error
future velocity error
control effort
uncertainty
constraint margin
candidate switching / smoothness
```

WISE is model-predictive in function. It does not need to solve a large nonlinear optimization online if a smaller enumerated/library/search policy meets latency and accuracy requirements.

## 9. Serving safety and fallback

WM/WISE output must carry:

- plan identity;
- state/source frontier used;
- validity/freshness;
- uncertainty/confidence;
- applicable horizon;
- candidate bounds.

If any serving contract fails:

```text
candidate -> ZERO / unavailable
baseline -> continues
```

The model is never allowed to hold the fast controller hostage to a stale prediction.

## 10. Training gate

Action-conditioned model training should begin only after a complete valid randomized pilot has established whether P1/P2 produce usable signal at the intended horizons.

Required reasoning sequence:

```text
complete valid randomized root
-> E0/E1/session-cluster diagnostics
-> treatment SNR
-> carryover / FAST-candidate interaction
-> constraint activity
-> causal-identification review
-> dataset acceptance
-> model training
```

A valid but weak signal leads to experiment-design review, not automatic amplitude escalation.

## 11. Evaluation philosophy

Future model evaluation should separate:

1. **prediction quality** — does the model forecast the controlled state accurately?
2. **incremental action quality** — does `G_action` predict randomized action response beyond ZERO?
3. **planning utility** — does using the prediction improve closed-loop trajectory behavior under bounded authority?
4. **latency/deadline behavior** — can serving meet the required realtime budget without blocking FAST?
5. **uncertainty/fail-closed behavior** — does the system reject unsupported plans safely?

These layers should not be collapsed into one scalar metric.

## 12. Research references

For literature supporting this direction, use the source registry categories:

```text
world_model_ml
closed_loop_identification
micro_randomized_trials
small_sample_cluster_inference
uav_control
disturbance_observer
stability_robust_control
```

FlowPilot, WorldFly and DroneDreamer are useful world-action/world-model references. Closed-loop identification and micro-randomized-trial sources govern the experiment logic. PX4 official source/docs govern actual runtime control semantics.