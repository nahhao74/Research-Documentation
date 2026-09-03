# PID Benchmark — G3 Progress Snapshot — 2026-09-03

## Current state

```text
G3_CURRENT_STATE=CONTEXT_DEV_EXTRAPOLATION_BLOCKS_CONCLUSION
FRF_PROMOTED_BAND=0.125-0.500_HZ
TMD_TRAJECTORY_DEPENDENCE=SUPPORTED
TMD_SESSION_DRIFT=SUPPORTED
TMD_OPERATING_REGIME=SCREENING_SIGNAL
TMD_AMPLITUDE=SCREENING_SIGNAL
M0_M1_GENERALIZATION=INSUFFICIENT
M2_M3_CONTEXT_RESULT=NUMERICALLY_IMPROVED_BUT_EXTRAPOLATION_BLOCKED
SELECTED_MODEL=NONE
R5_RESPONSE_OPENED=false
NEXT_GATE=EXISTING_DATA_CONTEXT_SUPPORT_EXPANSION_AUDIT
```

This is a progress snapshot for the PID benchmark G3 identification/modeling branch. It does not change the frozen architecture or authorize controller/runtime work.

## G3 scientific boundary

The estimand remains:

\[
u(t)=U^{accepted}_{NE}(t),\qquad y(t)=[v_N(t),v_E(t)]^T
\]

Input timing is the PX4 `PositionControllerPublicationAudit` accepted/read boundary with causal accepted-action ZOH. PX4 N/E position/velocity feedback is bypassed, D control and PX4 attitude/rate/allocation remain active, and AURA/FAST/WM/WISE/AEGIS are inactive in the G3 arm. R5 remains sealed.

## Acquisition and FRF

G3-A completed 10/10 valid sessions and 5/5 valid H2 pairs. TRAIN is R1-R3, DEV is R4, HELD_OUT is R5. The promoted contiguous MIMO FRF band is 0.125, 0.250, 0.375 and 0.500 Hz. Higher 0.625-2.000 Hz lines remain diagnostic-only.

The frequency-domain plant is therefore observed and promoted. The unresolved problem is deterministic parametric-model generalization, especially R4-B.

## Parametric-model history

VARX/predictor, stable latent state-space, explicit timing, direct PEM/Box-Jenkins and initial-state variants did not earn deterministic plant promotion. TRAIN fit, one-step prediction and noise separation could improve while R4-B remained poor. Blind order search was therefore stopped.

## TMD mechanism-discrimination result

The fresh TMD campaign completed:

```text
SESSIONS_VALID=12
PAIRS_VALID=6
ANCHORS_VALID=3
SESSION_SCIENTIFIC_CREDIT=12
```

A cross-root timing issue was closed by using `HOST_MONOTONIC_NS` only as the campaign/drift interpolation coordinate; within-root plant timing remains PX4 accepted/read-boundary source time.

Final TMD mechanism outcomes:

```text
TRAJECTORY_DEPENDENCE=SUPPORTED
SESSION_DRIFT=SUPPORTED
OPERATING_REGIME_DEPENDENCE=SCREENING_SIGNAL
AMPLITUDE_DEPENDENCE=SCREENING_SIGNAL
```

Thus a stationary global plant is not the only supported representation, but state/amplitude evidence is not strong enough to mandate LPV or nonlinear scheduling.

## M0/M1 trajectory-conditioned execution

Frozen models:

```text
M0 = stationary G0
M1 = G0 + sigma*DeltaG
sigma=-1 for member A
sigma=+1 for member B
```

DEV result:

| model | DEV mean plant NRMSE | worst-session mean NRMSE |
|---|---:|---:|
| M0 | 1.21698 | 1.65282 |
| M1 | 1.19452 | 1.65954 |

M1 improved R4-A but slightly worsened R4-B. Both failed the inherited no-session-catastrophe gate `max per-session mean NRMSE < 1.0`.

Conclusion:

```text
TRAJECTORY_CONDITIONING_ALONE=INSUFFICIENT
```

Trajectory dependence remains scientifically supported; it is simply not sufficient to recover deterministic DEV generalization.

## Causal drift context

The original `final 2 s before P1` context proposal was rejected because P0 already contains the scientific waveform, making that feature treatment-mediated.

The causal context is now frozen as:

```text
window=[QUALIFY-2s, QUALIFY)
accepted action=ZERO
waveform inactive
c=[1,s_preQ_N,s_preQ_E]
```

G3-A TRAIN alone had weak/near-saturated context support. Six valid TMD CENTER/A0 anchors (P01/P04/P06 A/B) were then admitted only as `AUXILIARY_TRAIN_CONTEXT_IDENTIFICATION`, producing 12 total context sessions.

Augmented feature support:

```text
context design rank=3
max grouped fit leverage=0.7817507
minimum residual DOF/output=7
max held-out prediction leverage=1.1021881
```

The campaigns overlap in feature space and are not exactly affinely separable. TMD anchors are not plant/noise TRAIN and are no longer independent confirmatory evidence for the context map.

## M2/M3 locked-plant context execution

The context experiment kept the fitted plants/noise locked:

```text
M2 = locked M0 + B_d c_r
M3 = locked M1 + B_d c_r
```

TMD anchor responses were allowed to estimate only the shared session-level `B_d` map. They did not update G0, DeltaG, poles, modes, H1 noise, FRF objectives or model order.

DEV result:

| model | DEV mean plant NRMSE | worst-session mean NRMSE |
|---|---:|---:|
| M0 | 1.21698 | 1.65282 |
| M1 | 1.19452 | 1.65954 |
| M2 | 1.11524 | 1.42236 |
| M3 | 1.09181 | 1.42883 |

M2/M3 numerically improve the worst DEV session by about 0.23 mean NRMSE, but both remain above the `<1.0` gate.

The decisive blocker is context support:

```text
TRAIN held-out context leverage envelope=1.1021881
R4-A context leverage=0.317244
R4-B context leverage=1.309137
CONTEXT_DEV_EXTRAPOLATION=true
```

R4-B is outside the context-training envelope. Context/plant leakage checks passed and M2/M3 preserve the locked plant FRF exactly, but the numerical improvement cannot receive drift-separation or model-promotion credit.

## Current interpretation

Evidence currently supports:

```text
stationary global model               -> insufficient
trajectory-conditioned model          -> insufficient
locked plant + causal constant context -> promising but extrapolative
trajectory + causal constant context   -> promising but extrapolative
```

Session/context variation remains the strongest unresolved modeling hypothesis. The current result neither proves nor disproves drift separation; it is support-limited.

## Next gate

Before new runtime, perform a response-blind existing-data context-support expansion audit:

1. inventory historical scientifically reusable CENTER/A0 G3/TMD roots;
2. apply factor/source/validator eligibility without using response performance;
3. extract the same pre-QUALIFY zero-action context feature;
4. test whether R4-B becomes covered by defensible context feature support;
5. do not refit M2/M3 during this eligibility audit.

If existing evidence is insufficient, design a small dedicated CENTER/A0 context-support acquisition. Do not repeat the full G3/TMD campaign and do not resume blind plant-model order search.

## Protection

```text
SELECTED_MODEL=NONE
R5_RESPONSE_OPENED=false
PRODUCTION_MODEL_AUTHORITY=false
NEW_RUNTIME_FROM_THIS_SNAPSHOT=NOT_AUTHORIZED
```

Large runtime/capture artifacts remain under `/media/nahhao74/KINGSTON/PID_Benchmark_Track` and are not committed to this documentation repository.

Key local evidence includes:

```text
PID_Benchmark_Track/reports/G3_TMD_12_SESSION_EXECUTION_AND_MECHANISM_SCREENING.md
PID_Benchmark_Track/reports/G3_TMD_CROSS_ROOT_CAMPAIGN_TIME_COORDINATE_CLOSURE.md
PID_Benchmark_Track/reports/G3_TMD_CONTRAST_COORDINATE_AMENDMENT_AND_MECHANISM_ANALYSIS.md
PID_Benchmark_Track/reports/G3_MIXED_MECHANISM_MODELING_CONTRACT.md
PID_Benchmark_Track/reports/G3_MIXED_MODEL_PREQUALIFY_CONTEXT_CAUSAL_CLOSURE.md
PID_Benchmark_Track/reports/G3_M0_M1_TRAJECTORY_CONDITIONED_MODEL_EXECUTION.md
PID_Benchmark_Track/reports/G3_TMD_ANCHOR_CONTEXT_AUGMENTATION_AUDIT.md
PID_Benchmark_Track/reports/G3_M2_M3_LOCKED_PLANT_CONTEXT_EXECUTION.md
```
