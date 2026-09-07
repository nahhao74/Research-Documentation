# D0 V3 Task Report

## Identity

TASK_ID=D0_V3_CALM_READINESS_20260906_04
RESULT=UNKNOWN_MISSING_EVIDENCE
ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260906_04
SCHEMA_IDS=PHASE_D0_PHASE_D_READINESS_V3;V3_RUNTIME_START_EVENT_V1;V3_AURA_OWNER_OPERAND_LEDGER_V1;V3_DOWNSTREAM_EVALUATION_CORRESPONDENCE_V1
GIT_HEADS=UNAVAILABLE (provenance frozen before launch)
WORKTREE_FINGERPRINTS=provenance.json sha256:877a08c1bfd6a5dc137be0b45ace8cbf05341ce092e6b01db7dc39e53af168f1

## Lifecycle

PRELAUNCH=PASS_AFTER_CANONICAL_NONSEMANTIC_PACKAGE_BUILD_INSTALL_REFRESH
TRACE_STARTUP=PASS; TRACE_OWNER=trace_writer; first cursor bind occurred after trace source creation
START_EVENT=V3_RUNTIME_START_EVENT_V1; source_us=8176000; native_source_generation=1582; reset_generation=0; runtime_gate=OPEN
WINDOW_START=8176000 px4 source us
WINDOW_END=28176000 px4 source us
REFERENCE_COLLECTOR_FINAL_STATE=OWNED_PROCESS_DEAD_AND_REAPED; reference_collector_finalized=true
CLEANUP=CONTROLLED; all tracked managed process records report alive=false

## Pre-root self-repairs

DEFECT=CANONICAL_PRODUCTION_ROS_PACKAGE_ENTRYPOINT_UNAVAILABLE
CAUSE=stale/missing generated `aura_data_acquisition` install entrypoint state
REPAIR=canonical `colcon build --symlink-install --packages-select aura_data_acquisition`, followed by canonical install-environment sourcing
FILES_CHANGED=NO_SOURCE_OR_CONTROL_FILES
COMMANDS_OR_BUILD_ACTIONS=colcon package build/install refresh only
VERIFICATION=ros2 package prefix/executable resolution, `aura_phase_d0_v3 --help`, production construction and rclpy runtime import passed before root creation
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
V3_CONTRACT_SEMANTIC_DELTA=NONE

## Evaluation accounting

TOTAL_REQUIRED=4001
ACCOUNTED=4001
VALID_CONTROL=2879
EXPLAINED_CONTROL_UNAVAILABLE=1094 (ATTITUDE_FRESHNESS)
UNEXPLAINED_CONTROL_UNAVAILABLE=28

## M3

M3_SOURCE_IDENTITY_INVALID=0
M3_RESET_GENERATION_CHANGED=0
M3_CALIBRATION_CHANGED=0
M3_CAUSAL_ATTITUDE_UNAVAILABLE=1096
M3_CAUSAL_EXPECTED_UNAVAILABLE=1

## Runtime gate

RUNTIME_GATE_ACCELERATION_UNAVAILABLE=0
RUNTIME_GATE_NOT_ARMED=0
RUNTIME_GATE_COMMAND_STALE=0
All 4001 recorded runtime-gate snapshots were true.

## Lookup

M3_ATTITUDE_LOOKUP_NO_ADMISSIBLE_NONFUTURE_RECORD=1094
M3_ATTITUDE_LOOKUP_NO_CANDIDATE=2
M3_ATTITUDE_LOOKUP_SELECTED_ADMISSIBLE=2905
OWNER_LOOKUP_VELOCITY_SELECTED_ADMISSIBLE=4001
OWNER_LOOKUP_VELOCITY_ABSENT_OR_STALE=0
DEPLOYABLE_SHADOW_MOTOR_WARMUP=18
DEPLOYABLE_SHADOW_MOTOR_COMMAND_STALE_OR_MISSING=5

## W20

EXACT_CORRESPONDENCE=4001/4001 diagnostic evaluation identities
W20_VALID=2879
W20_FAIL_CLOSED_ZERO=1120
W20_INVALID_NONZERO_FRONTIER=2
ZERO_FRONTIER_POLICY=PRESERVED; no forward fill or synthetic frontier observed.

## C1

EXACT_CORRESPONDENCE=4001/4001 diagnostic evaluation identities
C1_ZERO_PROPAGATION=1120 zero-frontier inputs published as zero; retained internal bridge did not create a causal source frontier.

## E8

E8_SIDECAR=canonical_data0_row/e8_ingress_evidence.jsonl
E8_TOTAL_RECORDS=4576 (startup=1; pair-evaluation=4574; shutdown=1)
E8_IN_WINDOW_RECORDS=4001 exact diagnostic identities
E8_EXACT_DIAGNOSTIC_ID_MATCHES=4001
E8_EXACT_C1_ID_MATCHES=4001
E8_PAIR_SELECTED=2881
E8_NO_PAIR=1120
E8_ZERO_INPUT=1120
E8_EFFECTIVE_SOURCE_FALSE=4001 (no-offer CALM semantics)
E8_DUPLICATES=0
E8_MALFORMED=0
E8_UNMATCHED_REQUIRED_EVALUATIONS=0
E8_FINALIZATION=PASS; shutdown record present; sha256:65eba95b403286280aefe7a1e28655ff8378c090bd9efa383400713eece42009

## Integrity

DUPLICATES=0
OMISSIONS=0
MALFORMED=0
DROPS=0
GAPS=0
WRITER_ERRORS=0
TRACE_FINALIZATION=PASS; trace.jsonl sha256:d12723503c73e2589fa80761ec7368758aa259da6e4ecdef359428036e6476d6

## Findings

FIRST_FAILED_PROOF_OBLIGATION=v3_evaluator_not_accountable
FIRST_CAUSAL_DIVERGENCE=aura-shadow:1 at source_us=8176000; M3 first false=CAUSAL_ATTITUDE_AVAILABLE; M3 lookup result=NO_CANDIDATE; downstream zero chain was persisted.
ROOT_CAUSE=V3WindowEvaluator has a frozen explained-gap rule for ATTITUDE_FRESHNESS but does not classify all owner-proven causes present in this root. The remaining 28 UNKNOWN evaluations comprise 3 M3 candidate-invalid cases, 23 deployable physical-shadow cases (18 motor_warmup and 5 motor_command_stale_or_missing), and 2 W20-invalid/nonzero-frontier cases. This report does not infer their eligibility for explanation.
DEFECT_OWNER=V3 measurement/evaluator explained-cause coverage; not AURA, W20, C1, E8, PX4, or FAST control semantics.
CONTRADICTORY_EVIDENCE=0
FAIL_CLASS=UNKNOWN_MISSING_EVIDENCE
FAILURE_EXPECTED_BY_CONTRACT=YES; V3 requires zero UNEXPLAINED_CONTROL_UNAVAILABLE evaluations.

## Semantic deltas

CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
V3_CONTRACT_SEMANTIC_DELTA=NONE
V3_WINDOW_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
M3_SEMANTIC_DELTA=NONE
RUNTIME_GATE_SEMANTIC_DELTA=NONE
LOOKUP_SEMANTIC_DELTA=NONE
W20_SEMANTIC_DELTA=NONE
C1_SEMANTIC_DELTA=NONE
E8_SEMANTIC_DELTA=NONE

## Qualification

D0_INFRASTRUCTURE_CLOSED=false
READY_FOR_PHASE_D=false

## Immutable artifacts

- d0_v3_readiness_report.json — sha256:cce96727109883c457c85e6ed38402fec0f50ab5f0ef71d5493055e55eef3085
- provenance.json — sha256:877a08c1bfd6a5dc137be0b45ace8cbf05341ce092e6b01db7dc39e53af168f1
- canonical_data0_row/trace/trace.jsonl — sha256:d12723503c73e2589fa80761ec7368758aa259da6e4ecdef359428036e6476d6
- canonical_data0_row/e8_ingress_evidence.jsonl — sha256:65eba95b403286280aefe7a1e28655ff8378c090bd9efa383400713eece42009
- canonical_data0_row/parent_lifecycle.jsonl
- canonical_data0_row/v3_data0_backend_result.json

## Next task

NEXT_TASK=OWNER_REVIEW_OF_V3_EXPLAINED_CAUSE_COVERAGE_AND_GLOBAL_EVIDENCE_CONTRACT_COMPLETENESS; no new runtime root.
OWNER_AUTHORIZATION_REQUIRED=true
