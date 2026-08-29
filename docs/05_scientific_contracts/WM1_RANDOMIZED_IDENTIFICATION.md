# WM1 Randomized Identification Contract

## 1. Scientific estimand

The current experiment estimates the incremental closed-loop action effect under the active controller baseline:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

with:

```text
B = active PX4 + AURA + FAST/T1/C1 baseline
```

This is not an open-loop plant-identification experiment.

## 2. Why baseline remains active

The scientific question is whether a bounded candidate adds useful predictive/control effect **inside the deployed closed loop**. Disabling FAST/T1/C1 would change the system being identified and would no longer estimate the intended `G_action`.

Randomization provides the treatment variation while the baseline controller remains active.

## 3. Frozen pilot design

Manifest identity:

```text
wm1_v2r1_within_run_randomized_pilot_schedule_v1.json
SHA256=253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7
```

Campaign:

```text
worker A
8 sessions
4 CALM
4 GUST_E
12 scientific blocks/session
96 scientific blocks total
```

Assignment totals:

```text
ZERO = 48
P1   = 24
P2   = 24
```

Directions:

```text
+E
-E
+N
```

## 4. Exact exposure arms

```text
ZERO = exactly 0 accepted nonzero candidate cycles
P1   = 0.008 m/s^2 x exactly 5 accepted cycles
P2   = 0.012 m/s^2 x exactly 7 accepted cycles
```

Treatment is defined by accepted candidate contribution at the qualified correction boundary.

Forbidden:

- extra compensation cycles;
- re-dose after partial exposure;
- post-hoc arm relabeling;
- excluding inconvenient blocks after assignment;
- adaptive amplitude change;
- replacing failed P1/P2 with another treatment.

## 5. Session bootstrap

Every fresh session begins with a non-scientific source-complete bootstrap:

```text
block_index=-1
bootstrap_only=true
```

The bootstrap must:

```text
wait for healthy StateBank
wait for all 7 required same-session source-bound streams
request snapshot
perform atomic barrier recheck
receive valid accepted ACK
return before candidate/C1/E8/scientific paths
```

The bootstrap transaction is excluded from scientific exposure and does not consume a manifest slot.

## 6. Scientific start boundary

The root becomes scientifically active at:

```text
FIRST_SCIENTIFIC_SLOT_T_D_FREEZE
```

Before that boundary, failures are classified as pre-science infrastructure failures.

After that boundary, the first scientific/certification invalidation stops the entire root.

## 7. Timing and causal validity

Every scientific transaction requires:

```text
SOURCE_CONTINUITY_VALID=true
CLOCK_ALIGNMENT_VALID=true
ATOMIC_PROVENANCE_VALID=true
session/reset/frame identity valid
T_D native causal frontier valid
T_A native accepted frontier valid
```

Source continuity uses native PX4 timestamp + generation. Cross-domain clock alignment is separate.

Mapping-epoch transition during science currently fails closed unless an explicitly qualified live transition-certification path is later approved.

## 8. T_D, T_A and outcomes

```text
T_D = native decision frontier
T_A = native accepted candidate frontier
```

Outcome targets are aligned to actual accepted treatment:

```text
Y_h = first valid future local_state at native time >= T_A + h
```

No interpolation or future leakage.

Core pilot analysis focuses on H40/H80 treatment response while preserving the broader causal timing contract.

## 9. H1000

Candidate-history closure uses:

```text
H1000 = 1,000,000 native source us
```

H1000 is candidate-only. FAST/T1/C1 baseline activity is not candidate exposure.

H1000 is not interpreted as proof that all physical carryover has vanished; carryover is assessed separately.

## 10. E8 / identity / release

Candidate action must remain bound to exact:

```text
generation
controller session
reset identity
source frontier
plan/action identity
accepted status
```

A pending valid candidate waiting for ACK cannot be superseded by unrelated baseline publication. Explicit release closes the nonzero treatment window.

## 11. Complete-root requirement

Scientific statistics are computed only if:

```text
SESSIONS_VALID=8/8
BLOCKS_VALID=96/96
```

A truncated or invalid root must not produce treatment efficacy inference.

No pooling across failed roots.

## 12. Frozen analysis

### E0 — primary randomized contrast

Assigned-arm / ITT-style contrast under the randomized schedule.

### E1 — secondary context-adjusted model

Frozen context-specific ridge model:

```text
lambda = 1e-3
```

Uses pre-treatment covariates/session effects only. No imputation that introduces post-treatment or future information.

### Cluster bootstrap

Session is the independent cluster.

```text
4 sessions/context
10,000 bootstrap repetitions
```

Blocks and controller cycles are not treated as independent subjects.

### E2

Optional frozen nominal-residualization diagnostic. It is secondary and must not overwrite E0 interpretation.

## 13. Treatment SNR diagnostics

P1/P2 identifiability is evaluated in output space, not by comparing candidate acceleration magnitude to baseline command magnitude.

Required diagnostics include:

- ZERO outcome variability;
- P1/P2 randomized contrasts at H40/H80;
- treatment-to-noise ratio within context;
- session-to-session variability;
- carryover indicators;
- lagged FAST-candidate interaction;
- projection/constraint/saturation activity.

A Fisher Information Matrix should only be used after specifying a concrete parametric model, likelihood/noise assumptions and sensitivity Jacobian.

## 14. Allowed interpretation

Pilot signal labels:

```text
CLEAR
WEAK
NOT_RESOLVED
```

The pilot alone does not authorize the statement:

```text
ACTION_RESPONSE_IDENTIFIED
```

A valid but weak pilot goes to owner experiment-design review. It does not automatically trigger larger candidate amplitude.

## 15. Root governance

After first scientific `T_D`:

```text
first invalid scientific/certification event -> STOP root
```

No patch-and-continue. No later-session continuation. No block deletion. No relabeling. No automatic second scientific root.

Failed roots remain immutable evidence.

## 16. Authority

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
production_authority=false
```

Training/R1 begins only after a complete valid pilot and subsequent owner causal-identification review.