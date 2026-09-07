# V3 Explained-Cause Coverage Report

TASK_ID=V3_EXPLAINED_CAUSE_COVERAGE_GLOBAL_AUDIT_20260906
RESULT=OWNER_DECISION_REQUIRED_NO_NEW_ROOT
SOURCE_ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260906_04

## Immutable-root audit

ORIGINAL_VALID=2879
ORIGINAL_EXPLAINED=1094_ATTITUDE_FRESHNESS
ORIGINAL_UNEXPLAINED=28

The source root was read only. Its official result remains `UNKNOWN_MISSING_EVIDENCE`.

| Evaluation IDs | Exact owner state | Final audit classification | Evidence finding |
| --- | --- | --- | --- |
| `aura-shadow:1,2` | M3 `CAUSAL_ATTITUDE_AVAILABLE=false`; lookup `NO_CANDIDATE` | `EXPECTED_CONTROL_UNAVAILABLE_BUT_CONTRACT_MAPPING_NOT_FROZEN` | Exact M3 order, no candidate, valid gate, W20 zero, C1 zero and E8 correspondence are present. `NO_CANDIDATE` is not `ATTITUDE_FRESHNESS`. |
| `aura-shadow:995` | M3 `CAUSAL_EXPECTED_AVAILABLE=false` | `UNKNOWN_REQUIRES_MORE_EVIDENCE` | The first-false predicate is exact, but no expected-setpoint lookup provenance was persisted. |
| `aura-shadow:3,4,5,7,8,9,11,12,13,15,17,18,19,20,21,22,24,25` | deployable `motor_warmup` | `EXPECTED_CONTROL_UNAVAILABLE_BUT_CONTRACT_MAPPING_NOT_FROZEN` | One bootstrap interval, +12 to +120 ms; V3 start eligibility has no motor-readiness operand. |
| `aura-shadow:559,1159,2727,2731,2751` | `motor_command_stale_or_missing`, command time 0 | `NOT_READY_CONTROL_UNAVAILABLE` | Recurrent at +2.792, +5.792, +13.632, +13.652 and +13.752 s; no causal `ActuatorMotors.timestamp_sample` command. |
| `aura-shadow:27,996` | W20 invalid/not-ready, nonzero current source frontier | `EXPECTED_CONTROL_UNAVAILABLE_BUT_CONTRACT_MAPPING_NOT_FROZEN` | No prior causal baseline; current timestamp is provenance only, `d_fast=NaN`, C1 is source-invalid and E8 is not effective-valid. |

M3_NO_CANDIDATE_COUNT=2
M3_NO_CANDIDATE_DISPOSITION=EXPECTED_CONTROL_UNAVAILABLE_BUT_CONTRACT_MAPPING_NOT_FROZEN
M3_CAUSAL_EXPECTED_UNAVAILABLE_COUNT=1
M3_CAUSAL_EXPECTED_DISPOSITION=UNKNOWN_REQUIRES_MORE_EVIDENCE
MOTOR_WARMUP_COUNT=18
MOTOR_WARMUP_DISPOSITION=EXPECTED_CONTROL_UNAVAILABLE_BUT_CONTRACT_MAPPING_NOT_FROZEN
MOTOR_WARMUP_RELATIVE_TO_WINDOW=ONE_INITIAL_BOOTSTRAP_INTERVAL_+12000_TO_+120000_US
MOTOR_COMMAND_STALE_MISSING_COUNT=5
MOTOR_COMMAND_OWNER=AuraMovingRuntimeNode._motor_callback__ActuatorMotors.timestamp_sample__DeployableResidualShadow
MOTOR_COMMAND_DISPOSITION=NOT_READY_CONTROL_UNAVAILABLE
RUNTIME_GATE_COMMAND_OWNER=AuraMovingRuntimeNode._process_imu__TrajectorySetpoint_receipt_age_100MS
COMMAND_FRESHNESS_DOMAINS_DISTINCT=true

## W20 invalid/nonzero-frontier audit

W20_INVALID_NONZERO_FRONTIER_COUNT=2
W20_INVALID_NONZERO_FRONTIER_ANALYSIS=`aura-shadow:27` and `aura-shadow:996` had valid M3 and physical shadow, `w20_ready=false`, `w20_valid=false`, `w20_baseline_sample_count=0`, current nonzero provenance, and no finite `d_fast`. C1 published `source_valid=false`; E8 paired diagnostics but was `effective_source_valid=false`. No source frontier was fabricated or forward-filled.
W20_INVARIANT_RESULT=PASS_W20_CONTRACT_DEFINED_NOT_READY_WITH_CURRENT_NONZERO_PROVENANCE

## Global state-space coverage proposal

EXPLAINED_CAUSE_REGISTRY_ID=V3_EXPLAINED_CAUSE_REGISTRY_V1_PROPOSAL

| Owner | Runtime state/reason | First-false predicate | Control outcome | Evidence required | Current V3 classification | Mapping complete? |
| --- | --- | --- | --- | --- | --- | --- |
| M3 | source invalid | `SOURCE_IDENTITY_VALID` | fail closed | ordered M3, continuity/generation | readiness failure | yes |
| M3 | reset/calibration changed | reset/calibration predicate | fail closed/reset | ordered M3, lineage | readiness failure | yes |
| M3 | attitude stale | `CAUSAL_ATTITUDE_AVAILABLE`, age >10 ms | W20 zero | lookup + W20/C1/E8 | explained freshness | yes |
| M3 | attitude no-candidate/future-only | `CAUSAL_ATTITUDE_AVAILABLE` | W20 zero | lookup + downstream | contract decision required | no |
| M3 | expected unavailable | `CAUSAL_EXPECTED_AVAILABLE` | W20 zero | expected lookup + downstream | unknown | no |
| shadow context | attitude missing/stale | context operand | W20 zero | lookup + downstream | stale explained; missing needs decision | partial |
| shadow context | velocity missing/stale | context operand | W20 zero | lookup + downstream | contract decision required | no |
| physical shadow | motor warmup | `motor_warmup` | W20 zero | warmup epoch + downstream | contract decision required | no |
| physical shadow | command stale/missing | `motor_command_stale_or_missing` | W20 zero | command provenance + downstream | not-ready control unavailable | yes |
| physical shadow | rejected/source invalid | owner reason | W20 zero | exact reason + downstream | readiness failure | yes |
| W20 | raw input rejected | invalid/nonmonotonic/nonfinite | zero | W20 reason + downstream | readiness failure | yes |
| W20 | no causal baseline | `ready=false` | invalid, current provenance | ready/count/current source + downstream | contract decision required | no |
| C1 | source invalid zero propagation | upstream result | no usable C1 action | W20/C1 exact join | follows upstream | yes |
| C1 | fabricated/inconsistent frontier | invariant breach | invalid measurement | exact bridge/source evidence | readiness failure | yes |
| E8 | no pair/rejected downstream | selector result | no usable ingress | C1/E8 exact join | follows upstream | yes |
| E8 | contradictory identity/reset/control identity | invariant breach | invalid measurement | E8 pair evidence | readiness failure | yes |

KNOWN_REASON_COUNT=16
UNMAPPED_KNOWN_REASON_COUNT=0_IN_PROPOSAL_ONLY
CONTRACT_DECISION_REQUIRED_STATES=5

Every current source state has one proposed disposition, but no registry was frozen. The five contract-decision states cannot be implemented as explained outcomes without owner approval.

## Conditional evaluator-only repair

EVALUATOR_REPAIR_APPLIED=true
EVALUATOR_IMPLEMENTATION_DELTA=ONE_STRICT_DEPLOYABLE_SHADOW_ATTITUDE_FRESHNESS_BRANCH_ALREADY_REQUIRED_BY_FROZEN_V3_LEDGER_FIRST_FALSE_RULE
QUALIFICATION_CONTRACT_SEMANTIC_DELTA=NONE

The evaluator now recognizes the existing V3 `ATTITUDE_FRESHNESS` rule when valid M3 and the existing deployable ledger prove a selected nonfuture stale attitude, valid provenance, and existing W20-zero fail closure. Missing, future, no-candidate, velocity, motor and W20-not-ready cases remain fail closed. `_04` has no such deployable-attitude case.

DERIVED_VALID=2879
DERIVED_EXPLAINED=1094
DERIVED_UNEXPLAINED=28

## Validation and decision

OFFLINE_TEST_RESULT=PASS_29_FOCUSED_TESTS
FALSE_EXPLAINED_COUNT=0
FALSE_PASS_COUNT=0
MISSING_REQUIRED_DECISION=OWNER_DISPOSITION_FOR_CAUSAL_ATTITUDE_NO_CANDIDATE_MOTOR_WARMUP_W20_NOT_READY_AND_EXPECTED_SETPOINT_LOOKUP_PROVENANCE_PLUS_POLICY
CONTROL_SEMANTIC_DELTA=NONE
SCIENTIFIC_SEMANTIC_DELTA=NONE
FAST_SEMANTIC_DELTA=NONE
READY_FOR_ONE_NEW_FRESH_D0_V3_ROOT=false
READY_FOR_PHASE_D=false
NEXT_TASK=OWNER_REVIEW_OF_V3_EXPLAINED_CAUSE_REGISTRY_PROPOSAL_AND_MEASUREMENT_ONLY_EXPECTED_SETPOINT_LOOKUP_PROVENANCE_CLOSURE_IF_APPROVED
OWNER_AUTHORIZATION_REQUIRED=true
