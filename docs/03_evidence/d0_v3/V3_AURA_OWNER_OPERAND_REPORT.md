# V3 AURA Owner Operand Report

TASK_ID=V3_AURA_OWNER_OPERAND_INSTRUMENTATION_20260906
RESULT=PASS_OFFLINE_DIAGNOSTIC_ONLY
LEDGER_SCHEMA_ID=V3_AURA_OWNER_OPERAND_LEDGER_V1

## Decision ownership

M3_DECISION_SITE=`aura.vnext_disturbance.AuraVNextDisturbanceAssembler.update_imu`

M3_PREDICATE_ORDER=`SOURCE_IDENTITY_VALID|RESET_GENERATION_UNCHANGED|CALIBRATION_UNCHANGED|CAUSAL_ATTITUDE_AVAILABLE|CAUSAL_EXPECTED_AVAILABLE`

M3_LEDGER_FIELDS=per-index name/evaluated/operand_available/operand_value/result; exact candidate result, first-false predicate/index and result reason.

M3_FIRST_FALSE_POLICY=recorded at the executing M3 path; unevaluated later predicates are `NOT_EVALUATED`.

RUNTIME_GATE_DECISION_SITE=`aura.moving_runtime_node.AuraMovingRuntimeNode._process_imu`

RUNTIME_GATE_OPERANDS=`ACCELERATION_AVAILABLE|ARMED|COMMAND_FRESH`

RUNTIME_GATE_LEDGER_FIELDS=per-index actual operand/result plus exact gate result, first-false predicate/index, reset reason and evaluation monotonic time.

ATTITUDE_LOOKUP_SITE=`AuraVNextDisturbanceAssembler._causal` and the existing deployable-shadow selected-attitude cache.

ATTITUDE_LOOKUP_PROVENANCE=actual selected identity/source/receipt/reset, nonfuture relation, age, freshness limit, admissibility and owner result reason.

VELOCITY_LOOKUP_SITE=`AuraVNextDisturbanceAssembler._causal` and the existing deployable-shadow selected-velocity cache.

VELOCITY_LOOKUP_PROVENANCE=equivalent owner-time provenance, independently recorded.

EXISTING_SHADOW_OUTPUT_FIELDS_REUSED=`deployable_evaluation_id`, selected source times, ages, continuity, clock validity, native generation and reset identity.

DUPLICATE_FIELDS_AVOIDED=true

DIAGNOSTIC_EVALUATION_ID=existing `aura-shadow:<sequence>` only.

EVALUATION_ID_JOIN_MODEL=M3 and runtime-gate snapshots receive the ID before `update_imu`; lookup and existing shadow records merge into the same AURA disturbance diagnostic record.

## Semantic and hot-path separation

EVALUATOR_INTEGRATION=`V3WindowEvaluator` only consumes complete new owner evidence; incomplete historical/new evidence remains `UNEXPLAINED_CONTROL_UNAVAILABLE`.

MISSING_EVIDENCE_POLICY=UNKNOWN_FAIL_CLOSED; `_02` is unchanged and remains `UNKNOWN_MISSING_EVIDENCE`.

HOT_PATH_IO=NONE_NEW. The patch only builds scalar fields for the existing AURA diagnostic publication; it adds no file operation, wait, acknowledgement or downstream dependency.

QUEUEING_MODEL=existing AURA diagnostic publisher/writer path, unchanged.

OVERFLOW_POLICY=no instrumentation-specific queue exists. Existing writer errors/drops/gaps and owner-sequence omissions remain V3 fail-closed evidence; control continues unchanged.

INSTRUMENTATION_OVERHEAD=bounded scalar/dictionary construction at existing decision and publication sites; offline control-inert regression retained identical `DisturbanceState` output with and without a diagnostic evaluation ID.

## Qualification

OFFLINE_TEST_RESULT=PASS_69 (`test_vnext_disturbance`, `test_phase_d0_v3_measurement`, `test_phase_d0_v3_orchestrator`)

CONTROL_INERT_REGRESSION=PASS

FALSE_EXPLAINED_COUNT=0

FALSE_PASS_COUNT=0

CONTRADICTION_TEST_RESULT=PASS_FAIL_CLOSED_BY_EXISTING_EVALUATOR_POLICY

The ROS-node integration suite could not be collected in this offline Python
environment because `rclpy` is absent; this is not a runtime result. Python
compilation of the modified ROS node passed.

## Boundaries and next gate

CONTROL_SEMANTIC_DELTA=NONE

SCIENTIFIC_SEMANTIC_DELTA=NONE

V3_CONTRACT_SEMANTIC_DELTA=NONE

V3_WINDOW_SEMANTIC_DELTA=NONE

FAST_SEMANTIC_DELTA=NONE

M3_SEMANTIC_DELTA=NONE

RUNTIME_GATE_SEMANTIC_DELTA=NONE

LOOKUP_SEMANTIC_DELTA=NONE

MISSING_REQUIRED_EVIDENCE=DOWNSTREAM_PER_EVALUATION_W20_C1_E8_CORRESPONDENCE_NOT_EXPANDED_IN_THIS_BOUNDED_AURA_TASK; audit before new-root authorization.

READY_FOR_ONE_NEW_FRESH_D0_V3_ROOT=false

READY_FOR_PHASE_D=false

NEXT_TASK=owner review of the new AURA ledger plus the separately frozen downstream correspondence requirement before a fresh D0 root.

OWNER_AUTHORIZATION_REQUIRED=true
