# WM1 Randomized Identification Contract

## 0. Authority and current pilot identity

This file is the canonical scientific contract for the current WM1 randomized `G_action` pilot.

Current authoritative manifest identity:

```text
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
```

Earlier schedule digests retained in historical documents are provenance only and are not current execution authority unless explicitly restored by the owner.

Current runtime status and next allowed task are not defined here; use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

## 1. Scientific estimand

The experiment estimates the incremental closed-loop action effect under the active controller baseline:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
```

with:

```text
B = active PX4 + AURA + FAST/T1/C1 baseline
```

This is not an open-loop plant-identification experiment.

## 2. Why the baseline remains active

The scientific question is whether a bounded candidate adds useful predictive/control effect **inside the deployed closed loop**. Disabling FAST/T1/C1 changes the system being identified and no longer estimates the intended `G_action`.

Randomization supplies treatment variation while the baseline remains active. FAST/PX4 reactions caused after treatment are part of the realized closed-loop treatment path.

## 3. Frozen pilot design

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

No adaptive arm rebalancing, replacement, resampling or post-hoc relabeling is allowed.

## 4. Canonical scientific world

All contexts use the same deterministic plugin-bearing generated world:

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

`NativeDisturbanceSystem` is present in both contexts:

```text
CALM   = plugin present; zero/no nonzero disturbance command
GUST_E = identical world/plugin bytes; frozen predeclared +E stimulus
```

Context labels alone are not scientific evidence. `GUST_E` requires native-truth evidence that the physical disturbance was actually applied.

The generated world must be prepared, format-validated, hashed and matched before PX4/Gazebo runtime launch.

## 5. Exact exposure arms

```text
ZERO = exactly 0 accepted nonzero candidate cycles
P1   = 0.008 m/s^2 x exactly 5 accepted cycles
P2   = 0.012 m/s^2 x exactly 7 accepted cycles
```

Treatment is defined at the qualified accepted candidate boundary.

Forbidden:

```text
extra compensation cycles
re-dose after partial exposure
post-hoc arm relabeling
excluding inconvenient assigned blocks
adaptive amplitude change
replacing failed P1/P2 with another treatment
```

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

Bootstrap consumes no scientific manifest slot and does not seed candidate-only H1000.

## 7. Mandatory pre-science integration corridor

A randomized pilot must not be the first integration test of a newly repaired path.

The governing pattern is:

```text
repair
→ deterministic regression
→ component qualification
→ bounded integrated non-scientific qualification/corridor
→ owner review
→ randomized scientific root
```

The exact corridor evolves with the current blocker, but it must exercise the repaired path without treatment-effect scientific credit and preserve:

```text
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_ACTIONS=0
MANIFEST_SLOTS_CONSUMED=0
SEALED_ACCESS=0
production_authority=false
```

Current execution-specific qualification requirements and their closure state are defined in the latest execution ladder, not duplicated here.

## 8. Scientific authority boundary and diagnostic markers

`T_D` is the causal decision frontier defined by this contract. However, raw instrumentation may contain diagnostic markers whose names include strings such as:

```text
first_scientific_t_d_committed
```

A diagnostic marker alone does **not** grant scientific block admission or treatment authority.

Authoritative scientific admission requires the complete frozen transaction/block contract to succeed and be recorded by the canonical scientific accounting path.

Therefore:

```text
raw diagnostic T_D marker
!= accepted scientific block
!= treatment-effect credit
!= manifest consumption
```

This distinction is required when auditing infrastructure-invalid roots such as historical `fresh_33` and `fresh_34`.

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

Source continuity uses native PX4 source identity. Cross-domain clock alignment is separate.

A host-only or Gazebo-only diagnostic must not be assigned a fabricated PX4 source timestamp.

Mapping-epoch transition during science fails closed unless an explicitly qualified live transition-certification path is later approved.

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

Horizon terminology is standardized as:

```text
DATASET / CAUSAL-COMPLETENESS HORIZONS = H0, H20, H40, H80
PRIMARY PILOT TREATMENT CONTRASTS       = H40, H80
```

Availability at H0/H20 supports complete causal record construction and diagnostics; primary treatment-response interpretation remains focused on H40/H80 unless a later frozen analysis contract changes that explicitly.

## 11. H1000

Candidate-history closure uses:

```text
H1000 = 1,000,000 native source us
```

H1000 is candidate-only. FAST/T1/C1 baseline activity is not candidate exposure.

Session bootstrap does not create a release anchor. H1000 begins only after an explicit candidate release or separately qualified candidate bootstrap.

H1000 is not proof that all physical carryover has vanished; carryover is assessed separately.

## 12. E8 / identity / release

Candidate action remains bound to exact:

```text
generation
controller session
reset identity
source frontier
plan/action identity
accepted status
```

A pending valid candidate waiting for ACK cannot be superseded by unrelated baseline publication. Explicit release closes the nonzero treatment window.

Current E8 source-causal infrastructure preserves:

```text
newest received positive-source nonfuture AURA record
→ existing exact reset/session/health/freshness/provenance gates
```

No future AURA sample, cross-reset carryover or older-favorable fallback is allowed.

Applied-status observer visibility is separate from runtime application authority. The canonical `next_status` successor predicate is:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

The mirror must not hide a contract-valid successor merely because additional runtime health/authority fields fail; those application gates retain their own canonical role after mirror publication.

## 13. Native-event lifecycle requirement

A scientific GUST block must have valid native-truth onset/clear identity.

A new native event must not overlap an earlier event when the canonical event owner still reports the previous event active.

The qualified implementation invariant is:

```text
arm
→ consume/onset
→ exact matching canonical CLEAR
→ clear
→ complete
→ retire
→ only then next native-event arm eligibility
```

This is an inter-block readiness requirement. It does not alter frozen within-block GUST profile, `M_STABLE_US`, `W_MAX_US`, `T_D`, `T_A`, H1000, assignment or treatment semantics.

The bounded non-scientific qualification demonstrated three consecutive GUST events with zero `PREVIOUS_EVENT_STILL_ACTIVE` rejection and zero overlap. Current status/authority remains defined by the execution ladder.

## 14. Complete-root requirement

Scientific statistics are computed only if:

```text
SESSIONS_VALID=8/8
BLOCKS_VALID=96/96
```

A truncated or infrastructure-invalid root must not produce treatment-effect inference.

No pooling across failed roots.

```text
partial valid rows != scientific dataset
infrastructure-invalid root != negative treatment result
```

## 15. Frozen analysis

### E0 — primary randomized contrast

Assigned-arm / ITT-style contrast under the randomized schedule.

### E1 — secondary context-adjusted model

Frozen context-specific ridge model:

```text
lambda = 1e-3
```

Uses pre-treatment covariates/session effects only. No imputation that introduces post-treatment or future information.

### Cluster bootstrap

Session is the independent cluster:

```text
4 sessions/context
10,000 bootstrap repetitions
```

Blocks and controller cycles are not treated as independent subjects.

### E2

Optional frozen nominal-residualization diagnostic. Secondary only; it must not overwrite E0 interpretation.

## 16. Treatment-SNR diagnostics

P1/P2 identifiability is evaluated in output space, not by comparing candidate acceleration magnitude directly with baseline controller-command magnitude.

Required diagnostics include:

```text
ZERO outcome variability
P1/P2 randomized contrasts at H40/H80
treatment-to-noise ratio within context
session-to-session variability
carryover indicators
lagged FAST-candidate interaction
projection/constraint/saturation activity
```

A Fisher Information Matrix is allowed only after a concrete parametric model, likelihood/noise assumptions and sensitivity Jacobian are specified.

## 17. Allowed interpretation

Pilot signal labels:

```text
CLEAR
WEAK
NOT_RESOLVED
```

The pilot alone does not authorize:

```text
ACTION_RESPONSE_IDENTIFIED
```

A valid but weak pilot goes to experiment-design review. It does not automatically authorize larger candidate amplitude.

## 18. Root governance

After scientific execution begins, the first invalid scientific/certification event stops the entire root.

```text
no patch-and-continue
no later-session continuation
no block deletion
no relabeling
no in-root retry / replacement / resampling
```

Failed-root classifications remain immutable historical conclusions. Raw failed roots may later be deleted by explicit owner storage cleanup; deletion never changes the classification or permits pooling/reconstruction as science.

Implementation-preserving repairs occur outside the failed root, followed by deterministic regression and bounded non-scientific qualification before another owner-authorized scientific attempt.

## 19. Causal dataset admission and WM boundary

A complete root still requires a separate causal-dataset audit covering at minimum:

```text
randomization integrity
context/arm support
assigned vs accepted treatment identity
ZERO/P1/P2 execution
T_D/T_A ordering
H1000 completeness
H0/H20/H40/H80 availability
source/session/reset continuity
washout/contamination
no post-treatment conditioning
response construction
```

Output:

```text
CAUSAL_DATASET_ACCEPTANCE=PASS | FAIL | INSUFFICIENT
```

Only `PASS` may progress to `G_action` identification. World-Model training still requires separate authorization.

## 20. Authority

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
production_authority=false
```

No scientific-contract, authority or model-training promotion is implied by an infrastructure qualification.

Future FASTv2/PID/INDI research is outside this frozen contract. Any promotion that changes the active baseline `B` requires a new versioned control/scientific contract after the current Phase-0 identification closes.
