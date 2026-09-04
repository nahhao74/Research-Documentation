# PID Benchmark — G3 Progress Snapshot — updated 2026-09-04

## Current state

```text
G3_CURRENT_STATE=G3_MULTIPLE_STRUCTURAL_MECHANISMS_REMAIN_MIXED_UNRESOLVED
FRF_PROMOTED_BAND=0.125-0.500_HZ
TMD_TRAJECTORY_DEPENDENCE=SUPPORTED
TMD_SESSION_DRIFT=SUPPORTED
TMD_OPERATING_REGIME=SCREENING_SIGNAL
TMD_AMPLITUDE=SCREENING_SIGNAL
M0_M1_GENERALIZATION=INSUFFICIENT
LOCKED_PARENT_STRUCTURAL_ADEQUACY=INSUFFICIENT
SELECTED_MODEL=NONE
MODEL_PROMOTION=NOT_RUN
R4_RESPONSE_USED=false
R5_RESPONSE_OPENED=false
NEW_RUNTIME=NOT_RUN
NEW_DATA=false
NEXT_GATE=READY_FOR_TARGETED_STRUCTURAL_DISCRIMINATION_DESIGN_OWNER_REVIEW
```

This file supersedes the earlier 2026-09-03 progress state `CONTEXT_DEV_EXTRAPOLATION_BLOCKS_CONCLUSION` as the current G3 snapshot for the PID benchmark branch. It remains research/identification state only and does not authorize controller/runtime work.

## G3 scientific boundary

The estimand remains:

\[
u(t)=U^{accepted}_{NE}(t),\qquad y(t)=[v_N(t),v_E(t)]^T
\]

Input timing remains the PX4 `PositionControllerPublicationAudit` accepted/read boundary with source-time causal accepted-action ZOH. PX4 N/E position/velocity feedback is bypassed; D control plus PX4 attitude/rate/allocation remain active; AURA/FAST/WM/WISE/AEGIS are inactive in the G3 identification arm.

The promoted contiguous MIMO FRF band remains:

```text
0.125, 0.250, 0.375, 0.500 Hz
```

Higher lines remain diagnostic only.

## What changed after the earlier context-support blocker

The previous G3 state established that:

```text
stationary global model               -> insufficient
trajectory-conditioned model          -> insufficient
locked plant + causal constant context -> numerically promising but support-limited
trajectory + causal constant context   -> numerically promising but support-limited
```

Subsequent observable-state, initial-state, reverse-peeling and locked-parent forensic work moved the blocker deeper.

The current conclusion is no longer merely that R4-B lies outside the context support envelope. The current conclusion is:

```text
A single locked stationary low-order parent is structurally inadequate,
but the structural mechanism responsible for the inadequacy is not yet separated.
```

## Locked-parent forensic result

The latest forensic used exactly the authorized six G3A R1/R2/R3 A/B TRAIN response sequences for fixed-parent replay and did not fit or promote a replacement model.

Key protection state:

```text
NEW_RUNTIME=NOT_RUN
NEW_DATA=false
PLANT_REFIT=NOT_RUN
PLANT_ORDER_CHANGED=false
NOISE_MODEL_CHANGED=false
NEW_MODEL_FAMILY=false
NEW_CONTEXT_FEATURES=false
R4_RESPONSE_USED=false
R5_RESPONSE_OPENED=false
TMD_RESPONSE_RESCORING=false
SELECTED_MODEL=NONE
MODEL_PROMOTION=NOT_RUN
STAGE2_RUNTIME=NOT_AUTHORIZED
```

The principal forensic signature is:

```text
one-step prediction can be very good
while deterministic free-run prediction remains poor
and residual structure persists over physical lags.
```

This favors accumulated state-transition / structural mismatch over a pure white-measurement-noise explanation, but it does not identify one exclusive cause and does not justify blind order increase.

## Current structural hypothesis ledger

```text
H1 — memory / modal-timescale inadequacy
     HIGH_PRIORITY_UNRESOLVED

H2 — trajectory / polarity-dependent dynamics
     SUPPORTED mechanism evidence
     but not a complete locked-parent explanation

H3 — session / operating-regime-dependent dynamics
     HIGH_PRIORITY_UNRESOLVED

H4 — nonlinear / amplitude-regime effect
     UNRESOLVED_EXISTING_DATA_INSUFFICIENT

H5 — plant / noise separation inadequacy
     HIGH_PRIORITY_UNRESOLVED

H6 — cross-axis / MIMO structural inadequacy
     UNRESOLVED_EXISTING_DATA_INSUFFICIENT

H7 — hidden non-retained runtime state / history
     HIGH_PRIORITY_UNRESOLVED
```

`SUPPORTED` means compatible mechanism evidence, not exclusive causal proof. `PEELED` means removed from the next candidate set under current evidence, not proven false.

## TMD interpretation retained

The inherited TMD outcomes remain:

```text
TRAJECTORY_DEPENDENCE=SUPPORTED
SESSION_DRIFT=SUPPORTED
OPERATING_REGIME_DEPENDENCE=SCREENING_SIGNAL
AMPLITUDE_DEPENDENCE=SCREENING_SIGNAL
```

These findings support the statement that one simple stationary global representation is inadequate. They do **not** establish that a specific measurable scheduling state `z` already exists or that LPV scheduling is currently qualified.

## Consequence for LPV / scheduled PID

The correct downstream distinction is now:

```text
supported:
    plant dynamics vary / one stationary parent is inadequate

not yet supported:
    plant dynamics vary as a known deployable function G(s; z)
```

LPV / gain scheduling therefore remains a downstream controller hypothesis.

Before promoting an LPV identification target, the next work must establish a candidate scheduling state `z` that is:

```text
measurable before the relevant action
causal / response-independent
available online
repeat-supported
inside a defensible support envelope
not merely a session label or proxy
not a post-treatment consequence
```

If such a state is supported, the identification target may move from

\[
G_{NE}(s)
\]

toward

\[
G_{NE}(s;z)
\]

or an LPV state-space family.

If no adequate `z` is found, the branch must retain a robust fixed-controller path rather than force unsupported scheduling.

## INDI status

INDI is not part of the current G3 identification arm and does not close the G3 structural question.

It remains the strongest fast-augmentation hypothesis after a qualified nominal fixed or scheduled PID exists.

Preferred benchmark logic is now:

```text
nominal robust PID
    vs
nominal robust PID + INDI
    vs
nominal robust PID + AURA/FAST
```

Only if INDI and AURA/FAST show repeat-supported complementary benefit should a combined fast-augmentation arm be opened.

The preferred INDI development order is:

```text
INDI0 — classical measured-response INDI
INDI1 — delay-synchronized / filtered INDI
INDI2 — actuator-dynamics-aware INDI
INDI3 — bounded control-effectiveness-adaptive INDI
```

No variant is retained without ablation evidence.

## Latency objective

The PID branch objective is not minimum arithmetic time alone.

Freeze and compare:

```text
T_compute — source sample -> controller output
T_accept  — source sample -> PX4 accepted-action boundary
T_effect  — physical event -> first useful corrective physical action
T_recover — physical event -> defined recovery threshold
```

For each report:

```text
p50
p95
p99
max
jitter
```

Champion selection should prioritize `T_effect` and `T_recover` over `T_compute` alone.

For INDI / fast-path candidates also retain:

```text
measurement source age
acceleration / derivative estimate age
filter group delay
relative command/measurement-path delay
actuator response / model delay
control-effectiveness freshness
PX4 accepted-action latency
synchronization mismatch
```

## Causal-model role

Causal reasoning remains an identification/supervisory function, not a large first-response runtime controller.

It should preserve distinctions between:

```text
pre-treatment state/context
requested action
accepted action
actual application time
post-action response
```

A candidate scheduling variable must be available before the relevant response and cannot be selected solely because it correlates with downstream outcome.

This keeps the PID branch compatible with the broader CALE research direction without making CALE a current PID runtime dependency.

## Updated downstream architecture hypothesis

Current strongest benchmark hypothesis before controller evidence:

```text
CAUSALLY_QUALIFIED_RGS_2DOF_PID
+
SYNCHRONIZED_ACTUATOR_AWARE_INDI
```

Functional split:

```text
RGS / LPV PID:
    nominal tracking + operating-envelope variation

INDI:
    fast measured-response incremental correction

causal / system-identification layer:
    determine valid model family + valid scheduling variables
```

This is a falsifiable benchmark hypothesis, not a promotion decision.

## Current next gate

```text
READY_FOR_TARGETED_STRUCTURAL_DISCRIMINATION_DESIGN_OWNER_REVIEW
```

The next task should maximize separation among the remaining structural hypotheses rather than simply collect more sessions.

Conceptual discrimination axes include:

```text
same operating state / different trajectory history
    -> trajectory / memory discrimination

same trajectory family / different operating regime
    -> regime dependence

same trajectory and regime / controlled amplitude contrast
    -> amplitude nonlinearity

same physical experiment / richer source-state retention
    -> hidden runtime-state testability

qualified longer-timescale / low-frequency excitation
    -> memory / modal-timescale discrimination
```

Exact experiment design, source instrumentation, amplitude, duration, response access and runtime authority remain owner-review items.

## Branch documentation update

The expanded downstream architecture/research rationale is recorded in:

```text
docs/04_research/PID_BENCHMARK_RESEARCH_DIRECTION_UPDATE_20260904.md
```

The original branch design remains useful as historical architecture context, but this snapshot and the 2026-09-04 direction update control the current G3/downstream interpretation.

## Protection

```text
SELECTED_MODEL=NONE
SELECTED_CONTROLLER=NONE
R5_RESPONSE_OPENED=false
PRODUCTION_MODEL_AUTHORITY=false
IMPLEMENTATION_AUTHORIZED=false
NEW_RUNTIME_AUTHORIZED=false
```

Large runtime/capture/identification artifacts remain under `/media/nahhao74/KINGSTON/PID_Benchmark_Track` and are not committed to this documentation repository.
