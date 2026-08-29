# VNEXT WM1 V2R1 — atomic SensorCombined provenance wire implementation and live 8-session soak

Date: 2026-08-28

This record covers the owner-approved versioned observational wire extension
and its non-scientific qualification. It does not authorize or execute a
randomized pilot, training, R1, SEALED access, or production authority.

## INTERFACE_SCHEMA_VERSION

```text
INTERFACE_SCHEMA_VERSION=SensorCombinedStampedV1
TIMESTAMP_RECORD_SCHEMA_VERSION=WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V2
SOURCE_CONTINUITY_DOMAIN=NATIVE_PX4_SOURCE_TIME_AND_GENERATION
CLOCK_ALIGNMENT_DOMAIN=CAUSAL_CROSS_DOMAIN_MAPPING_WITH_EXPLICIT_EPOCH_PROVENANCE
NATIVE_SOURCE_GAP_THRESHOLD_US=20000
DDS_TOPIC=/uav_a/fmu/out/sensor_combined_stamped_v1
DDS_TOPIC_ID=2032
DDS_CDR_SERIALIZED_SIZE_BYTES=132
STANDARD_SENSORCOMBINED_SEMANTICS_UNCHANGED=true
```

`SensorCombinedStampedV1` is additive and observational. It carries the
copied SensorCombined payload plus native source timestamp/generation and the
single sender mapping tuple (epoch, generation, version, protocol, offset,
raw representation, uncertainty and session/reset identity). The existing
standard SensorCombined topic is neither renamed nor reinterpreted.

## ATOMIC_SENDER_SNAPSHOT

```text
ATOMIC_SENDER_SNAPSHOT=PASS
SENDER_MAPPING_GENERATION_SEMANTICS=OBSERVATIONAL_PROVENANCE_ONLY
ATOMIC_PROVENANCE_IMPLEMENTATION=VERSIONED_SINGLE_SAMPLE_WRAPPER_WITH_EXACT_SENDER_TUPLE
```

The PX4 path copies one native `sensor_combined_s`, captures one
`SenderMappingContext::snapshot`, derives the wire timestamp from that
snapshot, and fills the wrapper provenance from the same snapshot before one
serialization/send operation. The snapshot is initialized at session creation
(mapping generation/epoch 1) and advances only when the applicable mapping
relation changes. No native message field, poll interval, QoS, controller law,
or candidate authority was changed.

## SOURCE_IDENTITY_REATTESTATION

```text
SOURCE_IDENTITY_REATTESTATION=PASS
SOURCE_IDENTITY_DRIFT=EXPECTED_OWNER_APPROVED_VERSIONED_INTERFACE_EXTENSION
CURRENT_RUNTIME_IDENTITIES=FROZEN_EXACT_HASH_LEDGER_BELOW
```

Current runtime identities used by the live artifacts:

| Component | Exact identity |
|---|---|
| PX4 executable | `18bd77a9ea60f5ea95c865fa48d794852ad21abc0bc9b1bfa7528981d386b284` |
| PX4 base commit | `85df8c2281c2466b30a121b22b0bf33dc69bcfe4` |
| `VehicleIMU.cpp` | `9b2712f20fa715b834cf312290ecb835778dbcbc85d19b39e6d6424f9e88a22e` |
| `sensor_combined_stamped_v1.hpp` | `fa2aec02917a5499fa63eacc8fb74a4cb899124abb91ff1227efa57181625b87` |
| `sensor_pipeline_audit.hpp` | `0226f89b88cce7d7a0ff4e5ab096b4a2ba25dea1720258ff68d7fc949ea18087` |
| `dds_topics.h.em` | `03e5308174f01de51c532cd67384a20caf09af33c14d1eb9565969030e4d018c` |
| generated `dds_topics.h` | `d39b1895f92f0792f3fe3a6d53375b9fc666c105c4d215cb51b242d377e3ff5a` |
| `uxrce_dds_client.cpp` | `17fcd412a9645005ae2dcdcce1d594d73dead3c41fa8eec4ac4e166ece42032f` |
| `utilities.hpp` | `8a17efb9031e5bb8b11341020331f31bf2a277c6d428244a371f18a7598dff03` |
| `dds_topics.yaml` | `83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0` |
| `SensorCombinedStampedV1.msg` | `5cb31c66d07c9c2bdaa9a4a7c243f10e9e70423b28957dee87b10862a59d06` |
| generated type description | `fdc4c4427435eddd86823456771fbb5563f5a29b476ac7a0b583a8e7bacdc764` |
| standard `SensorCombined.msg` | `ccac9a26fcf460228fe55459fe6710eb2e608665ff13141676805e98cbd5e462` |
| `TimesyncStatus.msg` | `244e915343f678a7a9895002d2f36983bfd4494a2a9167db70b01e2dc5a5566c` |
| AURA `moving_runtime.py` | `3289e6f8b03efd6983769ae826bbbde0682de33b105dde25641a6db40a803901` |
| AURA `moving_runtime_node.py` | `b8d9883526580950d3e308c5501849907ab6e0613428fe68a363079bc78ce9dd` |
| AURA `statebank_v1_node.py` | `e1014b4e6754441ab579479251c9b731c8e59a3b4d8351dadaec5855b489f2d0` |
| strict qualified serializer | `c76215db0672049da94135293705fbbc106bb65d8442653fd147fffd98b3a69b` |
| pilot runner (frozen, unused) | `575efc07194630873af671b976a7ba4b8d7fa6f14eb9bd58d25a7e64e6102cce` |
| frozen manifest | `253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7` |
| Source Registry v3 | `b4026e78d7da685c11503e594784b00091cdf2f86453bbca198589a2b0d640da` |
| Micro XRCE-DDS Client | `711aef423edd1820347b866d1e4164832df35d04`, v2.4.0 |
| MicroXRCEAgent | `155cfaaf8b7abac2e85d4a62d3649b09ace0be55`, v3.0.1, binary `004a44d9b9465b4eebb9595213a3d86661c6e0b47876a7b0be0561a9ca6f8bdf` |
| RMW | `rmw_fastrtps_cpp` 8.4.3 |

The previous pre-interface PX4 binary identity (`a06ed2c...`) is not claimed
for this run; the hash above is the binary actually used for the new wrapper
wire and live roots.

## REGRESSION_TEST_RESULT

```text
REGRESSION_TEST_RESULT=PASS
FOCUSED_TESTS=98 passed
PY_COMPILE=PASS
RELEVANT_DIFF_CHECK=PASS
UNRELATED_PX4_WORKTREE_DIFF_CHECK=PREEXISTING_BLANK_LINE_IN_4002_GZ_X500_DEPTH
ROS_INTERFACE_IMPORT=PASS
ROS_INTERFACE_SHOW=PASS
CDR_SIZE_CHECK=132 bytes PASS
AGENT_VERBOSE_RUNTIME_POLICY=OFF
```

The focused suite covers stable and transitioning mappings, missing/wrong
provenance, epoch/session/reset boundaries, future-sample rejection, native
source gaps and generation discontinuity, H1000/T_D/T_A native identity,
StateBank propagation, strict persistence, malformed wrapper rejection and
standard/wrapper payload parity.

## KINGSTON_STORAGE_RESULT

```text
KINGSTON_STORAGE_RESULT=PASS
KINGSTON_MOUNT=/media/nahhao74/KINGSTON
KINGSTON_DEVICE=/dev/sda1
KINGSTON_FILESYSTEM=exfat
KINGSTON_RW=PASS
WRITE_READ_PROBE=PASS
FREE_SPACE_BYTES=134668353536
FREE_SPACE_GIB=125.4
SUBSTANTIAL_ARTIFACTS_OUTSIDE_KINGSTON=NONE_FOR_THIS_TASK
STALE_FLIGHT_RUNTIME_PROCESSES=NONE
```

The mount is `rw` with `errors=remount-ro`; no destructive filesystem repair
was used. All live roots below are new roots under Kingston and prior roots
remain immutable.

## LIVE_SINGLE_SESSION_QUALIFICATION

```text
LIVE_SINGLE_SESSION_QUALIFICATION=PASS
LIVE_SINGLE_SESSION_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_atomic_sensorcombined_provenance_live_20260828_03
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
QUALIFICATION_DURATION_S=12
STANDARD_SAMPLES=2376
WRAPPER_SAMPLES=2377
PAIRED_SAMPLES=2376
PAYLOAD_PARITY_MATCHES=2376
PAYLOAD_PARITY_MISMATCHES=0
WRAPPER_PROVENANCE_VALID=2377
NATIVE_SOURCE_TIMESTAMP_MONOTONIC=true
WRAPPER_GENERATION_MONOTONIC=true
```

The one wrapper-only startup observation is not paired with a standard sample;
all 2,376 paired samples have exact sensor-value parity. The live wrapper
tuple was generation 1/epoch 1/offset 0 with raw representation
`px4_boot_us`; AURA received 2,373 records and StateBank 240 snapshots.

```text
ATOMIC_PROVENANCE_LIVE=PASS_STABLE_EPOCH
NATIVE_SOURCE_IDENTITY_LIVE=PASS
CLOCK_PROVENANCE_LIVE=PASS_EXACT_SENDER_TUPLE
SENSOR_PAYLOAD_PARITY=PASS
SOURCE_CONTINUITY=PASS
UXRCE_PREPARE_FAILURES=0
WRITER_ERRORS=0
WRITER_DROPS=0
LIVE_TIMESYNC_TRANSITION=NOT_OBSERVED_NATURALLY
```

The strict live-derived record also passed `allow_nan=false` write/readback,
retained one auxiliary non-finite provenance entry, and left raw evidence
unchanged. This qualification was diagnostic only and did not consume the
randomized manifest.

## LIVE_8_SESSION_CAMPAIGN_EQUIVALENT_SOAK

```text
LIVE_8_SESSION_SOAK_RESULT=PASS
LIVE_SOAK_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_atomic_sensorcombined_provenance_live_8session_soak_01
LIVE_SESSIONS_PLANNED=8 (CALM=4,GUST_E=4)
LIVE_SESSIONS_COMPLETED=8
LIVE_SESSIONS_VALID=8
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
```

Each session was a fresh live PX4/Gazebo/uXRCE/AURA/StateBank lifecycle with
startup, wrapper reception, StateBank reception, source-counter checks and
clean shutdown. `GUST_E` is a lifecycle context label only; no gust stimulus,
candidate action or scientific assignment was executed.

| Session | Context | Wrapper provenance | AURA | StateBank | Source counters |
|---:|---|---|---|---|---|
| 1 | CALM | PASS | PASS | PASS | PASS |
| 2 | CALM | PASS | PASS | PASS | PASS |
| 3 | CALM | PASS | PASS | PASS | PASS |
| 4 | CALM | PASS | PASS | PASS | PASS |
| 5 | GUST_E | PASS | PASS | PASS | PASS |
| 6 | GUST_E | PASS | PASS | PASS | PASS |
| 7 | GUST_E | PASS | PASS | PASS | PASS |
| 8 | GUST_E | PASS | PASS | PASS | PASS |

The per-session `SC_X4` observations had maximum generation lag 1 and source
lag 8,000 us. All sessions had zero prepare failures, writer errors/drops and
no canonical or upstream >20-ms trigger. The wrapper carried a valid native
timestamp/generation and exact sender mapping tuple in every session.

```text
NATIVE_SOURCE_GAP_RESULT=PASS;MAX_GAP_GT_20MS=0;SC_X4_MAX_GENERATION_LAG=1;SC_X4_MAX_SOURCE_LAG_US=8000
FALSE_NATIVE_SOURCE_GAP_RESULT=PASS_DETERMINISTIC_REPLAY;NO_LIVE_OFFSET_TRANSITION_OBSERVED
CLOCK_EPOCH_RESULT=PASS_INITIAL_EPOCH_PER_SESSION;NO_STALE_CROSS_SESSION_STATE
CLOCK_PROVENANCE_RESULT=PASS_EXACT_WIRE_TUPLE_STABLE_EPOCH
AURA_RESULT=PASS
C1_FAST_RESULT=PASS_DEPENDENCY_POLICY_NO_CANDIDATE
STATEBANK_RESULT=PASS
SESSION_TRANSITION_RESULT=PASS_8_FRESH_LIFECYCLES
QUALIFIER_RESULT=PASS_REPLAY_AND_LIVE_DERIVED_RECORD_PATH
STRICT_PERSISTENCE_RESULT=PASS_LIVE_DERIVED_RECORD_AND_STRICT_READBACK
PREPARE_FAILURES=0
WRITER_ERRORS=0
WRITER_DROPS=0
SAFETY_RESULT=PASS
```

The soak did not observe a natural Timesync offset transition. Its lifecycle
and stable-epoch wire evidence therefore do not substitute for live
transition-sample certification.

## SCIENTIFIC_TRANSITION_CERTIFIABILITY

```text
SCIENTIFIC_TRANSITION_CERTIFIABILITY=FAIL_CLOSED_NOT_CERTIFIABLE
```

The immutable `183018 us` replay is decisive for the implementation: exact
sender provenance reconstructs the native `32428000 us` transition and
prevents a native source-gap claim; absent provenance remains clock-invalid
and fail-closed. However, the owner requirement says `CERTIFIABLE_WITH_EXACT_PROVENANCE`
needs actual live wire transition evidence, and no natural transition occurred
in these eight non-scientific sessions. A future block-level transition must
therefore remain fail-closed until that evidence and the owner policy are
reviewed; this does not invalidate the stable-epoch qualification.

## IMPLEMENTATION_SEMANTICS_PARITY

```text
IMPLEMENTATION_SEMANTICS_PARITY=PASS
RAW_SENSORCOMBINED_SEMANTICS_UNCHANGED=true
NATIVE_SOURCE_SEMANTICS_UNCHANGED=true
T_D_T_A_NATIVE_IDENTITY_UNCHANGED=true
H1000_NATIVE_DURATION_UNCHANGED=true
AURA_20MS_RULE_UNCHANGED=true
FAST_T1_C1_P1_P2_H1000_QOS_REFERENCE_TARGET_SAFETY_UNCHANGED=true
```

The owner-approved interface is observational metadata only. No mapping
transition was hidden, interpolated, forward-filled or used to change source
freshness, action authority, targets, control, QoS or safety semantics.

## AUTHORITY

```text
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED_BY_THIS_TASK
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED
R1=NOT_AUTHORIZED
production_authority=false
RESEARCH_REQUEST=NONE
```

## NEXT_STATE

```text
NEXT_STATE=OWNER_PILOT_RETRY_REVIEW_WITH_FAIL_CLOSED_TRANSITION_POLICY
```

No randomized pilot was run. The live wire and eight-session lifecycle soak
are ready for owner review, while the absence of a natural live Timesync
transition keeps transition-time scientific binding fail-closed.

## FINAL DECISION

```text
ATOMIC_SENSORCOMBINED_PROVENANCE_LIVE_8SESSION_SOAK_PASS_READY_FOR_OWNER_PILOT_RETRY_REVIEW
```
