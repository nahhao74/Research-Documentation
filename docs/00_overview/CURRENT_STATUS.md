# Current Status — 2026-09-04

## Executive state

The main AURA–WISE–WM–AEGIS pipeline is active again. PID-benchmark work is paused and is not an authority source for the main WM scientific contract.

Option-B reference-stability scheduling is now closed and live-qualified in bounded non-scientific execution. The current blocker is no longer Option-B. It is infrastructure reliability of the fresh randomized `G_action` pilot: no immutable root has completed the frozen 96-block matrix, so no scientific `G_action` estimate or causal-dataset acceptance exists yet.

```text
MAIN_PIPELINE=ACTIVE
PID_BENCHMARK_TRACK=PAUSED_OUT_OF_SCOPE

PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID
PHASE_0B5_QUALIFICATION=VALID_BOUNDED_NOSCIENCE

OPTION_B_CONTRACT_READY=true
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
OPTION_B_LIVE_RUNTIME_QUALIFIED=true
OPTION_B_DOWNSTREAM_TRANSACTION_INTEGRATION=QUALIFIED_NOSCIENCE

RESET_AUTHORITY=AURA_C1_SOURCE_RESET
RESET_PROPAGATION_REPAIR=QUALIFIED

M_STABLE_US=100000
W_MAX_US=1000000
EVENT_SCHEDULING_POLICY=DELAY_RESCHEDULE_WITHIN_BLOCK
FAIL_CLOSED_TIMEOUT=true
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
W_MAX_RUNTIME_FEASIBILITY=SUPPORTED_IN_BOUNDED_NOSCIENCE_QUALIFICATION

FRESH_RANDOMIZED_G_ACTION_PILOT=INCOMPLETE_INVALID_INFRASTRUCTURE_ROOTS
G_ACTION_PILOT_RESULT=NOT_EVALUATED
SCIENTIFIC_ANALYSIS=NOT_AUTHORIZED_FROM_PARTIAL_ROOT
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
FRESH_SCIENCE=BLOCKED

SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
AEGIS_AUTHORITY=0
ROUND_Z_AUTHORITY=0
production_authority=false
```

## Frozen scientific target

The estimand remains unchanged:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The following remain frozen by the current contract:

```text
AURA semantics
FAST/T1/C1 semantics
Direct Guard
M_STABLE_US=100000
W_MAX_US=1000000
PX4_BOOT_US timing authority
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
T_D / T_A
H1000 semantics
P1/P2 treatment profiles
ZERO semantics
randomization
CALM/GUST context semantics
SEALED boundary
production authority
```

## Option-B closure

The qualified bounded non-scientific 0B.5 evidence established:

```text
ZERO_WAIT_PARITY=PASS_3_OF_3
POSITIVE_WAIT_DELAY_PATH=PASS
AURA_ONSET_BINDING_AFTER_DELAY=PASS
EXACTLY_ONCE_NATIVE_COMMIT=PASS
OPTION_B_ROOT_CAUSE_RESOLUTION=A_OPTION_B_LIVE_DELAY_PATH_SUPPORTED
```

The reset-lineage authority is frozen as:

```text
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
```

The downstream mechanism qualification also passed with one native onset, one exact AURA binding, five accepted P1 +N mechanism cycles, explicit zero release and candidate-only H1000 wait. This remains mechanism evidence only, not a treatment-effect result.

## Current randomized-pilot blocker

The owner-authorized fresh randomized pilot was attempted, but no root completed the required 96-block matrix. Partial roots were not pooled or analyzed as science.

Latest retained failure classes:

```text
fresh_25 = invalid: C1 mutation / continuous-trace completeness
fresh_26 = invalid infrastructure: H1000 observer-retention timeout
fresh_27 = invalid infrastructure: next_status_timeout; C1 child exited 1
```

The 25 failed/incomplete roots selected for cleanup were deleted operationally; valid Q1/Q1B/Q1C/Q1D and 0B.5 qualification roots were retained. No complete valid scientific root remains.

Therefore:

```text
NEXT_STATE=OWNER_FRESH_PILOT_RETRY_AFTER_INFRASTRUCTURE_REVIEW
```

Any future pilot retry must:

```text
use a new immutable Kingston root
re-run the complete frozen preflight
run the complete frozen 96-block matrix from block 1
stop on the first invalid scientific block
never retry/replace/resample a failed block inside the same root
never pool incomplete roots
never analyze a partial root as science
```

## Newly selected offline validity acceleration

The owner selected two algorithms for immediate integration into WM scientific-validation infrastructure:

```text
REVERSE_PROCESSING=SELECTED
PEELING_FIXED_POINT_VALIDATION=SELECTED
RUNTIME_HOT_PATH_CHANGE=false
SCIENTIFIC_CONTRACT_CHANGE=false
IMPLEMENTATION_STATUS=PENDING
```

They are to be used only for offline/preflight/post-run validation and forensic acceleration.

### Reverse processing

For every source-time row, build nearest-future invalidation frontiers such as:

```text
next_source_gap
next_source_reorder
next_reset
next_session_change
next_reference_generation_change
next_AURA_invalid
next_C1_invalid
next_nonzero_action
next_disturbance_change
next_block_end
```

This permits O(N)-style timeline indexing where possible and avoids repeated forward searches from each candidate/action anchor.

### Peeling

Construct a causal dependency DAG and propagate direct invalid seeds until fixed point:

```text
raw trace
→ reverse validity index
→ causal dependency DAG
→ direct invalid seeds
→ iterative remove/update/repeat
→ VALID_CAUSAL_CORE
```

Typical dependency chain:

```text
source
→ AURA
→ C1
→ reference-stability eligibility
→ native event / AURA binding
→ T_D
→ candidate offer
→ ACK
→ accepted action
→ release / H1000
→ response horizons
→ ZERO/treatment pair
→ G_action dataset row
```

Every removed node must retain an explainable invalidity lineage. Tri-state behavior remains fail-closed:

```text
FAIL > UNKNOWN > PASS
```

This layer must not use outcome information to make treatment eligibility true and must not redefine any frozen scientific rule.

Detailed note:

[`../04_research/WM_REVERSE_PROCESSING_PEELING_VALIDITY_ENGINE_20260904.md`](../04_research/WM_REVERSE_PROCESSING_PEELING_VALIDITY_ENGINE_20260904.md)

## Immediate execution ladder

```text
1. Inspect current validators / dataset builders / retained failure evidence
2. Implement reverse timeline index offline
3. Implement causal dependency DAG + peeling fixed point
4. Prove mapping of every invalidation rule to an existing frozen contract clause
5. Run deterministic equivalence/idempotence/monotonicity tests
6. Use the engine to identify earliest divergence in fresh_25 / fresh_26 / fresh_27 where evidence remains
7. Repair implementation/infrastructure only
8. Run bounded non-scientific pre-retry qualification if deterministic proof is insufficient
9. Require PRE_RETRY_VALID_CAUSAL_CORE=true
10. Launch one new immutable full randomized G_action pilot
11. Accept only a complete 96/96 valid root
12. Only then perform scientific G_action analysis and causal-dataset acceptance
13. Only after causal-dataset acceptance proceed to World-Model training / WISE refinement
```

Execution-only pointer:

[`CURRENT_EXECUTION_LADDER_WM_20260904.md`](CURRENT_EXECUTION_LADDER_WM_20260904.md)

## World-Model / WISE dependency

Action-conditioned World-Model training remains blocked until a valid randomized causal dataset exists.

```text
RAW / RANDOMIZED TRACE
→ VALID_CAUSAL_CORE
→ CAUSAL DATASET ACCEPTANCE
→ G_action identification
→ World Model
→ WISE predictive refinement
→ AEGIS integration
```

Do not jump directly to WM training from partial or infrastructure-invalid roots.

## Storage and authority boundaries

Large runtime/capture/dataset/training/intermediate artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

Source, tests, small deterministic fixtures, configuration and canonical documentation remain under the project/source workspace.

Failed scientific roots remain immutable unless the owner explicitly authorizes operational cleanup. Cleanup does not upgrade or alter scientific conclusions.

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
MODEL_TRAINING_ACTION_RESPONSE=BLOCKED_PENDING_CAUSAL_DATA_ACCEPTANCE
production_authority=false
```
