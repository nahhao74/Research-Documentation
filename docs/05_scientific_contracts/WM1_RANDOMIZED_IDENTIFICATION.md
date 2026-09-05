# WM1 Randomized Identification Contract

## 0. Authority and identity

This file is the canonical scientific contract for the current randomized `G_action` identification campaign.

Current runtime state and next allowed task are defined elsewhere:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

Authoritative manifest:

```text
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
```

Historical schedule digests are not execution authority unless explicitly restored by the owner.

## 1. Scientific estimand

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

This is closed-loop incremental action identification, not open-loop plant identification.

The baseline remains active because the scientific question is the additional effect of a bounded candidate inside the deployed closed loop. Post-treatment FAST/PX4 reactions caused by the candidate are part of the realized treatment path.

## 2. Frozen pilot design

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
ZERO=48
P1=24
P2=24
```

Directions:

```text
+E
-E
+N
```

Forbidden:

```text
adaptive arm rebalancing
replacement/resampling
post-hoc relabeling
arm-conditioned retry
```

## 3. Canonical scientific world

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

`NativeDisturbanceSystem` is present in both contexts:

```text
CALM   = plugin present; zero/no nonzero disturbance command
GUST_E = identical world/plugin bytes; frozen predeclared +E disturbance
```

`GUST_E` requires native-truth evidence that the physical disturbance was actually applied. Context labels alone are not evidence.

## 4. Exact treatment arms

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
adaptive amplitude change
post-hoc arm relabeling
excluding inconvenient assigned blocks
replacing failed P1/P2 with another treatment
```

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

Bootstrap consumes no scientific manifest slot and does not seed candidate H1000.

## 6. Mandatory pre-science qualification corridor

A randomized scientific root must never be the first integration test of a newly repaired path.

Required pattern:

```text
forensic / root-cause proof
→ minimal implementation-preserving repair
→ deterministic regression
→ bounded integrated non-scientific qualification
→ canonical validity audit
→ owner review
→ separate scientific authorization
```

The non-scientific corridor must preserve:

```text
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_ACTIONS=0
MANIFEST_SLOTS_CONSUMED=0
SEALED_ACCESS=0
production_authority=false
```

The exact currently required corridor is defined by the latest execution ladder.

## 7. Scientific admission vs diagnostics

Raw instrumentation markers are not scientific admission.

For example:

```text
first_scientific_t_d_committed
```

by itself does not imply:

```text
accepted scientific block
treatment-effect credit
manifest consumption
```

Authoritative admission requires the complete frozen transaction/block contract to succeed and be recorded by the canonical accounting path.

This rule applies to all infrastructure-invalid roots, including `fresh_33`, `fresh_34`, `fresh_35` and later failures.

## 8. Timing and causal validity

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

Do not fabricate a PX4 source timestamp for a host-only or Gazebo-only diagnostic.

A mapping epoch/generation transition during science fails closed unless a separately qualified transition-certification path is explicitly approved.

Forbidden repairs:

```text
future Timesync lookup
retrospective future-informed remapping
interpolation across mapping epochs
silently skipping invalid samples
extending/re-dosing treatment to compensate
```

## 9. T_D, T_A and outcomes

```text
T_D = native causal decision frontier
T_A = native accepted candidate frontier
```

Outcome target for horizon `h`:

```text
Y_h = first valid future local_state at native time >= T_A + h
```

No interpolation and no future leakage.

Horizon terminology:

```text
DATASET / CAUSAL-COMPLETENESS HORIZONS = H0, H20, H40, H80
PRIMARY PILOT TREATMENT CONTRASTS       = H40, H80
```

H0/H20 support complete causal construction and diagnostics. Primary treatment-response interpretation remains H40/H80 unless a future frozen analysis contract changes it.

## 10. H1000

```text
H1000 = 1,000,000 native source us
```

H1000 is candidate-history/refractory semantics only.

```text
FAST/T1/C1 baseline activity != candidate exposure
session bootstrap != candidate release
```

H1000 does not prove all physical carryover has disappeared. Carryover is assessed separately from pre-treatment state and randomized outcomes.

## 11. E8, accepted action and release identity

Candidate action is bound to exact:

```text
generation
controller session
reset identity
source frontier
plan/action identity
accepted status
```

A pending valid candidate waiting for ACK cannot be superseded by unrelated baseline publication. Explicit release closes the nonzero treatment window.

E8 source-causal pairing preserves:

```text
newest received positive-source nonfuture AURA record
→ exact reset/session/health/freshness/provenance gates
```

No future AURA sample, cross-reset carryover or older-favorable fallback is allowed.

## 12. Observer visibility vs runtime authority

Applied-status observer visibility and runtime application authority are separate contracts.

Canonical `next_status` successor predicate:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

The mirror must not hide a contract-valid successor merely because additional runtime health/authority fields fail; those application gates retain their role after observer publication.

Accepted-cycle callback/retention infrastructure may be repaired only in an implementation-preserving way. Such a repair must not loosen generation/session/reset/source matching, treatment identity or accepted-action semantics.

## 13. Native-event lifecycle

A GUST block requires valid native-truth onset/CLEAR identity.

Qualified inter-block invariant:

```text
arm
→ consume/onset
→ exact matching canonical CLEAR
→ clear
→ complete
→ retire
→ only then next native-event arm eligibility
```

This is an inter-block readiness rule. It does not change frozen within-block GUST timing, `M_STABLE_US`, `W_MAX_US`, `T_D`, `T_A`, H1000, assignment or treatment semantics.

## 14. Complete-root requirement

Scientific statistics are computed only if:

```text
SESSIONS_VALID=8/8
BLOCKS_VALID=96/96
```

Hard rule:

```text
partial valid rows != scientific dataset
infrastructure-invalid root != negative treatment result
```

No pooling across failed roots.

## 15. Frozen analysis

### E0 — primary randomized contrast

Assigned-arm / ITT-style contrast under the frozen schedule.

### E1 — context-adjusted model

Frozen context-specific ridge model:

```text
lambda=1e-3
```

Use pre-treatment covariates/session effects only. No post-treatment/future-informed imputation.

### Cluster bootstrap

Session is the independent cluster:

```text
4 sessions/context
10,000 bootstrap repetitions
```

Blocks and controller cycles are not independent subjects.

### E2

Optional frozen nominal-residualization diagnostic. Secondary only; it must not overwrite E0 interpretation.

## 16. Treatment-SNR and identifiability diagnostics

Evaluate P1/P2 in output space, not by comparing candidate command magnitude directly with baseline controller-command magnitude.

Required diagnostics:

```text
ZERO outcome variability
P1/P2 randomized contrasts at H40/H80
treatment-to-noise ratio within context
session-to-session variability
carryover indicators
lagged FAST-candidate interaction
projection/constraint/saturation activity
```

A Fisher Information Matrix is meaningful only after a concrete parametric model, likelihood/noise assumptions and sensitivity Jacobian are specified.

## 17. Allowed pilot interpretation

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

A valid but weak pilot goes to experiment-design review. It does not automatically authorize larger treatment amplitude.

## 18. Root governance

After scientific execution begins, the first invalid scientific/certification event stops the entire root.

```text
no patch-and-continue
no later-session continuation
no block deletion
no relabeling
no in-root retry / replacement / resampling
```

Failed-root classifications are immutable historical conclusions.

Implementation-preserving repairs occur outside the failed root, followed by deterministic regression, bounded non-scientific qualification, canonical validity audit and separate owner review.

## 19. Canonical causal-validity pipeline

Where applicable, qualification/scientific roots must reuse:

```text
reverse validity indexing
→ canonical causal dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
```

Do not create task-local replacement validity semantics.

## 20. Causal dataset admission and WM boundary

A complete root still requires a separate causal-dataset audit covering at minimum:

```text
randomization integrity
CALM/GUST and ZERO/P1/P2 support
assigned vs requested vs accepted treatment identity
T_D/T_A ordering
exact treatment exposure
H1000 completeness
native-event lifecycle
source/session/reset continuity
H0/H20/H40/H80 availability
washout/carryover
projection/constraint/saturation
no prohibited post-treatment conditioning
response construction
```

Output:

```text
CAUSAL_DATASET_ACCEPTANCE=PASS | FAIL | INSUFFICIENT
```

Only `PASS` may progress to `G_action` identification. World-Model training still requires separate authorization.

## 21. FAST baseline boundary

The current contract identifies `G_action` under the current FAST/T1/C1 baseline.

FAST shadow/replay research is outside this frozen contract. Any future material FAST promotion changes baseline `B` and therefore requires a new versioned control/scientific review before production action-conditioned data/model use.

No particular future FAST challenger is authorized by this contract.

## 22. Final authority boundaries

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
production_authority=false
```

Infrastructure qualification does not imply scientific, model-training, SEALED or production promotion.
