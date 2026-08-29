# VNEXT WM1 V2R1 — StateBank/AURA pre-science bootstrap root-cause closure

Date: 2026-08-29

## Decision

```text
STATEBANK_AURA_BOOTSTRAP_ROOT_CAUSE_CLOSED_REPAIR_8SESSION_QUALIFIED_READY_FOR_OWNER_PILOT_RETRY_REVIEW
```

This closure covers only the pre-science StateBank/AURA bootstrap defect.  No
randomized manifest slot, scientific `T_D`, candidate action, training, R1 or
SEALED payload was consumed.  The failed scientific root remains immutable and
is not promoted or pooled.

## Failed root and retained boundary

```text
FAILED_ROOT_STATUS=IMMUTABLE_STOPPED_PRE_SCIENCE
FAILED_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260828_atomic_provenance_pilot_01
ROOT_VALIDITY_CLASS=INVALID_INFRASTRUCTURE_PRE_SCIENCE
FAILURE_BOUNDARY=STATEBANK_SNAPSHOT_ACK_MISSING_REQUIRED_AURA_STREAM
DEEPEST_PROVEN_BOUNDARY=AURA_PUBLISHED_AFTER_SNAPSHOT_BARRIER
DEEP_ROOT_CAUSE=BOOTSTRAP_SNAPSHOT_REQUEST_PRECEDES_STATEBANK_REQUIRED_STREAM_READINESS
DEEP_ROOT_CAUSE_CLASS=IMPLEMENTATION_INFRASTRUCTURE
SCIENTIFIC_MONITORING_START=NOT_REACHED
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
STATIC_PREFLIGHT=PASS
```

The original probe error was `ValueError:snapshot_stream_missing:aura` at
`block_index=-1`.  The immutable acknowledgement at snapshot 287 had
`aura.sample_count=0` while the other source streams were present.  The
retained StateBank log then records its first AURA callback/ingest only after
the failed barrier: snapshot 501 contains a source-bound sample received at
host monotonic `7,387,222,131,926 ns`, native source `33,428,000 us`,
generation `6655`, with `clock_alignment_valid=true` (W20 was not ready, so
the sample was intentionally not a valid scientific sample).  The AURA
runtime was alive and receiving the stamped wrapper; its first logged accepted
wrapper batch is `cb=5398` at host monotonic `7,387,728,624,751 ns`.  Thus the
evidence is a startup/readiness ordering race, not proof that AURA or the
source/uXRCE path was absent.

The failed-root process chronology also shows the probe/row finalization after
runtime cleanup.  `statebank_runtime.log` was empty because its stream was
buffered; no StateBank traceback or lifecycle log was retained.  The observed
StateBank return code `1` occurred during cleanup after the probe exception and
is therefore a consequence of probe abort, not the cause of the missing AURA
stream.

## Snapshot contract audit

```text
SNAPSHOT_REQUIRED_STREAM_CONTRACT=CLOSED
REQUIRED_STREAMS=aura,imu,attitude,local_state,reference,controller,actuator
AURA_REQUIRED_IN_STATEBANK_CORE=true
AURA_REQUIRED_IN_SNAPSHOT_ACK_PRODUCER=true
AURA_REQUIRED_IN_WM1_ACTION_LINK_VALIDATOR=true
AURA_REQUIRED_IN_PILOT_BOOTSTRAP=true
```

`aura/statebank_v1.py` and `wm1_action_link.py` use the same seven-stream
contract.  `validate_snapshot_ack()` rejects a missing or interpolated stream
and raises `snapshot_stream_missing:<stream>`.  The repair does not make AURA
optional and does not promote an invalid/W20-not-ready sample to a valid
scientific sample.  Bootstrap readiness checks source-bound presence and
identity; the normal snapshot/qualification validators still enforce
`StateSample.valid`.

## AURA output and exact binding

```text
AURA_OUTPUT_TOPIC=/uav_a/aura/disturbance_state
AURA_OUTPUT_MESSAGE_TYPE=diagnostic_msgs/msg/DiagnosticArray
AURA_OUTPUT_QOS=RELIABLE/VOLATILE
AURA_FIRST_PUBLISH_CONDITION=accepted stamped SensorCombined source identity; runtime-gate invalid output is still source-bound
AURA_TO_STATEBANK_BINDING=EXACT
```

`moving_runtime_node.py` subscribes the versioned stamped wrapper and accepts a
sample only after the native timestamp/generation and mapping tuple pass the
existing mapper/guard.  It publishes the disturbance state on the topic above
with the native source timestamp, generation and V2 provenance fields.
`statebank_v1_node.py::_aura_callback()` subscribes that exact topic and
message type, extracts `native_source_timestamp_us` (with the existing
source-bound fallback fields), preserves source/session/reset/mapping
provenance, and ingests under stream key `aura`.  No cross-topic or nearest
neighbour join is used.

The `_04` qualification exposed and repaired a separate adapter defect:
the stamped wrapper wire timestamp is agent-epoch while
`raw_representation=1` describes the native PX4 representation.  The
implementation now derives `raw_timestamp_domain` from the wire value, while
leaving native source values unchanged.  This is a mechanical prerequisite,
not a StateBank or timestamp-contract reinterpretation.

The exact source/build identities used by the final qualification are:

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
BOOTSTRAP_QUALIFICATION_PY_SHA256=c50a93164720555af3c5ea38f398a0faded832075a9221c63c9a0593d4f19aa0
STRICT_SERIALIZER_PY_SHA256=c76215db0672049da94135293705fbbc106bb65d8442653fd147fffd98b3a69b
PILOT_RUNNER_PY_SHA256=ce118b9ca775768013b9c8c49228dc6d1e57bb1c0477a2ebaf9ecc181b33c460
```

## Failed-root bootstrap timeline

All values below are host monotonic nanoseconds unless explicitly marked.  A
value marked `NOT_RETAINED` is not inferred from a missing stream.

| Event | Retained evidence |
|---|---:|
| `T_statebank_process_start` | `7,362,210,321,085` (constructor timestamp encoded in StateBank session identity; no separate lifecycle marker) |
| `T_statebank_ros_ready` | first periodic StateBank snapshot `7,362,292,542,221` |
| `T_aura_process_start` | parent `aura_and_trace_started=7,360,080,089,769` |
| `T_wrapper_first_receive_by_AURA` | AURA `cb=1`, `7,360,565,755,910` |
| `T_aura_first_eligible_input` | first retained source-bound sample (native `33,428,000 us`, generation `6655`) received by StateBank at `7,387,222,131,926` |
| `T_aura_first_disturbance_state_publish` | publication is proven before that StateBank receive; exact AURA publisher tick is not separately retained |
| `T_statebank_aura_subscription_discovery` | `NOT_RETAINED` in failed root (subscription is constructed during node init) |
| `T_statebank_first_aura_callback` | `7,387,222,131,926` (first retained AURA sample in snapshot 501) |
| `T_statebank_first_aura_ingest` | `7,387,222,131,926` (same first retained sample) |
| `T_snapshot_request` | `7,376,562,317,985` |
| `T_snapshot_barrier_close` | `7,376,572,137,503` |
| `T_snapshot_ack` | `7,376,583,702,360`, rejected by the probe as `snapshot_stream_missing:aura` |
| `T_probe_validation_failure` | exact probe tick not retained; parent row finalized invalid at `7,447,360,345,233` |
| `T_statebank_process_exit` | exact lifecycle tick not retained; return code 1 observed during cleanup after probe abort |

```text
BOOTSTRAP_CAUSAL_TIMELINE=PASS_ORDERING_RECONSTRUCTED_WITH_EXPLICIT_NOT_RETAINED_FIELDS
```

The decisive ordering is: AURA wrapper callbacks begin, the bootstrap request
and barrier close occur, the required AURA StateBank sample is absent, and the
first source-bound AURA output/ingest is observed only after that barrier.

```text
DEEPEST_PROVEN_BOUNDARY=AURA_PUBLISHED_AFTER_SNAPSHOT_BARRIER
BOOTSTRAP_READINESS_RACE=PROVEN
BOOTSTRAP_REQUEST_READINESS_PREDICATE=OLD:controller_context+collector_context+snapshot_request_subscriber;REPAIRED:healthy_StateBank+all_required_same_session_source_bound_streams+valid_source_timestamp_identity
STATEBANK_EXIT_CAUSAL_ROLE=CONSEQUENCE_OF_PROBE_ABORT
```

The old request predicate waited for controller/collector context and a
snapshot-request subscriber, but not for StateBank health plus all required
same-session source-bound streams.  It could legally issue `block_index=-1`
before AURA had produced an eligible sample.

## Repair

```text
IMPLEMENTATION_REPAIR=EXPLICIT_PRE_SCIENCE_STATEBANK_READINESS_BARRIER_PLUS_ATOMIC_BARRIER_RECHECK
SEMANTICS_CHANGED=false
```

The repaired path is:

1. StateBank publishes healthy status with per-stream readiness.
2. `Stage1Probe` waits (bounded by the existing timeout policy) for healthy
   StateBank status and every required stream, including `aura`.
3. StateBank re-evaluates readiness at the snapshot barrier.  If a stream is
   absent or has the wrong session/reset/source/receive domain, it emits an
   invalid `required_streams_not_ready` ACK rather than an apparently complete
   ACK.
4. Only a source-complete same-session ACK can be accepted for bootstrap.

Readiness intentionally does not compare every stream with the global causal
generation: a reset in one modality advances that global marker, while
unaffected streams remain valid current-session source data.  Per-stream reset,
session, source domain, finite positive native timestamp and causal barrier
membership remain enforced.  This avoids a false bootstrap failure without
weakening source or validity semantics.

## Deterministic regression

```text
REGRESSION_TEST_RESULT=PASS_47
PY_COMPILE=PASS
GIT_DIFF_CHECK=PASS
```

Focused command:

```text
source /opt/ros/jazzy/setup.bash
source /home/nahhao74/px4_ros2_ws/install/setup.bash
export PYTHONPATH=.:1_AURA:3_AEGIS:${PYTHONPATH}
python3 -m pytest -q \
  1_AURA/tests/test_statebank_v1.py \
  1_AURA/tests/test_sensorcombined_provenance_wire.py \
  1_AURA/tests/test_wm1_stage1_acquisition.py \
  1_AURA/tests/test_wm1_action_link.py
```

The tests cover missing mandatory AURA, source-bound all-stream readiness,
invalid-AURA presence without validity promotion, stale-session rejection,
future-sample exclusion, modality-specific reset handling, readiness waiting,
and stamped wire-domain decoding.  The atomic barrier recheck is exercised by
the source implementation and the live qualification below.

## Fresh bootstrap-only qualification

```text
DIAGNOSTIC_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_statebank_aura_bootstrap_qualification_20260828_06
LIVE_BOOTSTRAP_QUALIFICATION=PASS_8_OF_8_PRE_SCIENCE_BOOTSTRAP_ONLY
BOOTSTRAP_SESSIONS_PLANNED=8
BOOTSTRAP_SESSIONS_COMPLETED=8
BOOTSTRAP_SESSIONS_VALID=8
MANIFEST_CONSUMED=false
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
```

The harness ran the exact startup/orchestration class with
`block_index=-1`, `--require-statebank-readiness`, a bounded readiness timeout,
and `bootstrap_only=true`.  It created empty qualification schedules only; the
frozen randomized manifest was neither read nor modified.  Each session
stopped after one valid snapshot ACK and before any action offer or scientific
`T_D`.

The per-session lifecycle evidence is retained in
`qualification_results.json`; decisive markers for all eight sessions are:

| Sessions | AURA subscription/callback/ingest | Snapshot presence | ACK | Action offer |
|---:|---|---|---|---|
| CALM R1–R4 | present; same StateBank session | all 7 required streams, including AURA | valid/accepted 4/4 | 0 |
| GUST_E R5–R8 | present; same StateBank session | all 7 required streams, including AURA | valid/accepted 4/4 | 0 |

Representative CALM-R1 monotonic order is:

```text
statebank_start       11,533,288,958,528
aura_sub_discovered   11,533,322,137,637
aura_callback         11,533,327,934,162
aura_ingest_accept    11,533,328,069,965
snapshot_request      11,546,493,733,490
snapshot_barrier      11,546,506,464,418
stream_presence       11,546,506,491,180 (required_streams_ready=true, missing=[])
snapshot_ack          11,546,522,542,875 (ack_valid=true, reason=accepted)
```

All eight qualification rows have the same causal shape.  AURA samples are
source-bound even when the initial W20/diagnostic validity bit is false; the
bootstrap contract checks presence and identity, while scientific/control
validity remains fail-closed downstream.

```text
STATEBANK_REQUIRED_STREAMS_RESULT=PASS_8/8_AURA_MANDATORY
AURA_STREAM_RESULT=PASS_8/8
SNAPSHOT_ACK_RESULT=PASS_8/8
SNAPSHOT_CAUSALITY_RESULT=PASS_8/8_ATOMIC_BARRIER_READINESS
RUNTIME_SOURCE_COUNTER_ATTESTATION=PASS_8/8_PRE_SCIENCE
STATEBANK_PROCESS_HEALTH=PASS_8/8
ATOMIC_PROVENANCE_RESULT=PASS_PRE_SCIENCE
SOURCE_CONTINUITY_RESULT=PASS_PRE_SCIENCE
CLOCK_ALIGNMENT_RESULT=PASS_PRE_SCIENCE
```

Storage preflight and cleanup also passed:

```text
KINGSTON_RW=PASS
WRITE_READ_PROBE=PASS
FREE_SPACE_REQUIREMENT=53687091200
FREE_SPACE_AT_QUALIFICATION_PREFLIGHT=106971791360
AGENT_V6=OFF
STALE_RUNTIME_PROCESSES_AFTER_RUN=0
SAFETY_RESULT=PASS_NO_VIOLATION
```

## Authority and next state

```text
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED_IN_THIS_TASK
SEALED_STATE=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
production_authority=false
MODEL_TRAINING=NOT_AUTHORIZED
R1=NOT_AUTHORIZED
NEXT_STATE=READY_FOR_OWNER_FRESH_PILOT_RETRY_REVIEW
```

This report closes the bootstrap implementation blocker and authorizes no
scientific run by itself.  The prior randomized root remains immutable
`INVALID_INFRASTRUCTURE_PRE_SCIENCE`; its records are not pooled or promoted.
