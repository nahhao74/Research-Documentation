# Prewind prefix seal: source-frontier binding audit

TASK_ID=PHASE_D_PREWIND_PREFIX_SOURCE_BINDING_20260908
RESULT=BLOCKED_EXACT_SCHEDULED_NATIVE_FRONTIER_NOT_BOUND
IMPLEMENTATION_COMPLETE=false
RUNTIME_STARTED=false
REFREEZE_STARTED=false

## Accepted semantics

The approved population is `[V2_ELIGIBLE_SOURCE_US, SCHEDULED_F0_SOURCE_US)`.
It cannot be shortened, retried, or delayed. Prefix persistence must be proven
before native command emission; shutdown remains an independent row obligation.
No 20-second prewind dwell is required. This audit does not reopen those rules.

## First unresolved source binding

The existing ordinary Phase-D path does not expose a prospectively scheduled
native source frontier:

- `collector._set_phase` records `time.monotonic()` as `phase_started`.
- `collector._phase_time` uses elapsed host monotonic time.
- `_disturbance` evaluates the frozen event envelope in that phase time.
- `_publish_wrench` calls `_publish_native_truth_command` without a scheduled
  application argument.
- `native_truth_command_payload` therefore emits v1; it includes no
  `apply_at_sim_time_ns`.
- `NativeDisturbanceSystem.OnCommand` queues that command. `PreUpdate` pops
  and applies it; native truth is stamped using `UpdateInfo.simTime` then.
- The frozen metric defines F0 from that native application timestamp, not
  from host emission time or the collector's most recent sensor timestamp.

Therefore no exact `SCHEDULED_F0_SOURCE_US` has been identified before the
ordinary command emission. A latest-sensor substitution would change prefix
membership. A host-time extrapolation would introduce an unqualified mapping.
Evaluations between emission and native application cannot simply be excluded.

The optional R10 path `_prepare_r10_native_onset` does provide an explicit
application target and emits v2. It is opt-in and is not the current ordinary
Phase-D path. Enabling it is a scheduling change, not merely a writer seal.
Native scheduling rejects late commands, so its acceptance behavior also
requires separate qualification before adoption.

## Executed offline checks

A direct AST inspection of the actual `_publish_wrench` found exactly one
native publish call with no scheduling keyword. Executing the actual source
`native_truth_command_payload` with its ordinary arguments produced:

```text
v1|1|1|DISTURBANCE_ONSET|i|e|p|m|base_link|0.7|0.0|0.0|0.0|0.0|0.0|world_enu
```

ORDINARY_CALL_NO_SCHEDULE_ARGUMENT=PASS
ACTUAL_PAYLOAD_DEFAULT_V1=PASS

Read-only original slot-1 trace sequence 27477 independently records
DISTURBANCE_ONSET at sim_time_ns=19532000000, iteration=4883, command_sequence=1,
generation=1. This is retained application evidence, not a pre-emission
scheduled target. No historical artifact was changed.

## Additional seal requirements still open

The trace writer increments persistence counters after `write()` but before
periodic `flush()`. A prefix seal must use an actual writer-owned flush and
accounting boundary, not those counters alone. Owner sequence evidence must
also bound the end of the prefix: contiguous received records do not prove
that no trailing owner evaluation is missing. These are implementation work
items, not permission to reduce the population.

EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
PREWIND_POPULATION_MEMBERSHIP_EXACT=UNQUALIFIED_END_FRONTIER
PREWIND_PREFIX_SEAL=NOT_IMPLEMENTED
PREWIND_GATE_TO_F0_PATH=NOT_QUALIFIED
F0_BEFORE_PREWIND_PASS_COUNT=NOT_MEASURED
FAIL_ALLOWED_F0_COUNT=NOT_MEASURED
UNKNOWN_ALLOWED_F0_COUNT=NOT_MEASURED
INCOMPLETE_PREFIX_ALLOWED_F0_COUNT=NOT_MEASURED
DYNAMIC_PREFIX_SHORTEN_COUNT=NOT_MEASURED
FAVORABLE_STATE_RETRY_COUNT=NOT_MEASURED
WINDOW_RESTART_COUNT=NOT_MEASURED
F0_SCHEDULE_SHIFT_COUNT=NOT_MEASURED
RUNTIME_ATTESTATION_PATH=NOT_REQUALIFIED_IN_THIS_TASK
TRACE_FINALIZATION_PATH=NOT_EXERCISED_IN_THIS_TASK
C1_ACCOUNTING_PATH=NOT_EXERCISED_IN_THIS_TASK
E8_FINALIZATION_PATH=NOT_EXERCISED_IN_THIS_TASK
POSTPROCESS_STRICT_JSON_PATH=NOT_EXERCISED_IN_THIS_TASK
RESULT_SERIALIZATION_PATH=NOT_EXERCISED_IN_THIS_TASK
CLEANUP_PATH=NOT_EXERCISED_IN_THIS_TASK

## Owner boundary and recommendation

Do not implement a dummy gate that always fails, an uncalled seal helper, or
a sensor-frontier substitute and call the task qualified. Identify an existing
authoritative scheduled-native frontier binding, or explicitly authorize a
prospective source-time scheduling binding and assess its comparability with
the existing host-triggered acquisition path. The latter is outside the
currently authorized no-scheduling-change scope. No runtime is proposed here.

No production edits were applied. Luna was delegated the bounded source audit
but had not returned a deliverable; the lead executed the checks above.

SCIENTIFIC_FREEZE_CHANGED=false
MEASUREMENT_IMPLEMENTATION_BINDING_CHANGED=false
EXECUTION_LINEAGE_CHANGED=false
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
V3_RUNTIME_CONTROL_SEMANTIC_DELTA=NONE
PHASE_D_METRIC_SEMANTIC_DELTA=NONE
DISTURBANCE_SEMANTIC_DELTA=NONE
PREWIND_SEMANTIC_DELTA=NONE_IMPLEMENTATION_NOT_COMPLETED
INFRASTRUCTURE_LAUNCH_ATTEMPTS=7
SCIENTIFIC_ACQUISITION_ATTEMPTS_F0_REACHED=6
RECORDED_USABLE_SCIENTIFIC_CONDITIONS=5_UNCHANGED
READY_FOR_EXECUTION_BINDING_REVIEW=false
