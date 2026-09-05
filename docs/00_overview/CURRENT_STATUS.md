# Current Status — 2026-09-05

## Executive state

The main AURA–WISE–WM–AEGIS pipeline remains active. Phase-0 Option-B, reverse/peeling validity, continuous-C1 replay, post-reset E8 source-causal pairing, native-event CLEAR lifecycle, and the latest `next_status` successor-frontier repair have all reached bounded non-scientific qualification.

The most recent scientific-root attempt, `fresh_34`, remains an immutable infrastructure-invalid historical result. Its failure was traced to a status-mirror contract mismatch, repaired in the canonical E8 mirror, and then qualified separately with 689 strict-future successor lookups and zero timeouts. A new fresh randomized scientific pilot is owner-authorized but has not yet been executed.

```text
MAIN_PIPELINE=ACTIVE
PID_BENCHMARK_TRACK=RESEARCH_REFERENCE_ONLY

PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID
PHASE_0B5_QUALIFICATION=VALID_BOUNDED_NOSCIENCE

OPTION_B_CONTRACT_READY=true
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
OPTION_B_LIVE_RUNTIME_QUALIFIED=true
OPTION_B_DOWNSTREAM_TRANSACTION_INTEGRATION=QUALIFIED_NOSCIENCE

RESET_AUTHORITY=AURA_C1_SOURCE_RESET
M_STABLE_US=100000
W_MAX_US=1000000
EVENT_SCHEDULING_POLICY=DELAY_RESCHEDULE_WITHIN_BLOCK
FAIL_CLOSED_TIMEOUT=true
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US

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
FRESH34_RAW_REPLAY=AVAILABLE_AT_CURRENT_ROOT_PATH
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
AEGIS_AUTHORITY=0
ROUND_Z_AUTHORITY=0
production_authority=false

OWNER_FRESH_RANDOMIZED_PILOT_AUTHORIZED=true
NEW_FRESH_RANDOMIZED_PILOT_EXECUTED=false
AUTHORIZE_0B5_RERUN=false
AUTHORIZE_WM_TRAINING=false
AUTHORIZE_SEALED_OPEN=false

NEXT_STATE=OWNER_AUTHORIZED_NEW_FRESH_RANDOMIZED_PILOT_READY_TO_EXECUTE
```

## Frozen scientific target

The estimand remains unchanged:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The following remain frozen for the next scientific root:

```text
AURA semantics
FAST/T1/C1 semantics
Direct Guard
M_STABLE_US=100000
W_MAX_US=1000000
PX4_BOOT_US timing authority
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
T_D / T_A
H1000 semantics
P1/P2 treatment profiles
ZERO semantics
randomization
CALM/GUST context semantics
SEALED boundary
production authority
```

Current authoritative pilot identity:

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

## Canonical infrastructure closures

### Reverse processing + Tarjan + peeling

The offline `WM_CAUSAL_VALIDITY_ENGINE` remains mandatory where applicable:

```text
raw/source-grounded evidence
→ reverse validity index
→ canonical causal dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

Canonical graph shape is 21 nodes / 34 edges. Tri-state precedence is:

```text
FAIL > UNKNOWN > PASS
```

This engine remains preflight/offline/post-run validation only and never enters the AURA/AEGIS/PX4 control hot path.

### Continuous-C1 replay/recovery

The qualified diagnostic trace transport remains:

```text
TRACE_QOS_DEPTH=4096
TRACE_QOS_RELIABILITY=RELIABLE
TRACE_QOS_DURABILITY=VOLATILE
TRACE_QOS_ROLE=DIAGNOSTIC_EVIDENCE_ONLY
```

Replay-critical evidence has demonstrated zero missing replayable C1 lifecycle references under the qualified path.

### Post-reset E8 source-causal pairing

E8 uses a bounded immutable AURA callback ledger (`maxlen=4096`) for decision-authoritative AURA/C1 pairing. For a C1 source frontier, the selector uses the newest received positive-source nonfuture AURA record and then applies the existing exact session/reset/validity/freshness/provenance/clock gates.

Hard rule:

```text
newer invalid/reset-mismatched AURA
must not be skipped for an older favorable AURA record
```

### Native-event CLEAR lifecycle

The `fresh_33` inter-block overlap failure was repaired without changing frozen within-block scientific timing.

Canonical invariant:

```text
arm
→ consume/onset
→ exact native CLEAR
→ clear
→ complete
→ retire
→ next block may arm
```

Qualified root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_native_event_clear_lifecycle_qualification_20260905_01
```

Qualification result:

```text
3/3 consecutive GUST blocks PASS
PREVIOUS_EVENT_STILL_ACTIVE_REJECTIONS=0
OVERLAPPING_NATIVE_EVENTS=0
INTER_BLOCK_CLEAR_GATE=PASS_3_OF_3
PRE_RETRY_VALID_CAUSAL_CORE=true
SCIENTIFIC_SEMANTIC_DELTA=NONE
```

Historical `fresh_33` classification remains immutable. Its raw root was later owner-deleted during storage cleanup, so historical replay is unavailable; retained canonical summaries remain the authority for that failed attempt.

## Latest scientific-root attempt — fresh_34

Root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_within_run_randomized_action_20260905_fresh_34
```

Provenance:

```text
Git HEAD=a6cebe4d8f1e99a941ab7886efdd7d6a446143c3
Manifest SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
```

Preflight passed, including:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
Direct Guard PASS
E8 source-causal pairing PASS
native-event CLEAR lifecycle PASS
continuous-C1 replay prerequisites PASS
```

Runtime stopped fail-closed in the first row:

```text
row=WM1V2R1_RAND_A_CALM_R1
sessions valid=0/8
scientific blocks valid=0/96
FIRST_INVALID_COMPONENT=NEXT_STATUS_SOURCE_FRONTIER_UNAVAILABLE
previous_timestamp_us=81280000
recorded_failure_frontier=79896000
terminal=RuntimeError:next_status_timeout
```

No GUST, candidate, `T_D`, ACK, exposure, H1000 completion, manifest slot or SEALED access occurred.

C1 replay remained healthy:

```text
records=19630
missing lifecycle references=0
writer errors/drops/sequence gaps=0/0/0
```

Reverse/peeling remained graph-valid with zero forbidden cycles, but `pre_retry_valid_causal_core=false` for the failed root because the timeout is an infrastructure invalid seed and downstream nodes peel away.

Final historical classification:

```text
FRESH_RANDOMIZED_G_ACTION_PILOT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

No partial row from `fresh_34` is scientific evidence.

## fresh_34 root cause — next-status mirror contract mismatch

Forensic reconstructed the exact successor predicate:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

For the failed request:

```text
previous_timestamp_us=81280000
session=360000
reset=1
```

The native publisher had 4568 same-lineage records before the observer deadline and 338 strict-future candidates satisfying the lookup predicate. The first qualifying strict-future native status mapped to `timestamp_us=81300000` and was ready with the expected session/reset.

The old E8 mirror applied additional filtering before publication:

```text
source_valid
source_fresh
gate_valid
applied_authority
```

Those fields are not part of the canonical `next_status` successor predicate. Therefore the first implementation divergence was:

```text
native strict-future status exists
→ old E8 mirror health filter rejects it
→ observer cannot see a contract-valid successor
→ bounded next_status lookup
→ host-only next_status_timeout
```

Canonical classification:

```text
NEXT_STATUS_FORENSIC_CLASS=B_STATUS_PUBLISHED_NOT_MIRRORED_FILTER_MISMATCH
CAPACITY_CAUSALITY=NOT_PROVEN
WATERMARK_VALIDITY=VALID
MIXED_DOMAIN_SENSOR_SAMPLE=INDEPENDENT
```

## next-status repair and qualification

The canonical repair was limited to:

```text
native status
→ mirror.publish(native_status)
→ existing ingress/source-health/ACK gate
```

No timeout, QoS policy, timestamp predicate, session/reset, treatment, control or scientific semantic changed.

```text
SCIENTIFIC_SEMANTIC_DELTA=NONE
INFRASTRUCTURE_RETENTION_ONLY
```

Qualified root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_next_status_frontier_qualification_20260905_03
```

Qualification result:

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

`status_observer.enabled=false` in this observer-only qualification means the task used the main ROS executor rather than the dedicated background observer thread; the callback path itself was active and error-free.

Canonical state:

```text
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
NEXT_STATUS_SUCCESSOR_FRONTIER_PASS_READY_FOR_OWNER_PILOT_REVIEW
```

## Current owner decision — new fresh pilot authorized

The owner has authorized exactly one new fresh randomized scientific pilot using the frozen manifest and all currently qualified infrastructure unchanged.

Authorized:

```text
one new immutable scientific root
full preflight
8 sessions / 96 blocks
stop on first invalid block
canonical reverse/Tarjan/peeling validation
```

Not authorized:

```text
0B.5 rerun
retry/skip/replace/resample inside a root
hot-fix while a scientific root is running
partial-root scientific analysis
World-Model training
SEALED opening
production authority
```

The next root must begin from block 1. Unless 8/8 sessions and 96/96 blocks are valid, report:

```text
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

If 96/96 completes validly, freeze the root and perform a separate causal-dataset admission audit before any `G_action` identification or WM training.

## Immediate execution ladder

See:

[`CURRENT_EXECUTION_LADDER_WM_20260905.md`](CURRENT_EXECUTION_LADDER_WM_20260905.md)

Current high-level sequence:

```text
1. Create one new immutable scientific root; preserve frozen manifest and infrastructure versions
2. Run full preflight, including next-status, native-CLEAR, E8, C1, Direct Guard, H1000 and validity-engine gates
3. Execute exactly 8 sessions / 96 blocks; stop on first invalid block
4. No retry/skip/replace/resample/hot-fix/pooling
5. Persist next-status/native/mirror diagnostics and all source/session/reset identities
6. Run canonical reverse index → graph → Tarjan → peeling
7. If incomplete/invalid: classify root; no partial science
8. If 96/96 valid: freeze root and run separate causal-dataset admission audit
9. Only after admission PASS may `G_action` identification be considered
10. World-Model training still requires separate owner authorization
```

## Post-Phase-0 FAST research direction — not current authority

Research using the independent PID benchmark branch has identified a plausible future FASTv2 challenger without modifying PX4 firmware PID:

```text
current PX4 PID/inner loops remain untouched
+
current AURA disturbance feedforward (-d_hat)
+
current T1/C1 continuity
+
bounded residual 2-DOF PI at the qualified acceleration-correction boundary
```

Working hypothesis:

```text
a_FASTv2 = -d_hat + a_T1/C1 + a_residual_2DOF_PI
```

The first candidate should use fixed robust PI gains, anti-windup, bounded output and source/lifecycle-aware integrator handling. Gain scheduling, INDI, DOB/ESO, ANN/RBF and any T1/C1 semantic reduction remain later ablation candidates only.

This is a research direction only. It must not alter the current Phase-0 baseline `B` or the next randomized scientific pilot.

## World-Model / WISE dependency

Action-conditioned World-Model training remains blocked until one complete valid randomized causal dataset is accepted.

```text
complete randomized root
→ VALID_CAUSAL_CORE
→ causal-dataset acceptance
→ G_action identification
→ World Model
→ WISE predictive refinement
→ AEGIS integration
```

Do not train from partial or infrastructure-invalid roots.

## Storage and authority boundaries

Large runtime/capture/dataset/training/intermediate artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

Historical failed-root conclusions remain immutable even if the owner later deletes raw data for storage management.

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
MODEL_TRAINING_ACTION_RESPONSE=BLOCKED_PENDING_CAUSAL_DATA_ACCEPTANCE
production_authority=false
```
