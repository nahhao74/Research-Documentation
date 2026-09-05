# World Model and WISE Architecture

## 1. Role

The World Model / WISE path is a **predictive refinement layer** on top of the active PX4 + AURA + FAST/T1/C1 closed loop.

It does not replace immediate disturbance rejection and must never become a prerequisite for first response.

```text
FAST path   = handle disturbance now
World Model = predict controlled future response
WISE        = select bounded predictive refinement
AEGIS       = enforce bounded execution
PX4         = authoritative flight control
```

Current runtime/scientific state is defined only by:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

## 2. Canonical model decomposition

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

### F_nominal

```text
F_nominal(X,h)
= future evolution under the active closed-loop baseline B
```

with:

```text
B = PX4 + AURA + FAST/T1/C1
```

This is a model of the **controlled system**, not an isolated open-loop airframe.

### G_action

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

`G_action` models the incremental closed-loop effect of a bounded candidate relative to ZERO under the **same** baseline.

## 3. Baseline dependency

The action-response model is conditional on the active baseline.

If:

```text
B0 = PX4 + AURA + FAST0/T1/C1
B1 = PX4 + AURA + FAST1/T1/C1
```

then do not assume:

```text
G_action^(B0) == G_action^(B1)
```

without direct evidence.

Therefore:

```text
current Phase-0 science freezes the active FAST/T1/C1 baseline
FAST research runs separately in shadow/replay
material FAST promotion requires a versioned baseline review
production action-conditioned data/model must be valid for the final frozen baseline
```

## 4. Why randomized action data is required

Ordinary closed-loop flight data can support baseline dynamics modeling but does not by itself isolate the incremental causal effect of a small candidate followed by controller reactions.

Randomized ZERO/P1/P2 interventions provide treatment variation while the baseline remains active.

Post-treatment FAST/PX4 reactions caused by a candidate belong to the realized treatment path; they must not be removed as if they were pre-treatment confounders.

## 5. State representation

World Model input `X` is assembled from StateBank using only causal records available at the decision frontier `T_D`.

Relevant classes include:

```text
local position / velocity
reference and tracking error
attitude / motion state
AURA disturbance state and validity
FAST/T1/C1 context
recent candidate history
controller / actuator context
source/session/reset identity
native timing and clock-alignment provenance
```

The representation may evolve, but future samples and post-assignment variables must never leak into `X(T_D)`.

## 6. Action representation

The candidate can persist over multiple accepted cycles, so the preferred serving representation is temporal:

```text
U_plan = bounded short temporal action sequence
```

A point action is used only when it faithfully represents the actual accepted exposure contract.

## 7. Decision-to-acceptance timing

The causal structure distinguishes:

```text
T_D = decision frontier
T_A = actual accepted-action frontier
```

A useful two-stage prediction structure is:

### Stage A

```text
X(T_D) -> X_hat(T_A)
```

using only information available at `T_D` plus a causal application-latency estimate.

### Stage B

```text
X_hat(T_A), U_plan -> Y(T_A+h)
```

The real future `X(T_A)` is supervision/audit evidence, never online decision input.

## 8. WISE planning role

WISE consumes causal state and WM rollouts:

```text
1. read causal StateBank state
2. generate bounded U_plan candidates
3. roll out F_nominal + G_action
4. score future trajectories
5. reject stale / uncertain / infeasible plans
6. select an acceptable bounded plan
7. hand the plan to AEGIS
```

Candidate scoring may include:

```text
trajectory / cross-track error
future velocity error
control effort
uncertainty
constraint margin
candidate switching / smoothness
```

WISE is model-predictive in function. It does not require a large nonlinear optimizer if a smaller bounded candidate library/search satisfies the measured requirement.

## 9. Serving contract and fallback

WM/WISE output must carry at least:

```text
plan identity
state/source frontier
validity/freshness
uncertainty/confidence
applicable horizon
candidate bounds
```

If serving validity fails:

```text
candidate -> ZERO / unavailable
baseline -> continues
```

The predictive path must never block FAST.

## 10. Training gate

Action-conditioned training requires this sequence:

```text
complete valid randomized root
→ canonical causal-validity audit
→ separate causal-dataset admission
→ treatment SNR / carryover / constraint review
→ G_action identification authorization
→ model-training authorization
```

Incomplete or infrastructure-invalid roots never authorize action-conditioned training.

A valid but weak action signal leads to experiment-design review rather than automatic treatment escalation.

## 11. Minimal model ladder

For an accepted final baseline, model capacity should increase only when simpler models fail held-out requirements:

```text
WM0 — persistence / simple closed-loop predictor
WM1 — linear / ridge action-conditioned model
WM2 — compact structured dynamics model
WM3 — small nonlinear model only if residual structure requires it
```

## 12. Evaluation layers

Evaluate separately:

```text
prediction quality
incremental G_action quality
planning utility
latency/deadline behavior
uncertainty/fail-closed behavior
incremental value over the best FAST baseline
```

Do not collapse all layers into one scalar score.

## 13. Runtime deployment decision

World Model / WISE is not automatically required in the final production loop.

After the best FAST baseline is established, compare:

```text
C0 = best qualified FAST baseline
C1 = C0 + WM/WISE predictive refinement
```

Retain WM/WISE at runtime only if repeat-supported incremental benefit justifies its latency, compute, uncertainty and engineering complexity.

## 14. Authority boundaries

World Model / WISE does not own:

```text
first disturbance response
PX4 inner-loop authority
raw thrust / motor command
scientific treatment validity
AEGIS safety authority
```

The active research sequence is defined by `../04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`; the source registry is methodological reference only.
