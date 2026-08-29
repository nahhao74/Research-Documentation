# VNEXT WM1 V2R1 — atomic Timesync provenance and live campaign-equivalent soak

Date: 2026-08-28  
Scope: implementation/qualification of the owner-approved Option A timestamp
contract. No randomized pilot, training, R1 or SEALED access was performed.

## OWNER_TIMESTAMP_CONTRACT

~~~text
OWNER_APPROVED_TIMESTAMP_SEMANTIC_CHANGE_IMPLEMENTED=true
SOURCE_CONTINUITY_DOMAIN=NATIVE_PX4_SOURCE_TIME_AND_GENERATION
CLOCK_ALIGNMENT_DOMAIN=CAUSAL_CROSS_DOMAIN_MAPPING_WITH_EXPLICIT_EPOCH_PROVENANCE
NATIVE_SOURCE_GAP_THRESHOLD_US=20000
TIMESYNC_OFFSET_CHANGE_MUST_NOT_BE_INTERPRETED_AS_NATIVE_SOURCE_LOSS=true
~~~

Option A is the approved semantic change. No further control-law or scientific
timing change was made or is implied by the two `FURTHER_*` fields below.

## SOURCE_IDENTITY_REATTESTATION

~~~text
SOURCE_IDENTITY_REATTESTATION=PASS_WITH_VEHICLEIMU_CORRECTION
PRIOR_VEHICLEIMU_IDENTITY=malformed_in_previous_report_only
CURRENT_VEHICLEIMU_SHA256=9b2712f20fa715b834cf312290ecb835778dbcbc85d19b39e6d6424f9e88a22e
PX4_BASE_COMMIT=85df8c2281c2466b30a121b22b0bf33dc69bcfe4
PX4_EXECUTABLE_SHA256=a06ed2c3714807257513c86e587c5d5e8402f33c7e0c0c37dc6cbe80009c57f9
SENSOR_PIPELINE_AUDIT_SHA256=0226f89b88cce7d7a0ff4e5ab096b4a2ba25dea1720258ff68d7fc949ea18087
UXRCE_DDS_CLIENT_SHA256=65b9433b35c832881a2f58670bb95b9acc66f18811e6890592d9f242a3f5e709
DDS_TOPICS_H_EM_SHA256=006a9f0ef62d67780be28ae1aa573462b4933222cbb9087c675afa44235dd370
GENERATED_DDS_TOPICS_H_SHA256=576cf91597d2b074c610d530fd8b61a0d11e3968326e827b5fef97a00a88eec3
DDS_TOPICS_YAML_SHA256=83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0
SENSORCOMBINED_MSG_SHA256=ccac9a26fcf460228fe55459fe6710eb2e608665ff13141676805e98cbd5e462
TIMESYNCSTATUS_MSG_SHA256=244e915343f678a7a9895002d2f36983bfd4494a2a9167db70b01e2dc5a5566c
AURA_MOVING_RUNTIME_SHA256=495e95284b55d52e31cb2e2630653d2a3494c42d02a57f325bd755d47da853cf
AURA_MOVING_RUNTIME_NODE_SHA256=50bdec9e6e2bef33ea55a6f81d978b36ffd714d5f0451791d6539801a8a8c066
AURA_STATEBANK_NODE_SHA256=29c81c693a8630010fd17982fac226082ec48de6be880851fad263a6f52e09c1
WM1_DATA1_VALIDATOR_SHA256=a5cbd3edb56768c582bea8ebbc62254d41dc923d4084af83641f69a1b1dd8d5c
WM1_CLOCK_BRIDGE_SHA256=ae16cc8c32b97a5e9e484154b7bf64d2e05fb7aeebb1f899baded7ec3766d429
PILOT_RUNNER_SHA256=575efc07194630873af671b976a7ba4b8d7fa6f14eb9bd58d25a7e64e6102cce
STRICT_SERIALIZER_SHA256=c76215db0672049da94135293705fbbc106bb65d8442653fd147fffd98b3a69b
MANIFEST_SHA256=253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7
SOURCE_REGISTRY=v3;SHA256=b4026e78d7da685c11503e594784b00091cdf2f86453bbca198589a2b0d640da
MICRO_XRCE_DDS_CLIENT_COMMIT=711aef423edd1820347b866d1e4164832df35d04;VERSION=2.4.0
MICROXRCE_AGENT_COMMIT=155cfaaf8b7abac2e85d4a62d3649b09ace0be55;VERSION=3.0.1
MICROXRCE_AGENT_BINARY_SHA256=004a44d9b9465b4eebb9595213a3d86661c6e0b47876a7b0be0561a9ca6f8bdf
RMW_IMPLEMENTATION=rmw_fastrtps_cpp;VERSION=8.4.3;TRANSPORT=UDP4_127.0.0.1:8888
PX4_WORKTREE_STATUS=DIRTY_OWNER_CONTROLLED_BUILD;HASHES_REATTESTED
~~~

The prior report/root was not edited. The corrected `VehicleIMU.cpp` digest is
recorded only in this new ledger.

## ATOMIC_PROVENANCE_IMPLEMENTATION

~~~text
TIMESTAMP_RECORD_SCHEMA_VERSION=WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V2
ATOMIC_PROVENANCE_IMPLEMENTATION=V2_DUAL_DOMAIN_RECORD_PATH_WITH_EXACT_SENDER_PROVENANCE_VALIDATION
LIVE_WIRE_PROVENANCE_BINDING=NOT_IMPLEMENTED_CURRENT_INTERFACE_BLOCKED
~~~

`DualDomainTimestampRecord` now retains native timestamp/generation, raw and
mapped values, mapping epoch/version/generation, offset and uncertainty, plus
the sender session/protocol/version/generation/epoch/offset/raw-representation
tuple. `CausalTimesyncTimestampMapper` accepts that tuple only when all fields
match a prior mapping snapshot. A generation alone is explicitly insufficient.
The record projection is observational; native PX4 message fields and action
semantics are unchanged. Native PX4-sized timestamps are marked
`NATIVE_SOURCE_IDENTITY`; agent-epoch records without the complete sender tuple
remain clock-invalid and fail closed.

## PER_MESSAGE_MAPPING_PROVENANCE

~~~text
PER_MESSAGE_MAPPING_PROVENANCE=NOT_AVAILABLE_CURRENT_PX4_UXRCE_WIRE
PER_MESSAGE_MAPPING_PROVENANCE_REPLAY=EXACT
LIVE_NATIVE_SOURCE_IDENTITY=NOT_AVAILABLE_CURRENT_SENSORCOMBINED_WIRE
~~~

The current local `SensorCombined.msg` contains no native source/generation or
mapping generation/epoch/offset field, and `TimesyncStatus.msg` contains no
sender mapping generation. `accelerometer_timestamp_relative` is not an
absolute native source identity.
`dds_topics.h.em` serializes every topic with the mutable
`session->time_offset / 1000`; `uxrce_dds_client.cpp:on_time()` updates that
offset asynchronously. Therefore the receiver cannot prove which sender
offset represented a particular agent-epoch message. The optional message
attributes supported by the AURA adapter are not present on the current
generated ROS message and do not make the live wire exact.

Adding exact tuple metadata to the wire, or a lossless topic-bound side ledger
joined at the receiver, requires a PX4/uXRCE/ROS interface change not present in
the frozen build. No such change was silently made.

## CONTROL_INPUT_DOMAIN_DEPENDENCY_AUDIT

~~~text
CONTROL_INPUT_DOMAIN_DEPENDENCY_AUDIT=PASS
FURTHER_CONTROL_SEMANTIC_CHANGE_REQUIRED=false
FURTHER_SCIENTIFIC_TIMING_SEMANTIC_CHANGE_REQUIRED=false
REQUIRES_INTERFACE_CHANGE_FOR_LIVE_ATOMIC_PROVENANCE=true
~~~

Native/local AURA detection may remain eligible when native continuity, local
freshness, finite/reset/frame/calibration and safety gates pass; that requires
an actual native source identity, which the current wire does not carry.
AURA/C1/FAST outputs requiring a cross-domain join, StateBank/WM binding and
scientific certification require `CLOCK_ALIGNMENT_VALID=true`; no clock-false
output is downgraded to native-only.

## ATOMIC_PROVENANCE_FAILED_EVENT_REPLAY

Immutable event arithmetic was replayed without mutating its root:

| sample | raw agent timestamp | sender offset used | mapped/native frontier | native delta | source | clock |
|---|---:|---:|---:|---:|---|---|
| first | 1,787,910,190,946,130 | -1,787,910,158,526,130 | 32,420,000 | — | valid | valid |
| transition | 1,787,910,191,129,148 | -1,787,910,158,701,148 | 32,428,000 | 8,000 | valid | valid with exact tuple |

The old receiver mapping retained the previous offset for the second raw value:
`32,603,018 - 32,420,000 = 183,018 us`; the exact sender tuple gives
`32,428,000 - 32,420,000 = 8,000 us`. A replay with no tuple keeps native
continuity true but sets `CLOCK_ALIGNMENT_VALID=false` and emits no
`NATIVE_SOURCE_GAP`.

~~~text
ATOMIC_PROVENANCE_FAILED_EVENT_REPLAY=PASS
RAW_SOURCE_DELTA_US=8000
OFFSET_STEP_US=-175018
OLD_MAPPED_DELTA_US=183018
EXACT_PROVENANCE_MAPPED_NATIVE_RECONSTRUCTION=32428000
NATIVE_SOURCE_GAP_EMITTED=false
~~~

## REGRESSION_TEST_RESULT

~~~text
REGRESSION_TEST_RESULT=PASS
FOCUSED_TESTS=118
PY_COMPILE=PASS
GIT_DIFF_CHECK=PASS
~~~

Coverage includes stable mapping, exact offset transition, incomplete/wrong
sender tuple, prior-only/future rejection, mapping epoch/session reset,
representation transition, real native gap, H1000/T_D/T_A native identity,
strict V2 record round-trip and V2 validator field completeness. The additional
guard prevents an agent-epoch record from claiming clock validity without the
complete sender tuple.

## LIVE_QUALIFICATION_RESULT

~~~text
LIVE_QUALIFICATION_RESULT=NOT_RUN_PRECONDITION_BLOCKED
BLOCKING_PRECONDITIONS=CURRENT_WIRE_HAS_NO_NATIVE_SOURCE_IDENTITY_OR_ATOMIC_SENDER_PROVENANCE;KINGSTON_RO
RUNTIME_SOURCE_ATTESTATION=NOT_STARTED
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
KINGSTON_MOUNT_STATE=RO
KINGSTON_RW=FAIL
WRITE_READ_PROBE=NOT_RUN_DUE_RO
KINGSTON_FREE_SPACE_GIB=144
~~~

The requested live run was not started: the frozen `SensorCombined` wire does
not carry an absolute native source identity/generation or atomic sender
mapping provenance, so the live native continuity and transition contract
cannot be certified; additionally the required `/media/nahhao74/KINGSTON`
mount was read-only during preflight (`findmnt`: `exfat ... ro`). No
substantial artifact was written and no scientific slot was consumed.

## LIVE_CAMPAIGN_EQUIVALENT_SOAK

~~~text
LIVE_SOAK_ROOT=NOT_CREATED
LIVE_SESSIONS_PLANNED=8 (CALM=4,GUST_E=4)
LIVE_SESSIONS_COMPLETED=0
LIVE_SESSIONS_VALID=0
CAMPAIGN_EQUIVALENT_SOAK_RESULT=NOT_RUN_INTERFACE_AND_STORAGE_PRECONDITION
~~~

The retained prior offline lifecycle soak remains qualification evidence only;
it is not relabelled as a live soak. Running it now would not resolve the
atomic provenance gap and would violate the required writable-storage
precondition.

## QUALIFICATION_STATE

~~~text
NATIVE_SOURCE_GAP_RESULT=PASS_DETERMINISTIC_REPLAY (8,000 us; >20 ms=0)
FALSE_NATIVE_SOURCE_GAP_RESULT=PASS_183018_US_CLASSIFIED_AS_MAPPING_TRANSITION
CLOCK_EPOCH_RESULT=PASS_PRIOR_ONLY_REPLAY_AND_SESSION_RESET
CLOCK_PROVENANCE_RESULT=FAIL_CURRENT_WIRE_PROVENANCE_NOT_AVAILABLE
AURA_RESULT=DUAL_DOMAIN_POLICY_IMPLEMENTED_NATIVE_LOCAL_ALLOWED_CROSS_DOMAIN_FAIL_CLOSED
C1_FAST_RESULT=DEPENDENCY_POLICY_PASS_OFFLINE_LIVE_NOT_STARTED
STATEBANK_RESULT=V2_FIELDS_PROPAGATED_OFFLINE_LIVE_NOT_STARTED
SESSION_TRANSITION_RESULT=PASS_PRIOR_OFFLINE_REPLAY_LIVE_NOT_STARTED
STRICT_PERSISTENCE_RESULT=PASS_PRIOR_STRICT_READBACK_LIVE_NOT_STARTED
SCIENTIFIC_TRANSITION_CERTIFIABILITY=FAIL_CLOSED_NOT_CERTIFIABLE
~~~

If a future source sample carries the complete sender tuple through an approved
versioned observational interface, the mapper can certify the transition
without changing native continuity. With the current frozen wire, a normal
transition must remain clock-invalid and scientific binding must fail closed.

## IMPLEMENTATION_SEMANTICS_PARITY

~~~text
IMPLEMENTATION_SEMANTICS_PARITY=PASS_FOR_APPROVED_OPTION_A
RAW_SOURCE_SEMANTICS_UNCHANGED=true
T_D_T_A_NATIVE_IDENTITY_UNCHANGED=true
H1000_NATIVE_DURATION_UNCHANGED=true
AURA_20MS_RULE_UNCHANGED=true
FAST_T1_C1_P1_P2_H1000_QOS_REFERENCE_TARGET_SAFETY_UNCHANGED=true
~~~

No PX4/DDS message, poll interval, QoS, control law, threshold, target or
authority was changed. The only implementation change is versioned
provenance/field validation in the Python record path; the current PX4/uXRCE
wire still lacks atomic per-message sender provenance.

## RESEARCH_REQUEST

~~~text
RESEARCH_REQUEST=NONE
~~~

## AUTHORITY

~~~text
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
IMPLEMENTATION_AUTHORITY=OWNER_APPROVED_OPTION_A;LIVE_WIRE_EXTENSION_REQUIRES_OWNER_INTERFACE_REVIEW
SEALED_STATE=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED
R1=NOT_AUTHORIZED
production_authority=false
NEXT_STATE=OWNER_INTERFACE_REVIEW_FOR_ATOMIC_TIMESYNC_PROVENANCE
~~~

## FINAL_DECISION

~~~text
ATOMIC_TIMESYNC_PROVENANCE_OWNER_INTERFACE_REVIEW_REQUIRED
~~~
