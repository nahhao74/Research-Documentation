# Current Execution Ladder — WM Main Pipeline — 2026-09-05

## Purpose

This file is the execution-only ladder for the current AURA–WISE–WM–AEGIS main pipeline after the `fresh_34` infrastructure failure was closed by a separately qualified `next_status` successor-frontier repair.

The owner has authorized exactly one new fresh randomized `G_action` scientific pilot. The new root has not yet been executed.

This file does not replace scientific contracts. It summarizes the next executable sequence from the latest qualified infrastructure state.

## Current authority

```text
PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID

OPTION_B_LIVE_RUNTIME_QUALIFIED=true
OPTION_B_DOWNSTREAM_TRANSACTION_INTEGRATION=QUALIFIED_NOSCIENCE
RESET_AUTHORITY=AURA_C1_SOURCE_RESET

WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
POST_RESET_E8_SOURCE_CAUSAL_HANDOFF_QUALIFICATION=VALID_NONSCIENTIFIC
NATIVE_EVENT_CLEAR_LIFECYCLE_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
INTER_BLOCK_CLEAR_GATE=PASS_3_OF_3
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
PRE_RETRY_VALID_CAUSAL_CORE=true

LATEST_FAILED_SCIENTIFIC_ROOT=fresh_34
FRESH34_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

OWNER_FRESH_RANDOMIZED_PILOT_AUTHORIZED=true
NEW_FRESH_RANDOMIZED_PILOT_EXECUTED=false
AUTHORIZE_0B5_RERUN=false
AUTHORIZE_WM_TRAINING=false
AUTHORIZE_SEALED_OPEN=false

NEXT_STATE=OWNER_AUTHORIZED_NEW_FRESH_RANDOMIZED_PILOT_READY_TO_EXECUTE
```

## Frozen scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

No step below may change the estimand, AURA/FAST/T1/C1 semantics, treatment profiles, randomization, Direct Guard, `M_STABLE_US=100000`, `W_MAX_US=1000000`, `PX4_BOOT_US`, `AURA_C1_SOURCE_RESET`, `T_D/T_A`, H1000, SEALED, or production authority.

Authoritative pilot identity:

```text
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
MANIFEST_SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

## Historical blocker closures relevant to the next root

### Native-event lifecycle

`fresh_33` failed because a later GUST block armed before the previous exact native event reached canonical CLEAR. The implementation-preserving lifecycle repair now enforces:

```text
arm
→ onset
→ exact matching CLEAR
→ complete
→ retire
→ next block may arm
```

Bounded qualification:

```text
3/3 consecutive GUST blocks PASS
PREVIOUS_EVENT_STILL_ACTIVE_REJECTIONS=0
OVERLAPPING_NATIVE_EVENTS=0
INTER_BLOCK_CLEAR_GATE=PASS_3_OF_3
PRE_RETRY_VALID_CAUSAL_CORE=true
```

Historical `fresh_33` classification remains immutable. Its raw root was owner-deleted during storage cleanup; retained canonical summaries preserve the historical conclusion.

### fresh_34 next-status failure

`fresh_34` passed preflight but failed in the first CALM row before any scientific block was admitted:

```text
FIRST_INVALID_COMPONENT=NEXT_STATUS_SOURCE_FRONTIER_UNAVAILABLE
previous_timestamp_us=81280000
recorded_failure_frontier=79896000
terminal=RuntimeError:next_status_timeout
```

Forensic established that a valid strict-future native successor existed. The old E8 mirror filtered it using additional health/authority fields that are not part of the canonical `next_status` predicate.

Canonical successor predicate remains:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

Root cause:

```text
native strict-future status exists
→ old E8 mirror health filter rejects it
→ observer cannot see contract-valid successor
→ bounded next_status lookup
→ next_status_timeout
```

Classification:

```text
NEXT_STATUS_FORENSIC_CLASS=B_STATUS_PUBLISHED_NOT_MIRRORED_FILTER_MISMATCH
CAPACITY_CAUSALITY=NOT_PROVEN
WATERMARK_VALIDITY=VALID
```

Canonical repair:

```text
native status
→ mirror.publish(native_status)
→ existing ingress/source-health/ACK gate
```

No timeout, QoS, timestamp, session/reset, treatment, control or scientific semantics changed.

Qualified root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_next_status_frontier_qualification_20260905_03
```

Qualified evidence:

```text
native callbacks=800
mirror callbacks=794
strict-future successor lookups=689
next_status timeouts=0
C1 records=19280
C1 missing lifecycle=0
writer errors/drops/gaps=0/0/0
graph_valid=true
forbidden_cycles=0
PRE_RETRY_VALID_CAUSAL_CORE=true
```

## Step 1 — Create exactly one new immutable scientific root

Create one new root under Kingston. Do not reuse or continue any previous scientific root.

Persist before execution:

```text
Git HEAD
worktree/dirty fingerprint
task/config version
manifest ID + SHA
PX4/world identity
trace configuration
E8 pairing version
native-CLEAR lifecycle version
next-status mirror/successor version
```

Historical roots `fresh_29` through `fresh_34` remain separate evidence and are never pooled.

## Step 2 — Full preflight

Require before the first scientific block:

```text
manifest identity PASS
PX4/world identity PASS
AURA PASS
C1 PASS
E8 source-causal pairing PASS
Direct Guard PASS
native-event CLEAR lifecycle PASS
next_status successor path PASS
status callback path PASS
continuous-C1 replay prerequisites PASS
H1000 prerequisites PASS
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
PRE_RETRY_VALID_CAUSAL_CORE=true
Kingston writable/capacity PASS
no stale PX4/Gazebo/task runtime PASS
```

If any preflight gate fails:

```text
SCIENTIFIC_EXECUTION=NOT_RUN
STOP
```

Do not repair inside the scientific root.

## Step 3 — Execute the frozen matrix exactly once

Run:

```text
8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
```

Start from block 1.

Forbidden inside the root:

```text
retry failed block
skip/replace block
resample arm
arm relabeling
hot-fix code
process restart and continue
QoS change
timeout increase
lifecycle semantic change
pooling with another root
```

Stop at the first invalid block.

## Step 4 — Keep the newly closed infrastructure observable

For each relevant `next_status` lookup retain:

```text
previous_timestamp_us
expected session
expected reset
observer retained count
oldest/newest retained timestamp
strict-future candidate count
selected successor timestamp
selected successor session/reset
timestamp_ready
lookup result
```

Also retain:

```text
native status callback count
mirror callback count
next_status lookup count
next_status timeout count
```

For every GUST block retain exact lifecycle identity and require:

```text
PREVIOUS_EVENT_STILL_ACTIVE_REJECTIONS=0
OVERLAPPING_NATIVE_EVENTS=0
RETIRE_WITHOUT_CLEAR=0
WRONG_CLEAR_ACCEPT=0
```

The callback counts are diagnostic; scientific acceptance depends on the required contract-valid successor/lifecycle observations, not arbitrary 1:1 callback-count equality unless a canonical contract explicitly requires it.

## Step 5 — Preserve existing infrastructure health

Keep visible:

```text
C1 replay missing lifecycle references
writer errors/drops/sequence gaps
E8 source-causal pairing
Direct Guard
H1000
source/session/reset continuity
projection/saturation diagnostics
```

Do not relax a gate to preserve progress.

## Step 6 — No partial science

Unless the root reaches:

```text
SESSIONS_VALID=8/8
BLOCKS_VALID=96/96
```

report:

```text
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

Do not calculate partial treatment effects and do not use apparently valid rows from an incomplete root as scientific dataset credit.

## Step 7 — Canonical reverse processing + Tarjan + peeling

Use the existing WM causal-validity engine; do not create a task-local replacement.

Required order:

```text
reverse validity indexing
→ canonical source-grounded dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid-seed assignment
→ fixed-point peeling
```

Require structurally:

```text
graph_valid=true
forbidden_cycles=0
```

For any failure retain the direct invalid seed and propagated causal path.

Do not fabricate PX4 source timestamps from host or Gazebo time.

## Step 8 — Complete-root scientific admission

Only after a complete valid 96/96 root, freeze the root and run a separate causal-dataset admission audit.

Audit at least:

```text
randomization integrity
CALM/GUST support
ZERO/P1/P2 support
assigned vs requested vs accepted action
T_D/T_A ordering
exact treatment exposure
H1000 completeness
native-event lifecycle
next_status integrity
source/session/reset continuity
H0/H20/H40/H80 response availability
washout/carryover
FAST/PX4 post-treatment mediation interpretation
constraint/projection/saturation
no prohibited post-treatment conditioning
response construction
```

Output only:

```text
CAUSAL_DATASET_ACCEPTANCE=PASS | FAIL | INSUFFICIENT
```

Primary treatment contrasts remain H40/H80; H0/H20/H40/H80 remain dataset-completeness horizons.

## Step 9 — G_action / World Model boundary

If and only if causal-dataset acceptance passes:

```text
READY_FOR_G_ACTION_IDENTIFICATION
```

World-Model training still requires separate owner authorization.

Forward ladder:

```text
complete valid randomized root
→ causal-dataset acceptance
→ G_action identification
→ action-conditioned World Model
→ WISE predictive refinement
→ latency/AoI/freshness characterization
→ uncertainty calibration
→ later FAST/CALE/AEGIS research
```

## FASTv2 research boundary

The current research candidate combining the main FAST path with PID-branch lessons is post-Phase-0 only:

```text
PX4 firmware PID unchanged
+
AURA disturbance feedforward (-d_hat)
+
T1/C1 continuation
+
bounded residual 2-DOF PI at the qualified acceleration-correction boundary
```

This is not part of the authorized next scientific root and must not change baseline `B` during the current randomized identification campaign.

## Hard boundaries

Never modify the experiment merely to obtain 96/96.

```text
failed root = immutable historical conclusion
partial valid rows ≠ scientific dataset
infrastructure failure ≠ scientific negative result
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

## Current final state

```text
OWNER_AUTHORIZED_NEW_FRESH_RANDOMIZED_PILOT_READY_TO_EXECUTE
```
