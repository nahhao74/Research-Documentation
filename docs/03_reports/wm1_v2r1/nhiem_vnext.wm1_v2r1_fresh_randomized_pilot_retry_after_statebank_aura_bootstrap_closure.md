# VNEXT WM1 V2R1 — fresh randomized pilot after StateBank/AURA bootstrap closure

Date: 2026-08-29

## Final decision

```text
VNEXT_WM1_V2R1_RANDOMIZED_PILOT_INVALID_INFRASTRUCTURE_PRE_SCIENCE
```

Exactly one owner-authorized fresh root was attempted.  It stopped before the
first scientific `T_D`; no automatic retry was launched.

## Root and scope

```text
RUNTIME_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260829_statebank_aura_bootstrap_closure_01
FAILED_ROOT_STATUS=IMMUTABLE_STOPPED_PRE_SCIENCE
ROOT_VALIDITY_CLASS=INVALID_INFRASTRUCTURE_PRE_SCIENCE
ROOT_CAUSE_CLASS=IMPLEMENTATION_INFRASTRUCTURE
ROOT_CAUSE=PILOT_BOOTSTRAP_RUN_TRANSACTION_OMITS_BOOTSTRAP_ONLY_FLAG_AND_ENTERED_CANDIDATE_OFFER_PATH
EARLIEST_INVALIDATING_EVENT=PRE_SCIENCE_CALM_R1_BOOTSTRAP_C1_VALID_OFFER_FRONTIER_TIMEOUT_AFTER_VALID_7_STREAM_SNAPSHOT_ACK
SCIENTIFIC_MONITORING_START=NOT_REACHED
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
MANIFEST_SLOTS_CONSUMED=0
```

The stopped root is retained as immutable evidence.  The prior atomic-
provenance root and all earlier roots remain separate, immutable and unpooled.
No treatment, efficacy, E0/E1/E2 or scientific interpretation is made.

## Preflight and frozen identities

```text
PREFLIGHT_RESULT=PASS
STATIC_PREFLIGHT=PASS
MANIFEST_BYTE_IDENTITY=PASS
MANIFEST_SHA256=253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7
SOURCE_IDENTITY_ATTESTATION=PASS
SENSORCOMBINED_STAMPED_V1_INTERFACE=PASS
ATOMIC_PROVENANCE_ATTESTATION=PASS_PRE_SCIENCE
PILOT_TIMESTAMP_BINDING_PREFLIGHT=PASS_STATIC_RETAINED
STATEBANK_BOOTSTRAP_REPAIR_ATTESTATION=PASS_LIVE
STRICT_JSON_PREFLIGHT=PASS_RETAINED
QUALIFIER_PREFLIGHT=PASS_RETAINED
E8_PENDING_ACK_PREFLIGHT=PASS_RETAINED
H1000_PREFLIGHT=PASS_RETAINED
NO_SYNCHRONOUS_HOT_PATH_DIAGNOSTICS_REGRESSION=PASS
AGENT_V6=OFF
```

The frozen campaign remains worker A, 8 sessions (4 CALM/4 GUST_E), 12 blocks
per session and 96 blocks total; the manifest was read only as the frozen
schedule and no scientific slot was consumed.

SOURCE_BUILD_IDENTITIES=FROZEN_EXACT_HASH_LEDGER

```text
PX4_EXECUTABLE_SHA256=18bd77a9ea60f5ea95c865fa48d794852ad21abc0bc9b1bfa7528981d386b284
PX4_BASE_COMMIT=85df8c2281c2466b30a121b22b0bf33dc69bcfe4
DDS_TOPICS_YAML_SHA256=83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0
GENERATED_DDS_TOPICS_H_SHA256=d39b1895f92f0792f3fe3a6d53375b9fc666c105c4d215cb51b242d377e3ff5a
STATEBANK_V1_PY_SHA256=4ca712bf27360c533aafbaaf0f9c5a3fe06566b9cdaa35b42930a17ac1ee3f61
STATEBANK_V1_NODE_PY_SHA256=5d4eb113f8bef7b5f7c962f02b32aeb55e618bbca2c098d975a327d05de116c2
MOVING_RUNTIME_NODE_PY_SHA256=2153a23fc8cb65d57ed9995f64fae8fdbebec34ca16d26fa1438b55d5e91af8e
STAGE1_PROBE_PY_SHA256=29661d73c2682fb5efb90b0519d26462d7dd6aa0dcf0fab12801689194bda323
STAGE1_ACQUISITION_PY_SHA256=6655005cb2948f9c0bf1d9d55e90323c5b8d1165280d1109b3d7461cd8be0d4d
PILOT_RUNNER_PY_SHA256=ce118b9ca775768013b9c8c49228dc6d1e57bb1c0477a2ebaf9ecc181b33c460
STRICT_SERIALIZER_PY_SHA256=c76215db0672049da94135293705fbbc106bb65d8442653fd147fffd98b3a69b
E8_APPLIED_PY_SHA256=11fe0f66b24c659edaf3c112f5e697c6226ae7b78525e072e2e2c32f2bf040a0
STAGE1_RUNTIME_SHA256=e89e3c88f48c2b300ca36664410cd680ce629656c40edd7b860bcd213d4d4e4c
ACTION_LINK_SHA256=72762cf9f89a0937aa85d09df855a0cdc5d0a606a49ac97f8b574bf0403c17ac
SENSORCOMBINED_STAMPED_V1_MSG_SHA256=5cb31c66d07c9c2bdaa9a4a7c243f10e9e70423b28957dee87b10862a59d06
```

Storage preflight was clean: `KINGSTON_RW=PASS`,
`WRITE_READ_PROBE=PASS`, `FREE_SPACE_BEFORE=92923232256` bytes and
`FREE_SPACE_AFTER=90003734528` bytes (both above the 50-GiB requirement).
No stale flight/runtime process remained after cleanup; row cleanup returned
zero.

## Live source and provenance attestation

```text
RUNTIME_SOURCE_COUNTER_ATTESTATION=PASS_PRE_SCIENCE
SENSOR_PIPELINE_INSTRUMENTATION=true
SC_P3=1231->1431
SC_X1_POLL_COPY=1->201
SC_X2_PREPARE_SEND_FLUSH=1->201
AURA_SC_CALLBACK=1->201
NATIVE_SOURCE_MAX_GAP_US=8000
NATIVE_SOURCE_GAP_GT_20MS=0
SOURCE_GENERATION_DISCONTINUITY_COUNT=0
CLOCK_MAPPING_EPOCH_TRANSITION_COUNT=0
CLOCK_ALIGNMENT_INVALID_COUNT=0
ATOMIC_PROVENANCE_INVALID_COUNT=0
UXRCE_PREPARE_FAILURES=0
WRITER_ERRORS=0
WRITER_DROPS=0
```

The live StateBank sample carried `WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V2`,
native source timestamp/generation, exact sender mapping generation/epoch/
offset and a valid session/reset identity.  Agent `-v6` stayed off and no
synchronous hot-path diagnostics were restored.

## Bootstrap readiness evidence

The repaired StateBank/AURA bootstrap path itself passed in this root:

```text
STATEBANK_BOOTSTRAP_REPAIR_ATTESTATION=PASS_LIVE
STATEBANK_BOOTSTRAP_READINESS=PASS
STATEBANK_REQUIRED_STREAMS_PRESENT=7/7
REQUIRED_STREAMS=aura,imu,attitude,local_state,reference,controller,actuator
AURA_STREAM_PRESENT=PASS
SNAPSHOT_BARRIER_RECHECK=PASS
SNAPSHOT_CAUSALITY=PASS
BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED_SNAPSHOT_275
```

Retained lifecycle order (host monotonic values as logged) was:

| Event | Evidence |
|---|---:|
| StateBank process start | `1506033656123` |
| AURA subscription discovered | `1506059023974`, `/uav_a/aura/disturbance_state`, `DiagnosticArray` |
| First AURA callback/ingest | `1506783812742` / `1506783953756`, source generation `1823` |
| Bootstrap snapshot request | `1519798461008`, `block_index=-1` |
| Snapshot barrier close | `1519811620774`, all-stream barrier sequence `12284` |
| Stream presence | `1519811657274`, all seven true, missing `[]` |
| Snapshot ACK | `1519830061188`, `ack_valid=true`, `reason=accepted` |

Thus `snapshot_stream_missing:aura` was not reproduced.  The AURA stream was
mandatory and source-bound; its initial W20-invalid status was not promoted to
scientific validity.

## Failure reconstruction and exact cause

The specialized runner calls the bootstrap transaction in
`1_AURA/aura_data_acquisition/wm1_v2r1_within_run_randomized_pilot_probe.py`
at lines 215–220 without `bootstrap_only=True`:

```python
bootstrap = self.run_transaction(
    row_id=row_id, block_index=-1, cycle_index=0,
    pattern="SESSION_START_TAU_BOOTSTRAP_ZERO_TRANSACTION", action=(0.0, 0.0),
    explicit_zero=True, accepted_cycles=1, zero_category=None,
    pre_offer_callback=self._bootstrap_pre_offer,
)
```

The base `wm1_stage1_probe.py` contains the intended `if bootstrap_only`
return at lines 403–412.  Because the flag is omitted, execution falls
through to the candidate-arm path (lines 413–446), arms an offer and waits for
a valid C1 frontier.  The run then raises:

```text
Stage1ProbeProcessExit
phase=stage1_probe
exit_code=1
RuntimeError:c1_valid_offer_frontier_timeout
```

The runtime row had `continuous_c1_reason_counts={"RESET":1,
"TIMESTAMP_INVALID":14860}` and no candidate was published:
`published_arm=null`, `published_authority=null`.  This timeout is therefore
the consequence of entering the wrong bootstrap transaction path, not a
StateBank/AURA readiness failure.  `statebank_process_exit=1` occurred during
cleanup after the probe abort:

```text
STATEBANK_EXIT_CAUSAL_ROLE=CONSEQUENCE_OF_PROBE_ABORT
FAILURE_BOUNDARY=PILOT_BOOTSTRAP_TRANSACTION_MISSING_BOOTSTRAP_ONLY
```

The smallest future implementation repair is to pass `bootstrap_only=True`
for this pre-science transaction and add a regression that asserts the
bootstrap returns after its accepted snapshot without arming a candidate.
That repair was deliberately **not** applied in this owner-authorized root;
there was no patch-and-continue and no automatic second root.

## Campaign accounting

```text
SESSIONS_PLANNED=8
SESSIONS_ATTEMPTED=1 (CALM-R1)
SESSIONS_COMPLETED=0
SESSIONS_VALID=0/8
BLOCKS_PLANNED=96
BLOCKS_COMPLETED=0
BLOCKS_VALID=0/96
```

The `block_index=-1` bootstrap is excluded from scientific exposure.  No
scientific `T_D`, candidate action, randomized treatment, release, H1000 or
target record was reached:

```text
ZERO_EXPOSURE=0/48_SCIENTIFIC_BLOCKS_NOT_STARTED_BOOTSTRAP_EXCLUDED
P1_EXPOSURE=0/24_NOT_STARTED
P2_EXPOSURE=0/24_NOT_STARTED
E8_RESULT=NOT_REACHED_PRE_SCIENCE
RELEASE_CLOSURE=NOT_REACHED_PRE_SCIENCE
H1000_RESULT=NOT_REACHED_PRE_SCIENCE
TARGET_H40_VALIDITY=NOT_RUN_PRE_SCIENCE
TARGET_H80_VALIDITY=NOT_RUN_PRE_SCIENCE
STRICT_JSON_SESSION_PERSISTENCE=NOT_REACHED_PRE_SCIENCE
SAFETY_RESULT=PASS_NO_VIOLATION
```

No scientific statistics or signals were produced:

```text
E0_RESULT=NOT_RUN_PRE_SCIENCE
E1_RESULT=NOT_RUN_PRE_SCIENCE
BOOTSTRAP_RESULT=NOT_RUN_PRE_SCIENCE
E2_RESULT=NOT_RUN_PRE_SCIENCE
P1_SIGNAL=NOT_APPLICABLE
P2_SIGNAL=NOT_APPLICABLE
```

## Authority state

```text
IMPLEMENTATION_REPAIR=NOT_APPLIED_IN_THIS_ROOT;PILOT_BOOTSTRAP_ONLY_FLAG_FIX_REQUIRED_OUTSIDE_IMMUTABLE_ROOT
REGRESSION_TEST_RESULT=PASS_47_PRE-RUN_RETAINED
PILOT_TIMESTAMP_BINDING_PREFLIGHT=PASS_STATIC_RETAINED
SEALED_STATE=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED
R1=NOT_AUTHORIZED
production_authority=false
NEXT_STATE=OWNER_REVIEW_PILOT_BOOTSTRAP_TRANSACTION_BUG_NO_AUTOMATIC_RETRY
```

## Decision

```text
VNEXT_WM1_V2R1_RANDOMIZED_PILOT_INVALID_INFRASTRUCTURE_PRE_SCIENCE
```

The StateBank/AURA readiness repair is live-proven, but the fresh pilot root
is not certified because the runner's bootstrap transaction omitted the
bootstrap-only guard.  The stopped evidence is not a treatment failure, source
failure, E8 failure, target failure or efficacy result.
