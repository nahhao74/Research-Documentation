# Scheduled-native frontier binding: offline assessment

TASK_ID=PHASE_D_SCHEDULED_FRONTIER_AUDIT_20260908
RESULT=PROTOCOL_SUPPORTED_END_TO_END_BINDING_NOT_QUALIFIED
RUNTIME_STARTED=false
PRODUCTION_IMPLEMENTATION_CHANGED=false

## Reusable owner and executed qualification

`collector.native_truth_command_payload` already emits v2 when an explicit
`apply_at_sim_time_ns` is supplied. The native protocol retains that target.
The existing C++ protocol suite was compiled and executed offline:

```sh
c++ -std=c++17 -I1_AURA/gazebo_native_truth/include \
  1_AURA/gazebo_native_truth/tests/native_truth_protocol_test.cc \
  -o /tmp/phase_d_native_protocol_audit_test
/tmp/phase_d_native_protocol_audit_test
```

NATIVE_PROTOCOL_REGRESSION=PASS

This tests lifecycle ordering, malformed/reversed generation handling and
scheduled metadata including zero-time rejection. It is NOT an execution of
Gazebo or qualification of physical application timing.

Source inspection of `NativeDisturbanceSystem.PopDuePending` establishes:
before target the queued command stays pending; at target it is processed;
after target it is rejected as `scheduled_application_late`. The native
application event remains stamped by `PreUpdate` simulation time.

## Why merely enabling v2 is insufficient

The ordinary Phase-D path has a host-monotonic envelope and sends immediate
v1 commands. The existing R10 path instead computes the first active wrench
sample on a deterministic grid and queues an explicit application target.
That changes timing ownership and potentially the first applied force sample.
The remainder of the ordinary envelope still uses host phase time. Switching
only onset to R10 therefore does not prove identical rise/change/clear timing.

COMPARABILITY_WITH_ORDINARY_PHASE_D=NOT_PROVEN
PROSPECTIVE_SCHEDULING_BINDING=NOT_FROZEN

## Separate causal completeness problem

An exact future target T does not by itself prove all evaluations with source
time less than T are known and persisted at an earlier emission decision D.
An evaluation near T can still be unproduced, in transport, or pending C1/E8
persistence at D. Received sequence contiguity cannot detect a missing suffix.

This is not an assertion that no individual opportunity can ever pass: a
discrete source schedule may have produced its last pre-T sample already.
It is an assertion that an owner-bound upper watermark and completed exact
downstream accounting must prove that fact. Neither a future scheduled target
nor the protocol regression supplies that proof. Unknown must forbid F0.

Consequently, adding a scheduled timestamp alone cannot qualify the requested
exact execution harness. A fixed delay, simulated-time pause, shortened prefix,
or after-the-fact completion would violate the currently approved constraints.

## Recommended bounded design decision

Reuse native v2 serialization; do not create another transport. Before code,
select and authorize the precise measurement/authorization boundary:

1. Keep the full pre-T population and pre-emission decision. Implement an
   owner upper-watermark plus live flush/reconciliation. Accept that a missing
   seal at the fixed opportunity aborts without F0. No feasibility/coverage
   promise is justified yet; an offline adversarial harness must include a
   delayed final evaluation and must abort it.
2. If the goal requires reliable scheduled execution despite this transport
   gap, a native two-stage prepare/commit boundary would need separate
   authorization and source-time qualification. Moving authorization from
   command emission to native application changes the currently approved
   boundary; it cannot be disguised as the same measurement-only repair.

Neither option is implemented or authorized for runtime by this report.
No extra dwell or performance threshold is proposed. Historical 7 launches /
6 F0-reached attempts / 5 recorded usable conditions remain unchanged.

SCHEDULED_FRONTIER_BINDING=NOT_QUALIFIED
PREWIND_PREFIX_SEAL=NOT_IMPLEMENTED
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
SCIENTIFIC_FREEZE_CHANGED=false
MEASUREMENT_IMPLEMENTATION_BINDING_CHANGED=false
EXECUTION_LINEAGE_CHANGED=false
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
READY_FOR_EXECUTION_BINDING_REVIEW=false

The lead ran the protocol regression and source audit. Luna was delegated
the bounded assessment; no executor qualification report is assumed here.
