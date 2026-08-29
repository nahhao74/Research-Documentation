# VNEXT WM1 V2R1 — pilot bootstrap-only callsite repair and exact preflight

Date: 2026-08-29

## Decision

```text
PILOT_BOOTSTRAP_ONLY_CALLSITE_REPAIR_QUALIFIED_READY_FOR_OWNER_FRESH_PILOT_RETRY_REVIEW
```

This task performed only an implementation repair, deterministic checks and
one exact pre-science runner qualification.  No randomized scientific pilot
was launched, no scientific `T_D` froze, and no manifest slot or candidate
action was consumed.

## Immutable failed root

```text
FAILED_ROOT_STATUS=IMMUTABLE_STOPPED_PRE_SCIENCE
FAILED_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260829_statebank_aura_bootstrap_closure_01
ROOT_VALIDITY_CLASS=INVALID_INFRASTRUCTURE_PRE_SCIENCE
SCIENTIFIC_MONITORING_START=NOT_REACHED
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
MANIFEST_SLOTS_CONSUMED=0
FAILURE_BOUNDARY=PILOT_BOOTSTRAP_TRANSACTION_MISSING_BOOTSTRAP_ONLY
DEEP_ROOT_CAUSE=SPECIALIZED_PILOT_RUNNER_OMITS_EXISTING_BOOTSTRAP_ONLY_FLAG
ROOT_CAUSE_CLASS=IMPLEMENTATION_INFRASTRUCTURE
```

The root above remains immutable and is not resumed, relabeled, promoted or
pooled.  Its StateBank/AURA readiness evidence remains accepted: the seven
required streams were present and the bootstrap ACK was valid.  The failure
occurred when the specialized runner omitted the existing `bootstrap_only`
flag and entered the candidate/C1 frontier path before science.

The earlier StateBank/AURA closure remains the authority for the underlying
seven-stream contract and is not reopened.

## Implementation repair

```text
IMPLEMENTATION_REPAIR=EXPLICIT_BOOTSTRAP_ONLY_TRUE_AT_SPECIALIZED_RUNNER_CALLSITE_PLUS_BOOTSTRAP_RETURN_GUARD
SEMANTICS_CHANGED=false
```

`WithinRunRandomizedPilotProbe.run_session()` now passes
`bootstrap_only=True` on its session-start `block_index=-1` call.  The base
`Stage1Probe.run_transaction()` early return is therefore used immediately
after the accepted source-complete snapshot ACK; no action offer, candidate
authority, C1 offer-frontier wait, accepted status or scientific record is
created.  The specialized observer explicitly skips bootstrap-only
transactions because that base return intentionally has no `accepted_status`.
Normal scientific and excluded release calls do not pass the flag and retain
the default `bootstrap_only=False` behavior.

The first repaired preflight attempt was preserved as a separate diagnostic
root (`...bootstrap_only_preflight_01`); it exposed a follow-on
`TypeError` in the observer when handling the intentional bootstrap-only
`accepted_status=None`.  The observer guard and session-status scope were
fixed outside that root.  The final qualification below passed; this was an
orchestration/observation-path correction, not a C1, StateBank, source or
scientific defect.

## Static and storage preflight

```text
STATIC_PREFLIGHT=PASS
MANIFEST_BYTE_IDENTITY=PASS
MANIFEST_SHA256=253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7
KINGSTON_RW=PASS
WRITE_READ_PROBE=PASS
FREE_SPACE_BEFORE=86771499008 bytes (>=50 GiB)
AGENT_V6=OFF
STALE_FLIGHT_RUNTIME_PROCESSES=NONE_AFTER_CLEANUP
```

The static preflight used the real outer pilot runner and its frozen schedule
validator.  It returned `pass=true`, `errors=[]`, eight manifest rows and the
same-cycle prehook `PASS`.  The manifest was byte-read only.  It was not
regenerated, normalized, reordered or changed.

## Frozen source/build identities

```text
PILOT_PROBE_REPAIRED_SHA256=e9282fe90c89d52fabcd0db4c2cd2982d84968727d24f5485e880aa08400a204
PILOT_RUNNER_SHA256=ce118b9ca775768013b9c8c49228dc6d1e57bb1c0477a2ebaf9ecc181b33c460
STAGE1_PROBE_SHA256=29661d73c2682fb5efb90b0519d26462d7dd6aa0dcf0fab12801689194bda323
STAGE1_ACQUISITION_SHA256=6655005cb2948f9c0bf1d9d55e90323c5b8d1165280d1109b3d7461cd8be0d4d
V2_TEMPORAL_PROBE_SHA256=43a3ae1bd618d18e192a9a5b5e4ab7580e6daa160db766df48644a9c538ea648
V2_CONTRACT_SHA256=8e4652073dbdf15805bd595310fea125ca98f8af79303d678940127b558f5d0d
ACTION_LINK_SHA256=72762cf9f89a0937aa85d09df855a0cdc5d0a606a49ac97f8b574bf0403c17ac
STATEBANK_V1_NODE_SHA256=5d4eb113f8bef7b5f7c962f02b32aeb55e618bbca2c098d975a327d05de116c2
STATEBANK_AURA_BOOTSTRAP_QUALIFIER_SHA256=c50a93164720555af3c5ea38f398a0faded832075a9221c63c9a0593d4f19aa0
MOVING_RUNTIME_NODE_SHA256=2153a23fc8cb65d57ed9995f64fae8fdbebec34ca16d26fa1438b55d5e91af8e
E8_APPLIED_NODE_SHA256=11fe0f66b24c659edaf3c112f5e697c6226ae7b78525e072e2e2c32f2bf040a0
STRICT_SERIALIZER_SHA256=c76215db0672049da94135293705fbbc106bb65d8442653fd147fffd98b3a69b
PX4_EXECUTABLE_SHA256=18bd77a9ea60f5ea95c865fa48d794852ad21abc0bc9b1bfa7528981d386b284
PX4_BASE_COMMIT=85df8c2281c2466b30a121b22b0bf33dc69bcfe4
DDS_TOPICS_YAML_SHA256=83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0
GENERATED_DDS_TOPICS_H_SHA256=d39b1895f92f0792f3fe3a6d53375b9fc666c105c4d215cb51b242d377e3ff5a
SENSORCOMBINED_STAMPED_V1_MSG_SHA256=5cb31c66d07c9c2bdaa9a4a7c243f10e9e70423b28957dee87b10862a59d06
```

The important PX4/runtime identity ledger returned by the real outer
preflight was also retained in the qualification evidence.  No source drift
was found.

## `run_transaction()` callsite audit

```text
RUN_TRANSACTION_CALLSITE_AUDIT=PASS
```

| Source/call | Classification | `bootstrap_only` |
|---|---|---|
| specialized probe `run_session()` line 222, `block_index=-1`, session-start pattern | BOOTSTRAP | explicit `True` |
| specialized probe scientific call line 274, manifest `block_index>=0` | SCIENTIFIC_ZERO/P1/P2 | omitted, default `False` |
| specialized probe release call line 292, explicit accepted-zero release | OTHER | omitted, default `False` |
| base `wm1_stage1_probe.py` CLI pre-science call | BOOTSTRAP | explicit `True` |
| base `wm1_stage1_probe.py` scientific loop | SCIENTIFIC_ZERO/P1/P2 | omitted, default `False` |
| inherited V2 temporal post-reset bootstrap | OTHER legacy reset bootstrap | omitted, existing candidate-zero semantics retained |

The inherited V2 temporal session-start method is not used by the specialized
override.  Every actual scientific transaction in the specialized runner
therefore remains on the normal candidate path; no scientific call inherits
bootstrap-only behavior.

## Deterministic regression

```text
REGRESSION_TEST_RESULT=PASS_51
PY_COMPILE=PASS
GIT_DIFF_CHECK=PASS
```

Command:

```text
source /opt/ros/jazzy/setup.bash
source /home/nahhao74/px4_ros2_ws/install/setup.bash
export PYTHONPATH=.:1_AURA:3_AEGIS:${PYTHONPATH}
python3 -m pytest -q \
  1_AURA/tests/test_wm1_v2r1_pilot_bootstrap_callsite.py \
  1_AURA/tests/test_statebank_v1.py \
  1_AURA/tests/test_sensorcombined_provenance_wire.py \
  1_AURA/tests/test_wm1_stage1_acquisition.py \
  1_AURA/tests/test_wm1_action_link.py
```

The specialized-runner tests use call tracing rather than source-text
matching.  They assert that the `-1` call carries `bootstrap_only=True`, the
accepted snapshot returns immediately, no offer/publish/C1 wait/E8/T_D path
is entered, and a normal `block_index=0` call retains `False` behavior.  The
base transaction test supplies a complete seven-stream ACK, fails the test
if `ActionLinkTransaction.offer_action()` is called, and asserts the actual
bootstrap early return.  An invalid seven-stream ACK still fails closed as
`required_streams_not_ready`.

## Exact live runner preflight

```text
LIVE_EXACT_RUNNER_PREFLIGHT=PASS_FINAL_ROOT
RUNTIME_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260829_bootstrap_only_preflight_02
LIVE_ROOT_VALIDITY_CLASS=NONSCIENTIFIC_PRE_SCIENCE_QUALIFICATION_PASS
```

This was executed through the actual `wm1_stage1_acquisition._run_row`
orchestration with
`probe_module=aura_data_acquisition.wm1_v2r1_within_run_randomized_pilot_probe`,
StateBank readiness enabled, and an empty diagnostic schedule.  The prior
`...preflight_01` root that caught the follow-on observer issue is retained
immutable and is not included as a passing root.

```text
RUNTIME_SOURCE_COUNTER_ATTESTATION=PASS
STATEBANK_BOOTSTRAP_READINESS=PASS
STATEBANK_REQUIRED_STREAMS_PRESENT=7/7
AURA_STREAM_PRESENT=PASS
BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED
SNAPSHOT_BARRIER_RECHECK=PASS
SNAPSHOT_CAUSALITY=PASS
BOOTSTRAP_ONLY_FLAG_LIVE=PASS
BOOTSTRAP_RETURNED_BEFORE_CANDIDATE_PATH=true
CANDIDATE_OFFER_COUNT=0
CANDIDATE_PUBLISH_COUNT=0
C1_OFFER_FRONTIER_WAIT_COUNT=0
E8_CANDIDATE_TRANSACTION_COUNT=0
FIRST_SCIENTIFIC_T_D=NOT_REACHED
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
MANIFEST_SLOTS_CONSUMED=0
CLEANUP=PASS
```

The exact probe transaction is retained at
`STATEBANK_BOOTSTRAP_A_CALM_R1/probe_transactions.json` with
`block_index=-1`, `bootstrap_only=true`, `state=ACKED`, `accepted_status=null`,
`action_offer=null`, `accepted_cycle_count=0`,
`scientific_record=false`, and
`excluded_from_scientific_records=true`.  The wrapper result at
`exact_runner_preflight_result.json` is `state=VALID`, `errors=[]`,
`transaction_count=1`, `schedule_count=0`, `action_offer=false`, and
`snapshot_ack=true`.

The lifecycle evidence records, in one StateBank session identity, AURA
subscription discovery, AURA callback/ingest, the `block_index=-1` snapshot
request, barrier close, all-seven-stream presence, and accepted snapshot ACK.
The accepted ACK is snapshot 71, with required stream presence `[]` missing.

## Live source/provenance evidence

```text
SOURCE_CONTINUITY_RESULT=PASS_PRE_SCIENCE
CLOCK_ALIGNMENT_RESULT=PASS_PRE_SCIENCE
ATOMIC_PROVENANCE_RESULT=PASS_PRE_SCIENCE
SENSOR_PIPELINE_INSTRUMENTATION=true
SC_P3=1228->1428
SC_X1_POLL=1->202
SC_X1_COPY=1->202
SC_X2_PREPARE=1->202
SC_X2_SEND=1->202
SC_X2_FLUSH=1->202
AURA_CALLBACK=1->201
NATIVE_SOURCE_MAX_GAP_US=8000
NATIVE_SOURCE_GAP_GT_20MS=0
SOURCE_GENERATION_DISCONTINUITY_COUNT=0
CLOCK_MAPPING_EPOCH_TRANSITION_COUNT=0
CLOCK_ALIGNMENT_INVALID_COUNT=0
ATOMIC_PROVENANCE_INVALID_COUNT=0
UXRCE_PREPARE_FAILURES=0
WRITER_ERRORS=0
WRITER_DROPS=0
SEQUENCE_GAPS=0
```

The trace summary records `sensor_pipeline_instrumentation=true`,
`writer_errors=0`, `writer_drops=0`, `sequence_gaps=0`, and a clean stopped
runtime.  No `probe_offer_evidence.jsonl` or timeout evidence was produced.
No `e8_ingress_evidence` record contains a published candidate arm or
authority.

## Scientific accounting and authority

```text
SESSIONS_PLANNED=8
SESSIONS_COMPLETED=0
SESSIONS_VALID=0/8
BLOCKS_PLANNED=96
BLOCKS_COMPLETED=0
BLOCKS_VALID=0/96
ZERO_EXPOSURE=0/48_NOT_STARTED
P1_EXPOSURE=0/24_NOT_STARTED
P2_EXPOSURE=0/24_NOT_STARTED
E8_RESULT=NOT_REACHED_PRE_SCIENCE
RELEASE_CLOSURE=NOT_REACHED_PRE_SCIENCE
H1000_RESULT=NOT_REACHED_PRE_SCIENCE
TARGET_H40_VALIDITY=NOT_RUN_PRE_SCIENCE
TARGET_H80_VALIDITY=NOT_RUN_PRE_SCIENCE
```

No statistical result, efficacy interpretation or treatment claim exists.

```text
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED_IN_THIS_TASK;READY_FOR_OWNER_REVIEW_ONLY
SEALED_STATE=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED
R1=NOT_AUTHORIZED
production_authority=false
NEXT_STATE=READY_FOR_OWNER_FRESH_PILOT_RETRY_REVIEW
```

The decision above is a readiness result for the owner; it is not an
automatic pilot launch or a production-authority grant.
