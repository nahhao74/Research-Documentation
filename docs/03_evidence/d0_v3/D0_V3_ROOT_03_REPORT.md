# D0 V3 Task Report

## Identity

TASK_ID=D0_V3_CALM_READINESS_20260906_03
ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260906_03
RESULT=UNKNOWN_MISSING_EVIDENCE
GIT_HEADS=recorded in provenance.json; per-subtree Git metadata unavailable to the frozen orchestrator
WORKTREE_FINGERPRINTS=recorded in provenance.json
SCHEMA_IDS=PHASE_D0_PHASE_D_READINESS_V3;V3_AURA_OWNER_OPERAND_LEDGER_V1;V3_DOWNSTREAM_EVALUATION_CORRESPONDENCE_V1

## Lifecycle

PRELAUNCH=PASS: canonical runtime bootstrap, rclpy, production entrypoint, PX4 identity gate, assets, storage, and no conflicting runtime.
TRACE_STARTUP=PASS: canonical trace owner marker followed by trace creation within the frozen 5 s grace.
START_EVENT=AURA_MOVING_ACCEPTED_SOURCE_RUNTIME_GATE_OPEN at PX4 source 7672000 us; reset_generation=0; native_source_generation=1484.
WINDOW_START=7672000 px4_boot_us
WINDOW_END=27672000 px4_boot_us
CLEANUP=DATA0 cleanup reported success; its finalized snapshot still listed reference_collector PID 41903 alive. The task terminated exact orphan PIDs 41903/41922 after finalization; no managed runtime remains.

## Evaluation accounting

TOTAL_REQUIRED=4000
ACCOUNTED=4000
VALID_CONTROL=2885
EXPLAINED_CONTROL_UNAVAILABLE=0
UNEXPLAINED_CONTROL_UNAVAILABLE=1115

## M3 attribution

M3_SOURCE_IDENTITY_INVALID=0
M3_RESET_GENERATION_CHANGED=0
M3_CALIBRATION_CHANGED=0
M3_CAUSAL_ATTITUDE_UNAVAILABLE=1084
M3_CAUSAL_EXPECTED_UNAVAILABLE=3
M3_ALL_PREDICATES_PASS=2913
M3_LEDGER_RECORDS=4000

## Runtime-gate attribution

RUNTIME_GATE_ACCELERATION_UNAVAILABLE=0
RUNTIME_GATE_NOT_ARMED=0
RUNTIME_GATE_COMMAND_STALE=0
RUNTIME_GATE_ALL_OPERANDS_PASS=4000

## Lookup attribution

ATTITUDE_LOOKUP_ABSENT=1084 (NO_ADMISSIBLE_NONFUTURE_RECORD)
ATTITUDE_LOOKUP_FUTURE_ONLY=0
ATTITUDE_LOOKUP_STALE=0
ATTITUDE_LOOKUP_OTHER_REJECTION=0
VELOCITY_LOOKUP_ABSENT=0 proven standalone; 1087 values were not recorded after M3 short-circuit and remain NOT_EVALUATED rather than absent.
VELOCITY_LOOKUP_STALE=0
VELOCITY_LOOKUP_OTHER_REJECTION=0

## Downstream correspondence

W20=diagnostic schema fields present in AURA disturbance records.
C1=diagnostic schema fields present in the trace.
E8=V3 downstream records were written to canonical_data0_row/e8_ingress_evidence.jsonl, but no E8 diagnostic-evidence topic was subscribed/persisted in trace/trace.jsonl for V3WindowEvaluator consumption.
ZERO_FRONTIER=retained; no synthetic frontier was observed.

## Evidence integrity

DUPLICATES=0
OMISSIONS=0
MALFORMED=0
DROPS=0
GAPS=0
WRITER_ERRORS=0
RECORDER=44222 unique trace records; cursor partial bytes=0; skipped=0.
WRITER=finalized; rows_written=44222; stopped=true.

## Findings

FIRST_FAILED_PROOF_OBLIGATION=v3_evaluator_not_accountable
FIRST_CAUSAL_DIVERGENCE=E8_DIAGNOSTIC_CORRESPONDENCE_NOT_INGESTED_BY_CANONICAL_V3_TRACE
ROOT_CAUSE=The exact E8 correspondence artifact exists separately but is not part of the canonical trace subscription/evaluator input, yielding 1084 e8_correspondence_unproven evaluations. This root does not establish a control defect.
DEFECT_OWNER=V3 measurement/runtime trace-ingestion binding for E8 diagnostic evidence
CONTRADICTORY_EVIDENCE=0

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

## Immutable Artifacts

- provenance.json — sha256 80058de4a4eb00b2d8c2db161dbb13a0fdcbc1a46f6e9e2e0eecedf53284230d
- d0_v3_readiness_report.json — sha256 3a733b05287f26c14d6cbde3a85c9e9f2bd45cc842afdb966385cc0e0be9260d
- canonical_data0_row/trace/trace_summary.json — sha256 e43b1b1844b910f6b07cb4a30bb307434d7c130e1d70ac86188bb05e8f766bed
- canonical_data0_row/e8_ingress_evidence.jsonl — separate E8 correspondence artifact, not reclassified into trace evidence.

## Next task

NEXT_TASK=Bounded offline audit of the V3 canonical trace-ingestion contract for E8 diagnostic correspondence and finalized collector cleanup ownership; do not launch another root until the missing measurement boundary is diagnosed and offline-qualified.
OWNER_AUTHORIZATION_REQUIRED=true

## Pre-root self-repairs

DEFECT=CANONICAL_PRODUCTION_ROS_PACKAGE_ENTRYPOINT_UNAVAILABLE_IN_BASE_SOURCED_ENVIRONMENT
CAUSE=The console entrypoint requires the canonical project runtime bootstrap, including project-root Python paths.
REPAIR=Sourced scripts/runtime_env.sh; no source, package, or environment installation change was made.
FILES_CHANGED=NONE
COMMANDS_OR_BUILD_ACTIONS=canonical runtime bootstrap only
VERIFICATION=ros2 run aura_data_acquisition aura_phase_d0_v3 --help; Python rclpy/aura/aegis/aegis_c1_stability_mock imports PASS.
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
V3_CONTRACT_SEMANTIC_DELTA=NONE

DEFECT=STALE_HISTORICAL_COLLECTOR_PROCESS_CONFLICT
CAUSE=An orphan collector for immutable root _02 was still running before prelaunch.
REPAIR=Controlled termination of exact orphan PIDs 23122/23129; no historical evidence file was modified.
FILES_CHANGED=NONE
COMMANDS_OR_BUILD_ACTIONS=SIGTERM exact collector PIDs
VERIFICATION=prelaunch conflicting-runtime check PASS.
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
V3_CONTRACT_SEMANTIC_DELTA=NONE
