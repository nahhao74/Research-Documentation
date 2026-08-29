# VNEXT WM1 V2R1 — dual-domain timestamp contract implementation and qualification

Date: 2026-08-28  
Scope: implementation-preserving timestamp provenance, deterministic replay and
non-scientific lifecycle qualification. No randomized pilot, training, R1 or
SEALED access was performed.

## OWNER_TIMESTAMP_CONTRACT

~~~text
OWNER_AUTHORITY=APPROVE_OPTION_A_DUAL_DOMAIN_TIMESTAMP_CONTRACT
SOURCE_CONTINUITY_DOMAIN=NATIVE_PX4_SOURCE_TIME_AND_GENERATION
CLOCK_ALIGNMENT_DOMAIN=CAUSAL_CROSS_DOMAIN_MAPPING_WITH_EXPLICIT_EPOCH_PROVENANCE
TIMESYNC_OFFSET_CHANGE_MUST_NOT_BE_INTERPRETED_AS_NATIVE_SOURCE_LOSS=true
NATIVE_SOURCE_GAP_THRESHOLD_US=20000
~~~

The immutable failed scientific root remains INVALID_SCIENTIFIC_CONTRACT; the
183018-us event is not retrospectively certified or relabelled.

## CURRENT_TIMESTAMP_CONTRACT

The pre-repair runtime used one prior-only Timesync-mapped PX4-compatible
frontier for the source-gap guard. That contract is retained only as historical
context; the current implementation uses the versioned dual-domain record
below. The immutable root remains invalid under the contract that was active
when it ran.

## ROOT_CAUSE_EVIDENCE

The retained R2 pair is native 32420000→32428000 us (8 ms), while a sender
Timesync offset step of -175018 us made the receiver's old-offset mapped pair
32420000→32603018 us (183018 us). PX4/uXRCE publication/copy/send and host
receipt remained present. This is mapping evidence, not a source-loss claim.

## TIMESTAMP_RECORD_SCHEMA_VERSION

~~~text
TIMESTAMP_RECORD_SCHEMA_VERSION=WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V1
~~~

Each record retains native and raw timestamps, native generation, both validity
predicates and reasons, mapping epoch/version/generation, raw domain, mapped
value, offset, uncertainty and host monotonic receipt. Raw/native values are
not overwritten.

## EXACT_LOCAL_SOURCE_IDENTITIES

| component | SHA/identity |
|---|---|
| PX4 base commit | 85df8c2281c2466b30a121b22b0bf33dc69bcfe4 |
| PX4 executable | a06ed2c3714807257513c86e587c5d5e8402f33c7e0c0c37dc6cbe80009c57f9 |
| sensor_pipeline_audit.hpp | 0226f89b88cce7d7a0ff4e5ab096b4a2ba25dea1720258ff68d7fc949ea18087 |
| GZBridge.cpp | 0a4fd7b2a410b52f69a47558869cf2a085a0f442d9f9c0857161bafc25521477 |
| VehicleIMU.cpp | 9b2712f20fa715b834cf312290ecb835778dbcbc85d19b39e6d6424f9e88a22e |
| voted_sensors_update.cpp | 5e724406f9fd11b34b3b09d81d4f8053a84e1f4fe67b2161732de4f9b13fb290 |
| sensors.cpp | 2d8e1370293da68983752b142d8eb4e77194ac1aca7dc2afef29e4007e533e19 |
| dds_topics.h.em | 006a9f0ef62d67780be28ae1aa573462b4933222cbb9087c675afa44235dd370 |
| generated dds_topics.h | 576cf91597d2b074c610d530fd8b61a0d11e3968326e827b5fef97a00a88eec3 |
| dds_topics.yaml | 83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0 |
| uxrce_dds_client.cpp | 65b9433b35c832881a2f58670bb95b9acc66f18811e6890592d9f242a3f5e709 |
| generated sensor_combined.h | 65e216ca810c9b33ce84aebc747af730ebba77796dd7f2a55fc67c13d43d1b1e |
| AURA moving_runtime.py | 2ecb336e3aa14d1b1df45194b5cfe1303c2a4d68a8f7a85bd3da1812440cc14c |
| AURA moving_runtime_node.py | 96220cd44d72afbd9e72ad2bd73830ee19ee2fd9fffa8181d02ebe796c2f1feb |
| AURA statebank_v1_node.py | a83e4cfb298ce22b71d39656b5bb0253f2c107d93d5a1d2e43099368cfa23d36 |
| wm1_clock_bridge.py | ed8cc0163e0fe82916ec156b2e89608705688ca1802f1bde7f7a8db8b528c2a4 |
| vnext_data1_runner.py | ce8b96e33f4ec7c113ba66c4c9362a68e5ffc56525be746a94578050babe1776 |
| wm1_v2r1_zero_twin_repeatability_r0.py | 396dbc10ceef1accd764ad8d4e2e27239055bbb444675d73f536014547feb0d8 |
| strict qualified serializer | c76215db0672049da94135293705fbbc106bb65d8442653fd147fffd98b3a69b |
| pilot runner | 575efc07194630873af671b976a7ba4b8d7fa6f14eb9bd58d25a7e64e6102cce |
| Micro XRCE-DDS Client | commit 711aef423edd1820347b866d1e4164832df35d04, 2.4.0 |
| MicroXRCEAgent | commit 155cfaaf8b7abac2e85d4a62d3649b09ace0be55, binary 004a44d9b9465b4eebb9595213a3d86661c6e0b47876a7b0be0561a9ca6f8bdf, 3.0.1 |
| transport / RMW | UDP4 127.0.0.1:8888 / rmw_fastrtps_cpp 8.4.3 |
| Source Registry | v3, 94 items, SHA b4026e78d7da685c11503e594784b00091cdf2f86453bbca198589a2b0d640da |

## CONTROL_INPUT_DOMAIN_DEPENDENCY_AUDIT

~~~text
CONTROL_INPUT_DOMAIN_DEPENDENCY_AUDIT=PASS
~~~

| deployable path | dependency | policy |
|---|---|---|
| AURA moving/native detection | NATIVE_LOCAL_ONLY | native SensorCombined source/generation, local receipt, finite/reset/frame/calibration and existing safety gates |
| AURA disturbance / d_hat cross-stream join | CROSS_DOMAIN_REQUIRED | native continuity plus causal mapping epoch; clock false is invalidated with explicit clock reason |
| StateBank/WM causal stream | CROSS_DOMAIN_REQUIRED | native identity and valid mapping provenance required; no post-hoc repair |
| mixed or unknown output | UNRESOLVED | fail closed; never silently downgraded to native-only |

SOURCE_CONTINUITY_VALID=true with CLOCK_ALIGNMENT_VALID=false remains
representable. Native/local diagnostics may continue; cross-domain disturbance
and scientific certification do not.

## SOURCE_CONTINUITY_IMPLEMENTATION

CausalTimesyncTimestampMapper.map_sample computes native SensorCombined time as
timestamp + accelerometer_timestamp_relative in PX4 boot microseconds, tracks
optional publication-derived generation, native deltas and the unchanged
20,000-us threshold, and emits distinct source reasons. Domain-invalid samples
do not advance the native cursor. The legacy source guard is fed the native
frontier, never a mapped value.

~~~text
SOURCE_CONTINUITY_IMPLEMENTATION=PASS
SOURCE_REASONS=NATIVE_SOURCE_GAP|SOURCE_GENERATION_DISCONTINUITY|SOURCE_RECEIPT_STALE|SOURCE_DOMAIN_MISMATCH
~~~

## SOURCE_CONTINUITY_DEFINITION

Native continuity is the causal forward progression of
timestamp + accelerometer_timestamp_relative in PX4 boot microseconds plus
the optional publication generation within one session/reset lineage. A native
delta over 20000 us, incompatible generation or invalid domain fails this
predicate; a Timesync offset never changes it.

## CLOCK_ALIGNMENT_IMPLEMENTATION

Mapping uses only Timesync observations received at or before a sample. Raw
representation, offset, uncertainty, mapping version/generation and epoch are
retained. A transition is CLOCK_MAPPING_EPOCH_TRANSITION; unavailable,
non-monotonic, uncertainty and mixed-representation conditions remain distinct.

~~~text
CLOCK_ALIGNMENT_IMPLEMENTATION=PASS
CLOCK_REASONS=CLOCK_MAPPING_UNAVAILABLE|CLOCK_MAPPING_EPOCH_TRANSITION|CLOCK_MAPPING_NONMONOTONIC|CLOCK_MAPPING_UNCERTAINTY_EXCEEDED|MIXED_TIMESTAMP_REPRESENTATION
~~~

## CLOCK_ALIGNMENT_DEFINITION

Clock alignment is a separate causal claim about a cross-process timestamp
representation. It requires a prior mapping observation, compatible epoch/
version/generation/representation, finite accepted uncertainty and no same-
epoch rollback. Thus source continuity can be true while clock alignment is
false.

## TIMESYNC_MAPPING_EPOCH_IMPLEMENTATION

~~~text
TIMESYNC_MAPPING_EPOCH_IMPLEMENTATION=PASS
EPOCH_PROVENANCE=controller_session_start_us+source_protocol+mapping_version+sender_mapping_generation+raw_representation
EPOCH_RULES=session/protocol/version/offset/representation transition opens a new epoch; reset drops old mapping/native cursors; prior-only; no cross-epoch mapped deltas; no future/interpolation/fill/smoothing
~~~

An explicit session/protocol/version transition also clears old state when the
reported offset is transiently zero. Native cursor state is not advanced by a
domain-mismatched sample.

## TIMESYNC_MAPPING_EPOCH_CONTRACT

Session, protocol, mapping-version, sender mapping-generation, offset or raw
representation changes advance mapping provenance. Mapped deltas from
incompatible epochs are never used as native deltas. Mapping is prior-only;
there is no future observation, interpolation, forward-fill or smoothing.

## T_D_TIME_DOMAIN

T_D remains the native PX4 boot-us source frontier plus native generation; host
receipt and mapped values are provenance only.

## T_A_TIME_DOMAIN

T_A remains the native PX4 accepted-status source frontier plus generation; raw
status and host receipt are retained separately.

## TARGET_TIME_DOMAIN

H40/H80 use native T_A plus the frozen horizon and existing reset/causal rules.
No interpolation or future mapping is introduced.

## H1000_TIME_DOMAIN

H1000_SCIENTIFIC_CANDIDATE_REFRACTORY_V2 remains candidate-only and uses exactly
1,000,000 native source microseconds, never mapped duration.

## AURA_FAST_PATH_REQUIREMENTS

Native/local outputs require native continuity, local receipt freshness,
finite/reset/frame/calibration and existing safety gates. A mapping transition
alone is not native source loss. A cross-domain output is masked with its
clock reason until alignment is valid.

## C1_FAST_REQUIREMENTS

C1/FAST baseline remains eligible only for explicitly NATIVE_LOCAL_ONLY
inputs and the existing authority/safety predicates. CROSS_DOMAIN_REQUIRED or
MIXED dependencies fail closed when clock alignment is false; no silent
downgrade occurs.

## SCIENTIFIC_BINDING_REQUIREMENTS

Certification requires source continuity, clock alignment, session/reset/
frame/clock identity and causal T_D/T_A/X_t binding. A clock-false record is
diagnostic only and cannot be promoted or repaired with future Timesync data.

## STATEBANK_WM_REQUIREMENTS

StateBank/WM retains native candidate identity, release and reset lineage;
cross-domain joins additionally require the valid mapping epoch/provenance.
H1000 remains native and candidate-only.

## FAILED_EVENT_DUAL_DOMAIN_REPLAY

Current implementation replay root:
/media/nahhao74/KINGSTON/wm1_v2r1_dual_domain_timestamp_qualification_20260828_02

The prior `_01` root remains retained as immutable qualification input/evidence
for the 12-record qualifier, strict persistence and lifecycle replay. The
`_02` root was generated after the final mapper/schema/session checks and is
bound to the current `moving_runtime.py` implementation hash above.

~~~text
native source: 32420000 -> 32428000 us = 8000 us
next native delta: 4000 us
Timesync offset step: -175018 us
runtime mapped false jump: 32420000 -> 32603018 us = 183018 us
transition reason: CLOCK_MAPPING_EPOCH_TRANSITION
source_continuity_valid: true
clock_alignment_valid: false at transition sample
scientific_binding_valid: false at transition sample
NATIVE_SOURCE_GAP emitted: no
TIMESTAMP_MAPPER_REPLAY=PASS_DECISIVE_PRIOR_ONLY
RUNTIME_REPLAY_PARITY=PASS_VALUE_LEVEL_WITH_RETAINED_RUNTIME_OFFSET_PROVENANCE
~~~

Replay artifacts (`_02`):

~~~text
replay/dual_domain_records.jsonl sha256=1413760f5653ab328bfef47728807c46671fc83091127138c7c4288efefa7327
replay/replay_summary.json sha256=cc22845873978bd3bbd851bf1610feb3d91092e4996d83de29b468e8b44db491
qualification/failed_event_dual_domain_replay.json sha256=5e70374071a7fe5b106dfa9e5c03afbf470feded80705b6f5a6f853c3c35bb49
qualification/qualification_summary.json sha256=af919d7fe7f1b439a7dcf4bf191bb3c84c05123f2a34a4fad94fe2001b406192
~~~

Exact arithmetic: 183018 + (-175018) = 8000. Raw wire values are unchanged;
the old scientific root is not promoted.

## REGRESSION_TEST_RESULT

~~~text
REGRESSION_TEST_RESULT=PASS
TESTS=146
PY_COMPILE=PASS
git_diff_check=PASS
~~~

Covered cases include stable mapping, offset transition, true native gap,
generation/domain errors, mapping unavailable/rollback, session reset and
zero-offset identity transition, no future observation, uncertainty,
H1000/T_D/T_A native identity, explicit control dependency fail-closed,
strict schema/roundtrip and raw-value preservation.

## NONSCIENTIFIC_QUALIFICATION_RESULT

~~~text
NONSCIENTIFIC_QUALIFICATION_RESULT=PASS_OFFLINE_REPLAY_AND_NATIVE_TRANSITION_CONTRACT
QUALIFIER_REPLAY=12_RECORDS_0_ERRORS_ALL_VALID
TARGET_H40=12/12
TARGET_H80=12/12
NATIVE_SOURCE_CONTINUITY=PASS
FALSE_NATIVE_SOURCE_GAP_FROM_TIMESYNC=0
CLOCK_MAPPING_EPOCH_TRANSITION=OBSERVED_AND_CLASSIFIED
AURA_FAST_NATIVE_DEPENDENCY=PASS
CROSS_DOMAIN_FAIL_CLOSED=PASS
H1000_NATIVE_PARITY=PASS
PREPARE_FAILURES=0
WRITER_ERRORS_DROPS=0
SAFETY=PASS
~~~

The final replay evidence is in `_02/qualification/qualification_summary.json`
(SHA `af919d7fe7f1b439a7dcf4bf191bb3c84c05123f2a34a4fad94fe2001b406192`),
while the retained `_01` input summary (SHA
`103b4d4ea06bdcdcce927d8927b2625b94f1a625424dd461329b2c9cff7abadd`) carries
the 12-record strict-JSON qualifier details. This is non-scientific
contract/replay qualification; it does not certify or mutate the failed
scientific root.

## CAMPAIGN_EQUIVALENT_SOAK_RESULT

~~~text
CAMPAIGN_EQUIVALENT_SOAK_RESULT=PASS_OFFLINE_LIFECYCLE_SOAK
SESSION_TRANSITION_RESULT=PASS
SESSIONS=8 (CALM=4, GUST_E=4)
MANIFEST_CONSUMED=false
SCIENTIFIC_BLOCKS=0
SCIENTIFIC_NONZERO_ACTIONS=0
STATEBANK=PASS
QUALIFIER=PASS_SESSION1_REPLAY
STRICT_PERSISTENCE=PASS
NEXT_SESSION_PREPARATION=PASS
~~~

The ledger is qualification/campaign_transition_ledger.json (SHA
d16dbe6fc4e15b3f1c361bd2ed305f09ee59d9e7ed9fc18b803d26bdb7a2502d). It
exercises session initialization/reinitialization, StateBank, qualification,
strict persistence and transition bookkeeping without consuming the frozen
manifest or launching a scientific action.

## NATIVE_SOURCE_GAP_RESULT

~~~text
NATIVE_SOURCE_GAP_RESULT=PASS_NO_NATIVE_GAP_IN_120_REPLAY_ROWS
RAW_NATIVE_MAX_DELTA_US=8000
RAW_NATIVE_GAP_GT_20MS=0
~~~

## FALSE_TIMESTAMP_SOURCE_GAP_RESULT

~~~text
FALSE_TIMESTAMP_SOURCE_GAP_RESULT=PASS_183018_MAPPED_JUMP_CLASSIFIED_CLOCK_MAPPING_EPOCH_TRANSITION
RUNTIME_MAPPED_DELTA_US=183018
NATIVE_DELTA_US=8000
~~~

## CLOCK_EPOCH_RESULT

~~~text
CLOCK_EPOCH_RESULT=PASS
DECISIVE_EPOCH=mapping_generation=2, epoch=2
TRANSITION_ROWS=2 (the decisive first transition is retained; a later offset transition is also classified)
NO_CROSS_EPOCH_NATIVE_COMPARISON=true
~~~

## AURA_RESULT

~~~text
AURA_RESULT=PASS_NATIVE_ONLY_CONTINUES_CROSS_DOMAIN_INVALIDATED
AURA_20MS_RULE=UNCHANGED_20000_US
~~~

Native detection/source accounting can continue through a mapping transition
when local dependencies are valid. A cross-domain disturbance record is
invalidated with CLOCK_MAPPING_EPOCH_TRANSITION instead of being labelled a
native source gap.

## C1_FAST_RESULT

~~~text
C1_FAST_RESULT=PASS_DEPENDENCY_AUDIT
~~~

The native/local baseline is eligible only when every required input is
explicitly native/local and existing source/freshness/finite/reset/frame/safety
gates pass. Cross-domain output fails closed while clock alignment is false; no
gain, control law or QoS changed.

## STATEBANK_RESULT

~~~text
STATEBANK_RESULT=PASS_NATIVE_IDENTITY_AND_CLOCK_GATE
~~~

StateBank stores native source identity and the versioned dual-domain fields;
its valid AURA sample requires both continuity and clock alignment. Candidate
history, release and H1000 identity remain native and reset-scoped.

## QUALIFIER_RESULT

~~~text
QUALIFIER_RESULT=PASS_12_RECORDS_0_ERRORS_ALL_VALID_H40_12_H80_12
~~~

## STRICT_PERSISTENCE_RESULT

~~~text
STRICT_PERSISTENCE_RESULT=PASS_ALLOW_NAN_FALSE_READBACK_RAW_UNCHANGED
NONFINITE_AUXILIARY_COUNT=34176
NONFINITE_REQUIRED_FIELD_FAILURES=0
~~~

The existing strict qualified-record projection remains after qualification;
auxiliary non-finite values carry explicit provenance and required scientific
fields are never sanitized.

## IMPLEMENTATION_SEMANTICS_PARITY

~~~text
IMPLEMENTATION_SEMANTICS_PARITY=PASS
RAW_SOURCE_SEMANTICS_UNCHANGED=true
T_D_T_A_SEMANTICS_UNCHANGED=true
TARGET_SEMANTICS_UNCHANGED=true
AURA_20MS_RULE_UNCHANGED=true
P1_P2_H1000_FAST_T1_C1_QOS_REFERENCE_TARGET_SAFETY_UNCHANGED=true
~~~

The change is limited to versioned observation/provenance, native-vs-mapped
gating and fail-closed record plumbing. No PX4 native message interface was
changed.

## AUTHORITY

~~~text
IMPLEMENTATION_AUTHORITY=OWNER_APPROVED_OPTION_A
REQUIRES_CONTROL_SEMANTIC_CHANGE=false
REQUIRES_SCIENTIFIC_TIMING_SEMANTIC_CHANGE=false
REQUIRES_INTERFACE_CHANGE=true (versioned observational record fields only)
RESEARCH_REQUEST=NONE
SEALED_STATE=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED
R1=NOT_AUTHORIZED
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
production_authority=false
NEXT_STATE=READY_FOR_OWNER_DECISION_ON_PROSPECTIVE_PILOT_RETRY_WITH_FAIL_CLOSED_MONITORING
~~~

## FINAL_DECISION

~~~text
DUAL_DOMAIN_TIMESTAMP_IMPLEMENTATION_QUALIFIED_AND_CAMPAIGN_SOAK_PASS_READY_FOR_OWNER_PILOT_RETRY_REVIEW
~~~
