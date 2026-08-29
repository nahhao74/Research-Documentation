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

Randomization provides the treatment variation while the baseline controller remains active. Post-treatment FAST/PX4 reactions caused by the candidate are part of the realized closed-loop treatment response.

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

## 4. Canonical scientific world

All contexts use the same deterministic plugin-bearing generated world.

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

`NativeDisturbanceSystem` is loaded in both contexts:

```text
CALM   = plugin present; no nonzero disturbance command
GUST_E = identical world/plugin bytes; frozen predeclared +E stimulus
```

Context labels alone are not scientific evidence. GUST_E requires native-truth evidence that the physical disturbance was actually applied.

The final generated world must be prepared, SHA-256 format-validated, hashed and matched **before** PX4/Gazebo runtime launch.

## 5. Exact exposure arms

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

## 6. Session bootstrap

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

Required streams:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

The bootstrap transaction is excluded from scientific exposure and does not consume a manifest slot.

A session-start bootstrap-only status is **not** a candidate release and must not seed H1000. Candidate-only H1000 begins only after an actual candidate release.

## 7. Mandatory full pre-science corridor

A randomized pilot must not be used as the first integration test of a newly repaired path.

Before authorizing the next scientific root, the exact real pilot runner/probe must pass a bounded non-scientific corridor:

```text
runtime/startup
-> generated-world identity gate
-> source attestation
-> StateBank 7/7
-> session bootstrap_only
-> first-block snapshot
-> C1 valid-offer frontier
-> pre-offer native-source / clock / provenance validity
-> intentional stop immediately before FIRST_SCIENTIFIC_T_D
```

The corridor must satisfy:

```text
FIRST_SCIENTIFIC_T_D=NOT_REACHED
CANDIDATE_OFFER_COUNT=0
CANDIDATE_PUBLISH_COUNT=0
E8_CANDIDATE_TRANSACTION_COUNT=0
SCIENTIFIC_BLOCKS=0
MANIFEST_SLOTS_CONSUMED=0
```

Mocking or bypassing the live C1/source path is not sufficient.

Process invariant:

```text
repair
-> deterministic regression
-> component qualification
-> FULL INTEGRATED PRE-SCIENCE CORRIDOR
-> randomized scientific root
```

## 8. Scientific start boundary

The root becomes scientifically active at:

```text
FIRST_SCIENTIFIC_T_D
```

Before that boundary, failures are classified as pre-science infrastructure failures.

After that boundary, the first scientific/certification invalidation stops the entire root.

## 9. Timing and causal validity

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

Native PX4 source frontier is not required to be represented as host/agent epoch time. A domain mismatch must fail explicitly rather than silently coercing native source time.

Mapping-epoch transition during science currently fails closed unless an explicitly qualified live transition-certification path is later approved.

## 10. T_D, T_A and outcomes

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

## 11. H1000

Candidate-history closure uses:

```text
H1000 = 1,000,000 native source us
```

H1000 is candidate-only. FAST/T1/C1 baseline activity is not candidate exposure.

Session-start `bootstrap_only` does not create an H1000 release anchor. H1000 applies only after an explicit candidate release or a separately qualified candidate bootstrap.

H1000 is not interpreted as proof that all physical carryover has vanished; carryover is assessed separately.

## 12. E8 / identity / release

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

## 13. Complete-root requirement

Scientific statistics are computed only if:

```text
SESSIONS_VALID=8/8
BLOCKS_VALID=96/96
```

A truncated or invalid root must not produce treatment efficacy inference.

No pooling across failed roots.

## 14. Frozen analysis

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

## 15. Treatment SNR diagnostics

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

## 16. Allowed interpretation

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

A valid but weak pilot goes to experiment-design review. It does not automatically trigger larger candidate amplitude.

## 17. Root governance

After first scientific `T_D`:

```text
first invalid scientific/certification event -> STOP root
```

No patch-and-continue. No later-session continuation. No block deletion. No relabeling. Failed roots remain immutable evidence.

Implementation-preserving repairs may be performed outside the immutable failed root, followed by the mandatory integrated pre-science corridor before another scientific attempt.

## 18. Authority

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
production_authority=false
```

Training/R1 begins only after a complete valid pilot and subsequent causal-identification review.
