# VNEXT WM1 V2R1 — cross-topic uXRCE update head-of-line root-cause closure

Ngày: 2026-08-28  
Phạm vi: forensic/runtime-mechanism (chỉ cơ chế); không randomized pilot, không
scientific block, không training/R1/SEALED; `production_authority=false`.

## Quyết định

```text
CROSS_TOPIC_UXRCE_ROOT_CAUSE_CLOSED_MECHANICAL_REPAIR_QUALIFIED_READY_FOR_OWNER_PILOT_RETRY_REVIEW
```

Một event runtime có per-topic ledger chứng minh `battery_status` là fd duy
nhất ready ngoài SensorCombined, nhưng branch của topic này hoàn tất trong
thời gian đo được bằng 0 us (CPU 25 us). Khoảng 148,000 us còn lại nằm sau
mọi topic branch, tại tám lệnh `PX4_INFO` đồng bộ của diagnostics tail. SITL
được launch với stdout là file và `px4_log_modulename()` gọi `fputs()` rồi
`fflush()`, nên đây là đường I/O có thể back-pressure chính thread uXRCE.

`_subs->update()` chạy đồng bộ ngay sau `px4_poll()` trong cùng vòng/thread;
do đó các SensorCombined publish trong khoảng này không thể được repoll cho
đến khi tail logging trả về. Đây là head-of-line mechanism đã được chứng
minh. Bản sửa chỉ giới hạn diagnostics snapshots ở hai snapshot startup,
không đổi poll interval, QoS, control hay scientific semantics; qualification
fresh sau sửa đạt continuity và không tái hiện gap >20 ms.

Mechanism mới là **tương thích nhưng chưa chứng minh đồng nhất** với incident
authoritative `7231..7256`: incident cũ không có per-call update/tail ledger.

## Trạng thái và boundary

```text
FIRST_LOST_PROVEN_BOUNDARY=UXRCE_NOT_POLLED
HEAD_OF_LINE_BLOCKING_MECHANISM=PROVEN
LOCALIZED_FAILURE_MECHANISM=UXRCE_SENSORCOMBINED_TOPIC_POLL_DISPATCH_GAP
UXRCE_CROSS_TOPIC_HOL_SEMANTICS=CLOSED
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
SEALED_STATE=LOCKED_PRE_EVALUATION
production_authority=false
```

Authoritative evidence remains unchanged: generation `7230` joined;
`7231..7256` had `SC_PUBLISH` but no `UXR_POLL/COPY/PREPARE/SEND`; generation
`7257` resumed. The current event is not used to rewrite that immutable
incident.

## Exact local source/build identities

Runtime is the local PX4 checkout, not upstream `main`:

```text
PX4_ROOT=/home/nahhao74/PX4-Autopilot
PX4_BASE_COMMIT=85df8c2281c2466b30a121b22b0bf33dc69bcfe4
DECISIVE_PRE_REPAIR_BINARY_SHA256=fad0ff6c4cf15f0fa9f7e36f06352770d7a3baa33a843f561b8a11581537d41d
DECISIVE_PRE_REPAIR_SENSOR_PIPELINE_AUDIT_HPP_SHA256=7c3bbd7b8bd9a384c579267b09cd93fd1b23b34e55b013bd8898fb255195514c
DECISIVE_PRE_REPAIR_DDS_TOPICS_H_EM_SHA256=ef61ca2cec06489c8806da5b55e552ee286aa329859993f890d6dfc48a0d5881
DECISIVE_PRE_REPAIR_GENERATED_DDS_TOPICS_H_SHA256=c57aa62683b61140a10c78747eed0e413dca4148d0de284f29f46a697a017fc2
CURRENT_REPAIRED_BINARY_SHA256=a06ed2c3714807257513c86e587c5d5e8402f33c7e0c0c37dc6cbe80009c57f9
CURRENT_BINARY_IDENTITY=a06ed2c3714807257513c86e587c5d5e8402f33c7e0c0c37dc6cbe80009c57f9 (repaired qualification)
CURRENT_SENSOR_PIPELINE_AUDIT_HPP_SHA256=0226f89b88cce7d7a0ff4e5ab096b4a2ba25dea1720258ff68d7fc949ea18087
CURRENT_UXRCE_DDS_CLIENT_CPP_SHA256=65b9433b35c832881a2f58670bb95b9acc66f18811e6890592d9f242a3f5e709
CURRENT_DDS_TOPICS_H_EM_SHA256=006a9f0ef62d67780be28ae1aa573462b4933222cbb9087c675afa44235dd370
CURRENT_GENERATED_DDS_TOPICS_H_SHA256=576cf91597d2b074c610d530fd8b61a0d11e3968326e827b5fef97a00a88eec3
DDS_TOPICS_YAML_SHA256=83a8da2307cfbbfc035ff1f05795ed9191589ca00f2d37b0645f26ac99ac8dc0
UORB_DEVICE_NODE_CPP_SHA256=60b1dfa9a64f10c81b93d085fd32c565abf5261f8d282910378cea8214f36daa
SUBSCRIPTION_INTERVAL_CPP_SHA256=f361b43ffbf699d98b3b879f906ac134ed48db08c708453ba630fee6799105a8
PX4_LOG_CPP_SHA256=9804158b0b0846261ad52e8779d9f9bd5c87f6e7ac3b171b1e93080a5957fc6d
GZBRIDGE_CPP_SHA256=0a4fd7b2a410b52f69a47558869cf2a085a0f442d9f9c0857161bafc25521477
MICROXRCE_CLIENT_COMMIT=711aef423edd1820347b866d1e4164832df35d04
MICROXRCE_CLIENT_CMAKE=2.4.0
MICROXRCE_AGENT_SOURCE_COMMIT=155cfaaf8b7abac2e85d4a62d3649b09ace0be55
MICROXRCE_AGENT_BINARY_SHA256=004a44d9b9465b4eebb9595213a3d86661c6e0b47876a7b0be0561a9ca6f8bdf
TRANSPORT=udp4 127.0.0.1:8888
ROS_RMW=rmw_fastrtps_cpp 8.4.3
AGENT_VERBOSE_RUNTIME=OFF
```

The decisive capture `_04` predates the logging cap (binary `fad0…` and
template hash recorded in the prior report). `_05_repair_qualification` uses
the current repaired binary `a06…`; this separation is intentional and roots
remain immutable.

## Generated subscription map

`dds_topics.h.em` constructs one `send_subscriptions[]` array (44 entries),
`fds[idx]` is subscribed in `init()`, and `create_data_writer()` uses the same
uORB id for the DDS object. Thus the runtime fd is `idx + 9`, the normal
interval is 10 ms, SensorCombined is 4 ms, and
`estimator_accel_bias_audit` is 0 ms.

## Decisive event

`DIAGNOSTIC_ROOT` is the immutable pre-repair capture
`/media/nahhao74/KINGSTON/wm1_v2r1_cross_topic_uxrce_hol_20260828_04`.
The complete PX4/poll/topic bundles share the outer trigger identity and are
kept as large artifacts.

```text
EVENT_TRIGGER_COMPLETENESS=PASS_PX4_POLL_TOPIC_BUNDLE_SHARED_EVENT_ID
DECISIVE_EVENT=diagnostic_event_id=5; bundle_suffix=6; poll_call_id=386926; loop_id=386926
PX4_BUNDLE_SHA256=06c2208512efbae9e0855d84a28172476e5fad81b91812875af93053cc9e2e19
POLL_BUNDLE_SHA256=9455a4456244bbc5d77bfa51c80140bb89057cf2748ab035718a9ef85bef3919
TOPIC_BUNDLE_SHA256=219018bd1790d008fb80abe6c6a2f189da1e3463b92028c1f2b3fcb35b446ef4
```

The selected `px4poll` row is:

| field | value |
|---|---:|
| `poll_call_id` / `loop_id` | 386926 |
| poll source interval | 654280000 → 654288000 us (8000 us) |
| `poll_return_value` | 1 |
| SensorCombined `fd/index` | 24 / 15 |
| SensorCombined `revents` | 0 (`POLLIN` absent) |
| `other_ready_count` | 1 |
| `other_revents_mask` | 16 (bit/index 4) |
| publisher/last-consumed generation | 130833 / 130833 |
| `update()` | 654288000 → 654436000 us (148000 us) |
| loop duration | 156000 us |

Therefore:

```text
DECISIVE_OTHER_READY_FD_INDEX=4
DECISIVE_OTHER_READY_TOPIC=battery_status
DECISIVE_OTHER_READY_DDS_OBJECT=21
DECISIVE_OTHER_READY_INTERVAL=10 ms
READY_TOPIC_COUNT=1
UPDATE_TOTAL_WALL_US=148000
SUM_TOPIC_WALL_US=0 (all topic branch endpoints quantized to 0 us)
MAX_TOPIC_WALL_US=0 (same quantization; no branch dominates)
LONGEST_TOPIC_UPDATE=battery_status (0 us recorded; tie only)
LONGEST_TOPIC_UPDATE_WALL_US=0
LONGEST_SUBSTAGE=OTHER (post-topic diagnostics tail)
```

The same pattern appeared earlier in this root at loop/poll `384149`, with
`battery_status` ready and a 28,000-us update. In the selected event the
SensorCombined publish ledger records generations `130834..130838` at
654288000..654308000 us while `update()` remains occupied; the next loop
dispatches/copies generation `130839` at 654436000 us. No SC UXR poll/copy/
prepare/send is present during the occupied interval.

## Per-topic and substage accounting

The per-topic ring for `poll_call_id=386926` contains only the battery branch:

```text
TOPIC_UPDATE_ENTER/EXIT       battery_status 654288000→654288000 us; CPU 25 us
TOPIC_ORB_COPY_ENTER/EXIT     battery_status 654288000→654288000 us; CPU 3 us
TOPIC_PREPARE_ENTER/EXIT      battery_status 654288000→654288000 us; CPU 2 us
TOPIC_SERIALIZE_ENTER/EXIT    battery_status 654288000→654288000 us; CPU 0 us
TOPIC_FLUSH_ENTER/EXIT        battery_status 654288000→654288000 us; CPU 15 us
UPDATE_DIAGNOSTICS_ENTER      654288000 us; CPU 20446573 us
UPDATE_DIAGNOSTICS_EXIT       654436000 us; CPU 20446659 us; wall 148000 us
```

The hrt resolution records topic branch wall times as 0 us; this is an upper
bound/quantization, not a claim that the operations mathematically take no
time. The unlabelled tail is the only interval spanning the source gap.

```text
OTHER_TOPIC_ORB_COPY_ROLE=NOT_CAUSAL
XRCE_FLUSH_ROLE=NOT_CAUSAL
SERIALIZATION_ROLE=NOT_CAUSAL
LONG_TOPIC_THREAD_CLASS=OFF_CPU_OR_DESCHEDULED
```

The tail consumes only 86 us of measured uXRCE thread CPU during 148 ms wall
time. No kernel scheduler trace or lock-owner record exists, so this is
classified as off-CPU/blocking I/O, not as a narrower scheduler primitive.

## Source-grounded HOL proof and cause

`uxrce_dds_client.cpp` lines 523–559 execute, in one thread and one loop:

```text
px4_poll(_subs->fds, 44, timeout)
if (poll > 0) _subs->update(...)
// only after update returns: uxr_run_session_timeout(...)
```

`dds_topics.h.em` lines 189–437 iterate all ready fd branches; lines 449–525
then execute the diagnostics tail in the same `update()` call. Thus a ready
non-SC fd can prevent the loop from returning to `px4_poll()` and can delay a
newly published SC sample. This closes:

```text
UXRCE_CROSS_TOPIC_HOL_SEMANTICS=CLOSED
CURRENT_EVENT_MECHANISM=CROSS_TOPIC_UPDATE_HEAD_OF_LINE_BLOCKING_BEFORE_SENSORCOMBINED_DISPATCH
REPRODUCED_MECHANISM=CROSS_TOPIC_UPDATE_HEAD_OF_LINE_BLOCKING_WITH_POST_TOPIC_DIAGNOSTIC_LOGGING_STALL
```

The final blocking operation is source-grounded: `px4_log_modulename()` uses
`fputs(buf, out)` and, for SITL, `fflush(out)` (`platforms/common/px4_log.cpp`
lines 152 and 173–177). The vehicle launcher redirects PX4 output to a
regular `px4.log` (`1_AURA/scripts/start_vehicle.sh` lines 340–347). The
eight diagnostic `PX4_INFO` calls therefore synchronously exercise that
stdout/file path. The low CPU/wall ratio and exact tail interval prove the
narrow cause:

```text
DEEP_ROOT_CAUSE=UXRCE_UPDATE_POST_TOPIC_DIAGNOSTIC_LOGGING_STDOUT_BACKPRESSURE
DEEP_ROOT_CAUSE_CLASS=IMPLEMENTATION_INFRASTRUCTURE_OBSERVABILITY_PATH
```

This is an observability-path defect, not DDS transport loss, BEST_EFFORT
evidence, a SensorCombined uORB lock stall, or a control-law change.

## Poll/readiness and interval semantics

The generated source sets `orb_set_interval(fd, 4)` only for SensorCombined
and 10 ms for other topics. The decisive delay begins after `px4_poll()`
returns and after the other topic branch is entered; SC interval is not the
cause of the observed tail stall:

```text
POLL_INTERVAL_ROLE=NOT_CAUSAL
```

The source evidence proves serialized update/HOL behavior, but does not claim
that the historical 7231–7256 event necessarily used this same tail.

```text
MECHANISM_MATCH_TO_AUTHORITATIVE_7231_7256=COMPATIBLE_NOT_PROVEN
```

## Repair and fresh qualification

```text
IMPLEMENTATION_REPAIR=APPLIED_OUTSIDE_IMMUTABLE_ROOT
REPAIR=
  cap synchronous sensor-pipeline PX4_INFO snapshots to the first two
  startup snapshots (`sensor_pipeline_audit_snapshot_count < 2`);
  retain in-memory counters/rings; no poll/rate/QoS/control change
```

Deterministic source regression passed: the cap is present, there is exactly
one increment, and both diagnostic-tail ring stages remain guarded. The
current binary was rebuilt, and `py_compile` plus focused trace tests passed.

```text
REGRESSION_TEST_RESULT=PASS
FOCUSED_TESTS=24 passed (tests/test_vnext_data0_nominal.py tests/test_vnext_data0_trace.py)
```

Fresh post-repair non-scientific qualification root:

```text
NONSCIENTIFIC_REPAIR_QUALIFICATION_ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_cross_topic_uxrce_hol_20260828_05_repair_qualification
NONSCIENTIFIC_REPAIR_QUALIFICATION=PASS
validation.json.pass=true
diagnostic_only=true
collector_exit=0
sensor_pipeline_instrumentation=true
rows_written=111662
sensor_combined_rows=19498
sensor_combined_max_gap=8000 us
aura_gap=0
upstream_lag_trigger_count=0
uxrce_prepare_failures=0
writer_errors=0
writer_drops=0
sequence_gaps=0
cpuload=1.1176..1.24 (observed cpuload topic)
```

The repaired qualification was a bounded Moving mechanism run (~70.5 s), not
a scientific campaign. It did not consume the manifest and did not prove the
historical defect impossible; it proves the implementation-preserving cap
removes the reproduced logging stall under this qualification.

## Required verdicts

```text
FIRST_LOST_PROVEN_BOUNDARY=UXRCE_NOT_POLLED
HEAD_OF_LINE_BLOCKING_MECHANISM=PROVEN
UXRCE_CROSS_TOPIC_HOL_SEMANTICS=CLOSED
DECISIVE_OTHER_READY_TOPIC=battery_status
UPDATE_TOTAL_WALL_US=148000
LONGEST_SUBSTAGE=OTHER
OTHER_TOPIC_ORB_COPY_ROLE=NOT_CAUSAL
XRCE_FLUSH_ROLE=NOT_CAUSAL
SERIALIZATION_ROLE=NOT_CAUSAL
LONG_TOPIC_THREAD_CLASS=OFF_CPU_OR_DESCHEDULED
POLL_INTERVAL_ROLE=NOT_CAUSAL
CURRENT_EVENT_MECHANISM=CROSS_TOPIC_UPDATE_HEAD_OF_LINE_BLOCKING_BEFORE_SENSORCOMBINED_DISPATCH
MECHANISM_MATCH_TO_AUTHORITATIVE_7231_7256=COMPATIBLE_NOT_PROVEN
DEEP_ROOT_CAUSE=UXRCE_UPDATE_POST_TOPIC_DIAGNOSTIC_LOGGING_STDOUT_BACKPRESSURE
DEEP_ROOT_CAUSE_CLASS=IMPLEMENTATION_INFRASTRUCTURE_OBSERVABILITY_PATH
SENSORCOMBINED_GAP_RESULT=PRE_REPAIR_DECISIVE_EVENT_148000_US; POST_REPAIR_MAX_8000_US
AURA_GAP_RESULT=PRE_REPAIR_TRACE_NO_AURA_GT20MS_COUNTER; POST_REPAIR_0
UPSTREAM_LAG_RESULT=PRE_REPAIR_EVENTS_OBSERVED; POST_REPAIR_0
UXRCE_PREPARE_RESULT=0_FAILURES
INSTRUMENTATION_OVERHEAD=BOUNDED_ACCEPTABLE
IMPLEMENTATION_REPAIR=MINIMAL_LOGGING_CAP_ONLY
REGRESSION_TEST_RESULT=PASS
NONSCIENTIFIC_REPAIR_QUALIFICATION=PASS
RESEARCH_REQUEST=NONE
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
SEALED_STATE=LOCKED_PRE_EVALUATION
production_authority=false
NEXT_STATE=OWNER_REVIEW_FOR_PILOT_RETRY_WITH_EXISTING_FAIL_CLOSED_MONITORING
```

No randomized pilot is authorized by this report. Any future pilot remains a
separate owner decision under the existing prospective fail-closed source
monitoring contract.
