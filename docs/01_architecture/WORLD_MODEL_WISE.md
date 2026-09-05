# World Model and WISE Design

## 1. Role in the vNext pipeline

The World Model / WISE layer is a **predictive refinement path** layered on top of the active PX4 + AURA + FAST/T1/C1 controller. It is not a replacement for immediate disturbance rejection and must never become a prerequisite for first response.

```text
FAST path       = handle disturbance now
World Model     = predict what happens next
WISE            = choose bounded predictive refinement
AEGIS           = enforce bounded execution
PX4             = authoritative flight control
```

## 2. Current model decomposition

The canonical target is:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

### F_nominal

`F_nominal(X,h)` models future behavior under the existing active baseline:

```text
B = PX4 + AURA + FAST/T1/C1
```

It represents the controlled closed-loop system, not an isolated open-loop airframe.

### G_action

`G_action(X,U_plan,h)` models the incremental effect of a bounded candidate action relative to ZERO candidate under the **same** baseline:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

This is why action-conditioned data must be acquired with the frozen baseline active.

## 3. Baseline dependency

`G_action` is conditional on the active controller baseline. A material FAST change therefore changes the controlled system being identified.

Conceptually:

```text
B0 = PX4 + AURA + FAST0/T1/C1
B1 = PX4 + AURA + FAST1/T1/C1
```

In general, do not assume:

```text
G_action^(B0) == G_action^(B1)
```

without evidence.

Consequences:

1. The current Phase-0 randomized campaign must keep current FAST/T1/C1 semantics frozen.
2. Separate FAST shadow research may proceed without changing the scientific baseline.
3. If a future FAST challenger is materially promoted, the production action-conditioned model must be re-evaluated under the newly frozen baseline.
4. Current Phase-0 remains valuable even if FAST later changes because it qualifies the causal acquisition/admission machinery; it does not automatically make a model trained on one baseline valid for another.

## 4. Why randomized action data is required

Ordinary baseline flight data can train useful controlled dynamics, but it does not by itself isolate the incremental causal effect of a small candidate followed by PX4/FAST closed-loop reactions.

The action-conditioned branch therefore uses predeclared randomized ZERO/P1/P2 interventions in a continuous canonical runtime. Randomization supplies the variation needed to estimate the incremental closed-loop treatment effect.

The current pilot is a mechanism/SNR/carryover calibration experiment, not a final efficacy trial.

## 5. State representation

World Model input state `X` is assembled from StateBank using causal records available at `T_D` only.

Relevant state classes include:

```text
local position / velocity
reference and tracking error
attitude / motion state
AURA disturbance state and validity
current FAST/T1/C1 context
recent candidate history
controller / actuator context
source/session/reset identity
native timing and clock-alignment provenance
```

The exact training representation may evolve, but future samples or post-assignment variables must not leak into the decision-state representation.

No online adaptive vehicle-context mechanism is part of the current canonical WM structure.

## 6. Action representation

A point action can be insufficient because the candidate is held over multiple accepted controller cycles.

Preferred serving representation:

```text
U_plan = bounded short temporal action sequence
```

The current frozen pilot uses simple fixed-duration plans to determine whether action-response signal exists before richer candidate sequences are considered.

## 7. Stage A / Stage B temporal structure

Current causal logic distinguishes:

```text
T_D = decision frontier
T_A = actual accepted-action frontier
```

### Stage A

Predict the accepted-frontier state from information available at `T_D` plus a causal application-latency estimate:

```text
X(T_D) -> X_hat(T_A)
```

The actual future `X(T_A)` is training/audit supervision, not online decision input.

### Stage B

Predict future state/outcome from the accepted-frontier state plus candidate plan:

```text
X_hat(T_A), U_plan -> Y(T_A+h)
```

This structure avoids giving the online model access to the true future state at action acceptance.

## 8. Existing corpus interpretation

Earlier action-conditioned datasets remain useful only where their causal contracts are satisfied. Historical target bugs, aliased action representations or infrastructure-invalid roots must not be treated as evidence that candidate action has no physical effect.

A model failing to improve DEV can mean:

```text
treatment signal too small relative to noise
representation misses the action effect
target construction is weak
training is underpowered
true incremental effect is negligible
```

Only a valid randomized identification experiment can distinguish these possibilities with appropriate evidence.

## 9. WISE planning role

WISE consumes causal state and model predictions to choose a bounded future correction:

```text
1. read causal StateBank state
2. generate bounded U_plan candidates
3. roll out F_nominal + G_action
4. score candidate trajectories
5. reject stale / uncertain / infeasible plans
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

WISE is model-predictive in function. It does not require a large nonlinear online optimizer if a smaller enumerated/library/search policy meets latency and accuracy requirements.

## 10. Serving safety and fallback

WM/WISE output must carry:

```text
plan identity
state/source frontier
validity/freshness
uncertainty/confidence
applicable horizon
candidate bounds
```

If any serving contract fails:

```text
candidate -> ZERO / unavailable
baseline -> continues
```

The predictive path never holds the fast controller hostage to a stale or unsupported prediction.

## 11. Current training gate — fresh_35 state

Action-conditioned WM training remains blocked.

The latest randomized root, `fresh_35`, stopped in its first CALM row before any scientific block was admitted:

```text
sessions attempted/valid=1/0 of 8
blocks attempted/valid=1/0 of 96
native GUST=0
candidate/T_D/ACK/exposure=0/0/0/0
manifest slots=0
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

The current failure is infrastructure-only:

```text
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
```

It is not evidence about WM quality, treatment effect or FAST efficacy.

## 12. Required training sequence

No action-conditioned training begins until:

```text
complete valid randomized root
→ canonical reverse/Tarjan/peeling validity
→ separate causal-dataset admission
→ treatment SNR / carryover / constraint review
→ G_action identification authorization
→ model training authorization
```

A valid but weak action signal leads to experiment-design review, not automatic amplitude escalation.

## 13. Relationship to FAST research

FAST optimization is currently a separate simulator shadow/replay research track.

Current scientific baseline remains frozen:

```text
B = PX4 + AURA + current FAST/T1/C1
```

No FAST replacement algorithm is selected. The earlier residual-PI proposal is not current main-pipeline direction.

The practical sequencing rule is:

```text
close current causal acquisition infrastructure
+
research FAST challengers separately
→ after Phase-0, review which FAST baseline should be carried forward
→ train/validate production WM against the final frozen baseline
```

This avoids both changing the current estimand mid-campaign and over-investing in a final WM before the baseline controller is settled.

## 14. Evaluation philosophy

Future WM evaluation must separate:

1. **prediction quality** — forecast accuracy of the controlled state;
2. **incremental action quality** — whether `G_action` predicts randomized action response beyond ZERO;
3. **planning utility** — whether model-guided action improves closed-loop trajectory behavior;
4. **latency/deadline behavior** — whether serving meets realtime requirements without blocking FAST;
5. **uncertainty/fail-closed behavior** — whether unsupported plans are rejected safely;
6. **incremental value over best FAST baseline** — whether WM/WISE adds enough benefit to justify complexity.

These layers must not be collapsed into one scalar metric.

## 15. Deployment decision principle

World Model is not automatically required in the final production loop merely because it exists in the research architecture.

After a best FAST baseline is established, compare:

```text
C0 = best qualified FAST baseline
C1 = C0 + WM/WISE predictive refinement
```

Retain WM/WISE in runtime only if repeat-supported incremental benefit justifies its latency, compute, uncertainty and engineering complexity.

This is a future benchmark rule, not a current Phase-0 decision.

## 16. Research references

Use the source registry categories:

```text
world_model_ml
closed_loop_identification
micro_randomized_trials
small_sample_cluster_inference
uav_control
disturbance_observer
stability_robust_control
```

Closed-loop identification and randomized-design sources govern the causal experiment. PX4 source/docs govern runtime control semantics.
