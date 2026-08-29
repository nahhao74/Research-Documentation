# Closed-Loop Identification Research Synthesis for WM1 / AEGIS

## Scientific target

The current causal target is the incremental closed-loop action effect

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

where `B` is the active PX4 + AURA + FAST/T1/C1 baseline.

## Randomized identification with FAST/T1/C1 active

Keeping the feedback baseline active is compatible with closed-loop identification as long as the scientific estimand is defined as the incremental intervention effect under that active baseline and treatment assignment provides independent excitation.

The goal is not to recover an open-loop plant by pretending the deployed feedback loops are absent. The goal is to identify the response of the actual controlled system to the bounded incremental candidate.

## FAST response after treatment

FAST/PX4 reactions caused after applying a randomized candidate are downstream treatment-mediated closed-loop response. They are part of the realized `G_action` path.

Pre-treatment FAST/AURA/controller state can act as context/moderation and belongs in the pre-treatment state/covariate set. Post-treatment FAST response must not be adjusted away as if it were a pre-existing confounder.

## Treatment SNR / identifiability

P1/P2 should be evaluated using output-space randomized contrasts and session-level variability, not by comparing raw candidate amplitude with baseline-controller amplitude.

Useful diagnostics include:

- ZERO floor / untreated variability;
- P1/P2 randomized contrasts at H40/H80;
- treatment-to-within-context noise ratio;
- session-level contrast variability;
- carryover diagnostics;
- lagged FAST-candidate interaction;
- constraint/projection/saturation activity.

Fisher Information Matrix metrics are meaningful only after a concrete parametric model class, likelihood/noise model and Jacobian are specified. They should not be emitted as generic evidence of identifiability before that specification exists.

## Few-session inference

The current pilot has only 4 CALM and 4 GUST_E sessions. Blocks/cycles inside a session are not independent experimental subjects.

The pilot therefore uses session-level clustering and is primarily a mechanism/SNR/carryover calibration experiment, not a definitive efficacy trial. Large-sample cluster-robust asymptotics should not be over-interpreted with so few independent clusters.

## H1000

H1000 controls direct candidate history/refractory semantics over 1,000,000 native source microseconds. It does not by itself prove that all physical plant/controller carryover has disappeared.

Physical carryover must be assessed from pre-treatment state and randomized outcome diagnostics rather than inferred solely from the refractory timer.

## PX4 authority / action semantics

The candidate is a bounded incremental requested contribution added to the active baseline at the qualified acceleration-correction boundary. PX4 inner loops remain authoritative.

The implementation does not assume linear physical superposition:

```text
u_total_requested = u_baseline + u_candidate
```

is exact at the requested composition boundary, while

```text
Y(B+U) = Y(B) + Y(U)
```

is not assumed for the nonlinear closed-loop physical response.

## Current research interpretation rule

A complete valid pilot may classify P1/P2 signal as:

```text
CLEAR
WEAK
NOT_RESOLVED
```

The pilot alone must not emit `ACTION_RESPONSE_IDENTIFIED`.

If the campaign is valid but signal is weak, the next state is owner experiment-design review rather than automatic amplitude increase or training on weak/uncertified treatment labels.
