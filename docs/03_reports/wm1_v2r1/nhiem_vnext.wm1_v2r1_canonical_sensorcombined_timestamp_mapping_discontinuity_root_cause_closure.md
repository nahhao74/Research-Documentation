# VNEXT WM1 V2R1 — Canonical SensorCombined timestamp-mapping discontinuity

Ngày: 2026-08-28  
Vai trò: audit source/time-domain và replay, không chạy pilot mới  
`production_authority=false`

## Quyết định

```text
TIMESTAMP_MAPPING_ROOT_CAUSE_CLOSED_OWNER_TIMESTAMP_SEMANTICS_REVIEW_REQUIRED
```

Evidence đã đóng được mâu thuẫn `183018 us` canonical với chuỗi PX4 native
8-ms. `SensorCombined` native source/generation không mất; một bước
`TimesyncStatus`/uXRCE offset xảy ra trong cùng session nhưng AURA chỉ nhận
offset mới sau khi sample đã được gửi. AURA vì vậy dùng prior offset cũ cho
sample đã được PX4 serialize bằng offset mới. Khoảng nhảy được tạo ở canonical
mapping, không phải ở PX4 producer, uXRCE poll/copy, DDS transport hay ROS
callback.

Root scientific cũ vẫn là bằng chứng immutable và vẫn invalid. Việc phân rã
timestamp này không hồi tố chứng nhận R2, không gộp root, và không thay đổi
20-ms gate, AURA validity, H1000, FAST/T1/C1, action hay target semantics.

## 1. Root và evidence boundary

```text
FAILED_ROOT_STATUS=IMMUTABLE_INVALID_SCIENTIFIC_CONTRACT
ROOT_VALIDITY_CLASS=INVALID_SCIENTIFIC_CONTRACT
SCIENTIFIC_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260828_uxrce_logging_hol_retry_01
R2_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260828_uxrce_logging_hol_retry_01/WM1V2R1_RAND_A_CALM_R2
IMMUTABLE_ROOT_MODIFIED=false
```

Incident pair trong R2 là `32420000 -> 32603018 us`; AURA đánh dấu
`source_gap=true`, `canonical_gap=true`, `valid=false` sau downstream
reprime. R2 đã qua `FIRST_SCIENTIFIC_SLOT_T_D_FREEZE`, nên gate violation vẫn
giữ nguyên `INVALID_SCIENTIFIC_CONTRACT` dù nguyên nhân mapping được làm rõ.

Evidence artifact hashes giữ nguyên:

```text
sensor_pipeline_event_bundle.aura.1.json   3a263ef28e13f45292aae32a013c07554cbd7721d3b3b5574634dfb1517842dc
sensor_pipeline_event_bundle.px4.1.jsonl   689ebb7cc42e13a35483f0c5fd412497236b69c8dbe35345c299d56dc1a1ed30
sensor_pipeline_event_bundle.px4poll.1.jsonl c735dfd7bf2dae33362b65bccf216010a85f9c476e800d30104610c86b1eca64
trace/trace.jsonl                           f052da5ca39af9a20e44b06966bb2eca8670d75a7687bcdec52c7bc45528ac83
timesync_observations.jsonl                 972a27c440de7b43df89a86f9af3844f01f220ae51bb09e9a732bcff38ebd4c7
```

## 2. Exact local source/build identities

Runtime authority is the dirty local PX4 checkout at commit
`85df8c2281c2466b30a121b22b0bf33dc69bcfe4`, not PX4 `main` or an upstream
issue report.

| component | local identity |
|---|---|
| PX4 executable | `a06ed2c3714807257513c86e587c5d5e8402f33c7e0c0c37dc6cbe80009c57f9` |
| PX4 `uxrce_dds_client.cpp` | `65b9433b35c832881a2f58670bb95b9acc66f18811e6890592d9f242a3f5e709` |
| PX4 `dds_topics.h.em` | `006a9f0ef62d67780be28ae1aa573462b4933222cbb9087c675afa44235dd370` |
| generated `dds_topics.h` | `576cf91597d2b074c610d530fd8b61a0d11e3968326e827b5fef97a00a88eec3` |
| generated `uORB/ucdr/sensor_combined.h` | `65e216ca810c9b33ce84aebc747af730ebba77796dd7f2a55fc67c13d43d1b1e` |
| AURA `moving_runtime.py` | `b08dfa7b56745426ae5b32d225bfd2e0a2548a3e978ef5ebd39ae72bece12862` |
| AURA `moving_runtime_node.py` | `d62c3034cdf75329fd7fb6665a7a08b290a9b720e53096fcba442f726c41f6d9` |
| AURA `wm1_clock_bridge.py` | `2d111afdd097018a017d9e9764623965fa04cdbd7f6e2906ef6dacee33b29a8a` |
| temporal probe | `46b4d830cdf83e5f753d4914a809f6ac71796e84c0a05375ba44895ed573b8f0` |
| `dds_topics.yaml` | `83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0` |
| manifest | `253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7` |
| vendored Micro XRCE-DDS Client | commit `711aef423edd1820347b866d1e4164832df35d04`, CMake 2.4.0 |
| MicroXRCEAgent | commit `155cfaaf8b7abac2e85d4a62d3649b09ace0be55`, binary `004a44d9b9465b4eebb9595213a3d86661c6e0b47876a7b0be0561a9ca6f8bdf` |
| transport / RMW | UDP4 `127.0.0.1:8888` / `rmw_fastrtps_cpp 8.4.3` |

Local source chain is explicit: `SensorCombined` subscription uses
`orb_set_interval(..., 4 ms)`; generated update enters only for the
SensorCombined fd `POLLIN`; `orb_copy` is followed by
`uxr_prepare_output_stream`, serialization and flush. The serializer writes
`topic.timestamp + session->time_offset/1000`. `on_time()` can update
`session.time_offset` from the local Timesync filter.

## 3. Native sequence and event rows

```text
RAW_GENERATION_CONTINUITY=PASS_NATIVE_PX4
RAW_SENSORCOMBINED_MAX_DELTA_US=8000 (PX4 native source; 4/8-ms cadence)
RAW_SENSORCOMBINED_GAP_GT_20MS=0 (PX4 native source)
RAW_SENSORCOMBINED_WIRE_FIELD_MAX_DELTA_US=183018 (mixed wire representation)
RAW_SENSORCOMBINED_WIRE_FIELD_GAP_GT_20MS=1 (not a native-source gap)
```

The PX4 bundle contains 121 `SC_PUBLISH` events over `32.2–32.8 s`; native
source deltas have maximum `8000 us`, no delta over `20000 us`, and
`audit_generation` increments by one. Decisive native rows are:

| audit generation | uORB publisher generation | PX4 native source us |
|---:|---:|---:|
| 6452 | 6265 | 32416000 |
| 6453 | 6266 | 32420000 |
| 6454 | 6267 | 32428000 |
| 6455 | 6268 | 32432000 |
| 6456 | 6269 | 32436000 |

The same PX4/uXRCE poll ledger shows the downstream path active, with no
prepare failure:

| poll record | poll return | SC revents | publisher gen/source us | last copied source us | lag |
|---:|---:|---:|---|---|---:|
| 18150 | 7 | `POLLIN` | 6453 / 32420000 | 32416000 | 1 / 4000 us |
| 18156 | 2 | `POLLIN` | 6454 / 32428000 | 32420000 | 1 / 8000 us |
| 18160 | 9 | `POLLIN` | 6455 / 32432000 | 32428000 | 1 / 4000 us |

No native producer or uXRCE poll/copy boundary disappears at the R2 pair.
The ordered SensorCombined rows retained in the replay workspace are the
complete 120-row `32.2–32.8 s` ROS/AURA window:

```text
/media/nahhao74/KINGSTON/wm1_v2r1_timestamp_mapping_replay_20260828_01/ordered_sensorcombined_rows.jsonl
sha256=91e0f971022cc8c44c1da9323160cda731e93a6c80bc15e66594b93159b0e87c
```

Decisive rows (ROS callback sequence, raw wire field, runtime canonical field,
offset actually used, host receipt) are:

| callback | raw `SensorCombined.timestamp` | runtime mapped us | offset used us | ROS receipt monotonic ns | AURA host ns | accepted |
|---:|---:|---:|---:|---:|---:|---|
| 5010 | 1787910190946130 | 32420000 | -1787910158526130 | 28669869392482 | 28669869227545 | true |
| 5011 | 1787910191129148 | 32603018 | -1787910158526130 | 28669877442491 | 28669877599909 | true, gap trigger |
| 5012 | 1787910191133181 | 32432033 | -1787910158701148 | 28669882115529 | 28669882402228 | false, reversed |

`accelerometer_timestamp_relative=0` in all three messages, so the frozen
corrected accelerometer time (`timestamp + relative`) equals the selected
`timestamp` for this incident.

## 4. Exact AURA timestamp path and gate domain

```text
AURA_CANONICAL_TIMESTAMP_SOURCE=
moving_runtime_node._imu_callback
 -> runtime_node._sensor_timestamp_us(message)
 -> getattr(message, "timestamp_sample", message.timestamp)
 -> CausalTimesyncTimestampMapper.canonicalize(raw, callback_monotonic_ns)
 -> moving_runtime_node._imu_timestamp_guard.gap_exceeds(mapped, 20000)
```

The local `SensorCombined` ROS type has `timestamp`, not
`timestamp_sample`; therefore the selected field is the wire
`SensorCombined.timestamp`. The event has relative timestamp zero. The mapper
returns either native `px4_boot_us` for values below the agent-epoch floor or
`raw + latest already-received estimated_offset_us` for agent-epoch values.

```text
SOURCE_GAP_GATE_DOMAIN=PX4_BOOT_US_CANONICAL_MAPPED_FRONTIER
```

This is a mapped PX4-boot-compatible frontier, not a direct native PX4
publication counter. The gate compares the mapped value in
`gap_exceeds()`; it does not compare the native `SC_PUBLISH` value.

## 5. Timestamp-domain table

```text
TIMESTAMP_DOMAIN_TABLE=SEE_EXACT_FIELD_DOMAIN_TABLE_BELOW
```

| field/path | exact domain in this root | evidence/meaning |
|---|---|---|
| PX4 `sensor_combined_s.timestamp` at `SC_PUBLISH` | `px4_boot_us` | native `Sensors` publication source; 32420000→32428000 is 8000 us |
| uXRCE serialized `SensorCombined.timestamp` on ROS wire | `agent_epoch_us` when nonzero `session.time_offset`; transient `px4_boot_us` when offset is zero | generated serializer adds current offset to every uint64 timestamp |
| ROS `receipt_monotonic_ns` | `host_monotonic_ns` | executor receive timestamp; not source time |
| AURA raw timestamp | same wire domain as ROS field | retained raw before mapping; magnitude identifies representation only |
| AURA canonical/mapped timestamp | `px4_boot_us` comparable source domain | raw agent epoch plus prior offset, or native raw below floor |
| `TimesyncStatus.timestamp_us` in retained log | serialized wire representation | line 24 is native 32428000 because estimated offset is zero; lines 23/25 are epoch-like |
| `TimesyncStatus.remote_timestamp_us` | agent/remote epoch us | remote endpoint timestamp |
| `estimated_offset_us`, `observed_offset_us` | signed offset in microseconds | offset quantity, not a timestamp domain |

The raw trace jump around `58420000 -> 1787910217308387` is not unrelated
noise. It is the same mixed-representation transition: a zero-offset
serialized timestamp is native PX4 boot time, then a later nonzero-offset
serialization is agent epoch. The local serializer and the AURA mapper both
retain this representation distinction. It is not the 32.42-s pair and is not
evidence of a producer gap.

## 6. TimesyncStatus ledger and causality

All decisive records have controller session `284000`, reset `0`, and the same
clock epoch
`clock_epoch_px4_session_timesync_v1:timesync_status_dds_v1:session:284000:protocol:2`.

| retained line | host observation monotonic ns | `timestamp_us` | estimated offset us | representation | causal relation |
|---:|---:|---:|---:|---|---|
| 23 | 28668868649677 | 1787910189950130 | -1787910158526130 | agent epoch + offset | prior used by callback 5010/5011 |
| 24 | 28669879878453 | 32428000 | 0 | native PX4 | after callback 5011; `observe()` intentionally ignores zero |
| 25 | 28669880030462 | 1787910191129148 | -1787910158701148 | agent epoch + offset | after callback 5011; used by callback 5012 |
| 26 | 28669896808710 | 1787910191141235 | -1787910158701235 | agent epoch + offset | later smoothing |

```text
TIMESYNC_OFFSET_BEFORE=-1787910158526130 us
TIMESYNC_OFFSET_AFTER=-1787910158701148 us
TIMESYNC_OFFSET_STEP=-175018 us
TIMESYNC_SAMPLE_CAUSALITY=PASS_PRIOR_ONLY;new offset is received after row 5011; zero sample is ignored by local mapper
```

The line-24 zero-offset status is received at
`28669879878453 ns`, about `2.278544 ms` after the AURA callback for row 5011;
the nonzero line-25 status is received about `2.430553 ms` after that callback.
Thus no future Timesync sample was used to produce the erroneous 32603018
value. This is a causal ordering defect between the sender's current offset
and the receiver's asynchronously observed offset, not a future-sample or
interpolation defect.

## 7. Numeric gap decomposition

The raw wire field is the serialized value, while the native source is
retained independently in PX4/uXRCE counters. Both are reported to avoid
calling a wire-domain step a physical source gap.

```text
RAW_SOURCE_DELTA_US=8000 (native PX4: 32428000 - 32420000)
RAW_WIRE_FIELD_DELTA_US=183018 (1787910191129148 - 1787910190946130)
HOST_RECEIPT_DELTA_US=8050.009 (ROS receipt); 8372.364 (AURA callback)
MAPPED_SOURCE_DELTA_US_RUNTIME=183018 (both values mapped with stale old offset)
MAPPED_SOURCE_DELTA_US_CORRECTED=8000 (second value mapped with new offset)
OFFSET_DELTA_US=-175018
```

Exact arithmetic:

```text
1787910190946130 + (-1787910158526130) = 32420000
1787910191129148 + (-1787910158526130) = 32603018   # runtime false jump
1787910191129148 + (-1787910158701148) = 32428000   # native next source
183018 + (-175018) = 8000                             # raw + offset step
1787910191133181 + (-1787910158701148) = 32432033   # next sample
```

The host interval is only milliseconds while the runtime canonical interval is
183018 us. The replay artifact records the full raw/mapped ordered sequence and
the native PX4 publication rows; no interpolation or retroactive timeline
repair was used.

```text
GAP_DECOMPOSITION=RAW_WIRE_DELTA 183018 + OFFSET_STEP -175018 = NATIVE_DELTA 8000; runtime used old offset for the first post-step sample
```

## 8. Frozen prior-only mapper replay

`CausalTimesyncTimestampMapper.observe()` stores only the latest offset already
received and ignores a transient zero. Replaying the decisive pair gives:

```text
previous: (32420000, "agent_epoch_plus_timesync_offset", -1787910158526130)
post-step sample before new status: (32603018, "agent_epoch_plus_timesync_offset", -1787910158526130)
zero status: old offset retained
next sample after new status: (32432033, "agent_epoch_plus_timesync_offset", -1787910158701148)
```

```text
TIMESTAMP_MAPPER_REPLAY=PASS_DECISIVE_PRIOR_ONLY
RUNTIME_REPLAY_PARITY=PASS_VALUE_LEVEL_WITH_RETAINED_RUNTIME_OFFSET_PROVENANCE
```

The retained AURA event rows contain the offset state actually consumed by the
runtime; raw-plus-that-offset equals the mapped value for all 120 replay rows.
The separate TimesyncStatus recorder is asynchronous, so its receipt times
cannot replace the runtime callback state for a byte-level replay of every
later row. This limitation does not affect the decisive pair: line 25 is
strictly after callback 5011, and the pair arithmetic is exact.

Replay workspace and machine assertions:

```text
/media/nahhao74/KINGSTON/wm1_v2r1_timestamp_mapping_replay_20260828_01
replay_summary.json sha256=8f143cb93a8e9b73d2b465ef9bd97bb3ecc29c0166335c2d70d6993783bf5a0d
assertions=native_no_gap;old_offset_prev;old_offset_false_jump;new_offset_native_next;offset_step_identity;runtime_value_parity=PASS
```

## 9. Mapper lifecycle and downstream response

```text
MAPPER_LIFECYCLE_RESULT=
one controller session 284000/reset 0 and one clock epoch; no cross-session
state leak or reset transition; PX4 sender offset changed within the session
before the corresponding TimesyncStatus DDS callback reached AURA; AURA
retained the prior offset for the first post-step sample, then applied the new
offset and rejected the resulting reversed sample.
```

The source-grounded downstream chain is:

```text
sender offset step + asynchronous status visibility
→ raw wire field remains epoch-like but changes offset
→ prior-only AURA mapping produces 32603018
→ mapped gap > 20000 us and next mapped sample reverses
→ detector.reprime / physical_residual_unqualified
→ AURA valid=false, fresh=false, w20_valid=false
→ C1 source/gate fail-closed
→ E8 R2 b04 generation 900400 has no exact accepted status and times out
```

```text
FIRST_LOST_PROVEN_BOUNDARY_CURRENT_EVENT=OTHER_EXACT_SOURCE_GROUNDED_CAUSE_CANONICAL_MAPPING_STAGE;NO_PHYSICAL_UPSTREAM_LOSS_PROVEN
TIMESTAMP_ROOT_CAUSE=TIMESYNC_OFFSET_STEP_INJECTED_CANONICAL_GAP
ROOT_CAUSE_CLASS=IMPLEMENTATION_INFRASTRUCTURE_TIMESTAMP_MAPPING
AURA_FAIL_CLOSED_BEHAVIOR=EXPECTED_CONTRACT_RESPONSE
C1_DOWNSTREAM_RESPONSE=SOURCE_VALID_FALSE_SOURCE_FRESH_FALSE_GATE_INVALID_FAIL_CLOSED
E8_DOWNSTREAM_RESPONSE=R2_B04_GENERATION_900400_ACCEPTED_STATUS_TIMEOUT_AFTER_SOURCE_GATE_INVALID;DOWNSTREAM_SYMPTOM
```

This does not reuse the historical `UXRCE_NOT_POLLED` boundary: the current
R2 event has native PX4 `SC_PUBLISH`, uXRCE `POLLIN/COPY/PREPARE/SEND/FLUSH`
and short host receipt intervals at the pair. The historical 175602-us event
remains `UNRESOLVED_IMMUTABLE`; no same-mechanism claim is made.

## 10. Semantic and repair boundary

```text
RAW_SOURCE_SEMANTICS_UNCHANGED=true
T_D_T_A_SEMANTICS_UNCHANGED=true
TARGET_SEMANTICS_UNCHANGED=true
AURA_20MS_RULE=UNCHANGED
SCIENTIFIC_TIME_SEMANTICS_CHANGED=false
```

No repair was applied. A future correction must choose, under owner review,
how a mid-session uXRCE timestamp-offset transition is made atomic with its
source provenance, or whether the scientific gate must use a different frozen
source field. Choices such as freezing the sender offset, carrying offset
provenance, rejecting mixed wire representations, switching the gate to native
source time, or accepting a transition as continuous change the timestamp
source/validity contract. They are therefore not silently treated as a
mechanical patch.

```text
IMPLEMENTATION_REPAIR=NONE_APPLIED;OWNER_TIMESTAMP_SEMANTICS_REVIEW_REQUIRED
```

Deterministic checks performed without changing semantics:

```text
REGRESSION_TEST_RESULT=PASS_OFFLINE_EXACT_DECOMPOSITION_AND_PRIOR_ONLY_ASSERTIONS;EXISTING_MOVING_RUNTIME_AND_TARGET_TESTS=25_PASS;NO_SOURCE_REPAIR
FAILED_EVENT_REPLAY_RESULT=RAW_SEQUENCE_REPLAY_PASS;MAPPER_REPLAY_PASS_FOR_DECISIVE_PAIR;AURA_GAP_REPLAY_PASS;NO_RAW_ARTIFACT_MUTATION
NONSCIENTIFIC_QUALIFICATION_RESULT=NOT_RUN_NO_REPAIR_APPLIED;NO_NEW_RUNTIME_ROOT
```

## 11. Required fields and authority

```text
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
RESEARCH_REQUEST=NONE
SEALED_STATE=LOCKED_PRE_EVALUATION;SEALED_PAYLOAD_OPENED=false
production_authority=false
NEXT_STATE=OWNER_TIMESTAMP_SEMANTICS_REVIEW_REQUIRED_BEFORE_ANY_FRESH_ROOT
```

Không có model training, R1, SEALED read, QoS/control/action envelope,
FAST/T1/C1, H1000, reference, target hoặc safety-radius thay đổi. AURA
20-ms rule và scientific root validity không được nới lỏng.
