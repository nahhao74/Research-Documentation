# Current Execution Ladder — WM Main Pipeline — 2026-09-05

## Purpose

This file is the execution-only ladder for the AURA–WISE–WM–AEGIS main pipeline after the owner-authorized `fresh_35` randomized `G_action` root stopped fail-closed in its first CALM row.

`fresh_35` is immutable infrastructure evidence. No scientific block, candidate exposure or manifest slot was admitted. The next executable task is **not** another randomized pilot; it is a separate forensic/minimal-repair review of accepted-cycle status callback visibility and bounded retention.

This file does not replace the scientific contract.

## Current authority

```text
PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID

OPTION_B_LIVE_RUNTIME_QUALIFIED=true
OPTION_B_DOWNSTREAM_TRANSACTION_INTEGRATION=QUALIFIED_NOSCIENCE
RESET_AUTHORITY=AURA_C1_SOURCE_RESET

WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
POST_RESET_E8_SOURCE_CAUSAL_HANDOFF_QUALIFICATION=VALID_NONSCIENTIFIC
NATIVE_EVENT_CLEAR_LIFECYCLE_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC

LATEST_FAILED_SCIENTIFIC_ROOT=fresh_35
FRESH35_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN

G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

AUTHORIZE_NEW_SCIENTIFIC_ROOT=false
AUTHORIZE_TIMEOUT_INCREASE=false
AUTHORIZE_QOS_CHANGE=false
AUTHORIZE_WM_TRAINING=false
AUTHORIZE_FAST_BASELINE_CHANGE=false

NEXT_STATE=OWNER_ACCEPTED_CYCLE_STATUS_VISIBILITY_RETENTION_FORENSIC_REVIEW_REQUIRED
```

## Frozen scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

No step below may change the estimand, AURA/FAST/T1/C1 semantics, PX4 authority, treatment profiles, randomization, Direct Guard, `M_STABLE_US=100000`, `W_MAX_US=1000000`, `PX4_BOOT_US`, `AURA_C1_SOURCE_RESET`, `T_D/T_A`, H1000, SEALED or production authority.

Authoritative manifest:

```text
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
MANIFEST_SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

## Latest root — fresh_35

Immutable root:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm1_v2r1_within_run_randomized_action_20260905_fresh_35
```

Preflight passed with:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
E8 ledger=4096
Option-B=Direct Guard
native CLEAR lifecycle qualified
next_status successor qualification retained
scientific semantic delta=NONE
```

Runtime identity:

```text
Git HEAD=a6cebe4d8f1e99a941ab7886efdd7d6a446143c3
worktree entries=397
dirty fingerprint SHA256=c6d0bb85b2d07c8db90c564a183866d1915c10dc22a81ab8c2aeb4bacad9385c
```

Execution stopped at the first row:

```text
row=WM1V2R1_RAND_A_CALM_R1
sessions attempted/valid=1/0 of 8
blocks attempted/valid=1/0 of 96
requested source frontier=38576000 PX4_BOOT_US
previous accepted timestamp=38232000
generation/session/reset=900706/632000/0
terminal=RuntimeError:accepted_cycle_timeout
next_status lookups/timeouts=0/0
```

No native GUST, candidate, `T_D`, ACK, accepted exposure, H1000 completion, manifest slot or SEALED access occurred.

## Proven accepted-cycle boundary

A same-lineage valid native status exists at:

```text
source frontier=38916000 PX4_BOOT_US
generation/session/reset=900706/632000/0
source_valid=true
source_fresh=true
gate_valid=true
applied_authority=true
```

The status is `340000 us` after the requested frontier, inside the existing `500000 us` source-match budget. Therefore producer absence is not proven.

Timeout diagnostics:

```text
thread_alive=true
error=null
status_count=2000
lineage_count=2
matching_statuses=[]
```

Current implementation facts:

```text
accepted-cycle matcher scans self.statuses[-2000:]
per-lineage latest slot exists
find_cycle does not use the per-lineage latest slot
```

Because callback receipt versus later retention loss was not independently persisted, do not claim ring eviction as proven.

## Step 1 — Forensic callback receipt vs retention

Instrument and reproduce only the accepted-cycle visibility path under a new non-scientific task/root.

For the exact contract identity persist:

```text
generation
controller session
reset generation
native/source frontier
callback receipt sequence
callback receipt time/source identity
ring insertion sequence
ring oldest/newest identity
per-lineage latest identity
matcher scan range
matcher selected/rejected candidates
first reason a contract-valid candidate becomes unavailable
```

The forensic must classify the earliest proven boundary as one of:

```text
A_CALLBACK_NOT_RECEIVED
B_CALLBACK_RECEIVED_THEN_NOT_RETAINED
C_CALLBACK_RETAINED_BUT_MATCHER_SELECTION_FAILED
D_OTHER_PROVEN_IMPLEMENTATION_BOUNDARY
```

If evidence is insufficient, report `UNRESOLVED`; do not infer eviction from capacity alone.

## Step 2 — Source/semantic audit

Before any repair, prove that the proposed change preserves the existing accepted-cycle contract:

```text
same generation/session/reset identity
same source-time matching budget
same accepted/applied status semantics
same fail-closed behavior
same candidate/exposure semantics
same scientific estimand
```

Required classification:

```text
SCIENTIFIC_SEMANTIC_DELTA=NONE
```

Otherwise stop for owner review.

## Step 3 — Minimal canonical repair

Only after Step 1 identifies a concrete implementation defect, make the smallest reusable repair at the canonical owner.

Preferred principle:

```text
preserve contract-valid status identity long enough for the canonical accepted-cycle matcher
```

Do not solve the problem by:

```text
increasing timeout
increasing QoS merely to obtain PASS
loosening generation/session/reset matching
using host time in place of PX4 source time
accepting an older favorable cross-lineage status
patching fresh_35
```

If a canonical per-lineage index/slot can satisfy the existing matcher semantics without changing the contract, compare it against simple ring enlargement and prefer the smallest bounded deterministic mechanism that directly closes the proven failure.

## Step 4 — Deterministic regression

Cover at least:

```text
exact generation/session/reset match
wrong generation reject
wrong session reject
wrong reset reject
source frontier equal/behind reject
valid in-budget future status accept
out-of-budget status reject
candidate remains selectable under unrelated status volume
no stale cross-lineage fallback
ring/index consistency
reset/session transition cleanup
```

Existing `next_status`, E8 source-causal pairing, Direct Guard, native-CLEAR, C1 replay, H1000 and WM validity-engine tests must remain unchanged.

## Step 5 — Bounded non-scientific qualification

Run a dedicated qualification that exercises the repaired accepted-cycle visibility path without scientific treatment credit.

Require:

```text
accepted-cycle lookups > 0
accepted-cycle timeouts = 0 for contract-valid in-budget successors
callback receipt/retention provenance persisted
wrong-lineage false accepts = 0
source/session/reset integrity = PASS
C1 missing lifecycle = 0
writer errors/drops/gaps = 0/0/0
graph_valid=true
forbidden_cycles=0
PRE_RETRY_VALID_CAUSAL_CORE=true
SCIENTIFIC_BLOCKS=0
MANIFEST_SLOTS_CONSUMED=0
SEALED_ACCESS=0
```

A PASS returns to owner review. It does not authorize a randomized pilot automatically.

## Step 6 — Owner review

Only after the bounded qualification passes may the owner separately decide whether to authorize another fresh scientific root.

Until then:

```text
FRESH_SCIENCE=BLOCKED
```

## Step 7 — Future scientific retry, only if separately authorized

A later authorized root must still use:

```text
new immutable root
same frozen manifest
full preflight
8 sessions / 96 blocks
start from block 1
stop on first invalid block
no retry/skip/replace/resample/hot-fix/pooling
```

No incomplete root may produce treatment-effect inference.

## Canonical reverse processing / Tarjan / peeling

Every qualification or scientific root that can establish readiness/validity must reuse the canonical WM causal-validity pipeline:

```text
reverse validity indexing
→ canonical source-grounded dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
```

For `fresh_35`:

```text
graph=21 nodes / 34 edges
SCC=21 singleton components
forbidden cycles=0
graph_valid=true
reverse compact records=621
peeling iterations=10
VALID_CAUSAL_CORE=false
```

The failed root remains immutable infrastructure evidence.

## FAST research boundary

The current Phase-0 FAST/T1/C1 semantics remain frozen. Separate shadow/replay research may benchmark alternative FAST algorithms on the current simulator, but no challenger is currently selected or authorized for the scientific baseline.

The earlier residual-PI proposal is not current main-pipeline direction.

If a future FAST algorithm is promoted, baseline `B` changes and the action-conditioned scientific/model contract must be versioned accordingly.

## World Model boundary

Current canonical structure remains:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

WM training is blocked until a complete valid randomized root passes a separate causal-dataset admission audit and the owner separately authorizes training.

## Hard boundaries

```text
failed root = immutable historical evidence
partial valid rows ≠ scientific dataset
infrastructure failure ≠ scientific negative result
FAST research ≠ current baseline change
WM design work ≠ training authority
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

## Current final state

```text
OWNER_ACCEPTED_CYCLE_STATUS_VISIBILITY_RETENTION_FORENSIC_REVIEW_REQUIRED
```
