# D0 V3 `_02` Attribution Report

TASK_ID=D0_V3_02_UNEXPLAINED_OWNER_OPERAND_AUDIT_20260906
RESULT=PASS_OFFLINE_DIAGNOSIS
SOURCE_ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260906_02

TOTAL_EVALUATIONS=4001
VALID_CONTROL=2833
UNEXPLAINED_INPUT_COUNT=1168

M3_CANDIDATE_INVALID_COUNT=1149
DEPLOYABLE_PHYSICAL_SHADOW_INVALID_COUNT=17
NONE_WITH_UNEXPLAINED_COMPOSITION_COUNT=2

EXPLAINABLE_FROM_EXISTING_ROOT=0
NOT_EXPLAINABLE_DUE_TO_MISSING_OWNER_EVIDENCE=1168
CONTRADICTORY_EVIDENCE=0

ATTITUDE_STALE_RAW_COUNT=1148
ATTITUDE_STALE_FULLY_ATTRIBUTABLE_COUNT=0
ATTITUDE_STALE_MISSING_REQUIRED_EVIDENCE_COUNT=1148

EVALUATOR_JOIN_DEFECT_COUNT=0
OWNER_EVIDENCE_MISSING_COUNT=1168

FIRST_MISSING_EVIDENCE_BOUNDARY=M3_candidate_valid_internal_first_false_predicate_and_runtime_gate_operands_not_persisted_on_the_exact_AURA_evaluation
MINIMAL_MISSING_EVIDENCE_SET=ordered_M3_candidate_predicate_operands_with_first_false;runtime_gate_operands_and_composed_result;exact_owner_lookup_availability_provenance;W20_C1_E8_diagnostic_correspondence_for_each_evaluation_id

EVIDENCE_SUFFICIENCY_MATRIX=
| Predicate | Owner | Present in `_02` | Exact join | Missing |
|---|---|---:|---:|---|
| evaluation identity/source/reset/native generation | AURA | yes | yes, 1168/1168 | none |
| candidate validity outcome | AURA | yes | yes | internal M3 first-false operands |
| attitude/velocity presence and ages | AURA | yes | yes | owner-time lookup selection/provenance |
| source and clock validity | AURA | yes | yes | none for recorded booleans |
| runtime gate | AURA | no | no | armed/reference/acceleration/gate operands |
| deployable shadow outcome | AURA | yes | yes | composition inputs beyond recorded fields |
| W20/C1/E8 downstream correspondence | downstream owners | incomplete | no exact per-evaluation proof | diagnostic evaluation identity linkage |

PROPOSED_DIAGNOSTIC_ONLY_FIELDS=M3 predicate operand ledger in frozen evaluation order; runtime armed/reference freshness/acceleration/gate values; owner-time lookup record identity/nonfuture relation/receipt provenance; W20/C1/E8 evaluation-id correspondence and zero propagation result

CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
V3_CONTRACT_SEMANTIC_DELTA=NONE
V3_WINDOW_SEMANTIC_DELTA=NONE

READY_FOR_MEASUREMENT_ONLY_REPAIR=true
READY_FOR_NEW_D0_ROOT=false
READY_FOR_PHASE_D=false

NEXT_TASK=OWNER_AUTHORIZED_DIAGNOSTIC_ONLY_OWNER_OPERAND_LEDGER_COMPLETION_AND_OFFLINE_QUALIFICATION
OWNER_AUTHORIZATION_REQUIRED=true
