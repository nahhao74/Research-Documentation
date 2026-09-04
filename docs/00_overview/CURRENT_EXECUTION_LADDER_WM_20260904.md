# Current Execution Ladder — WM Main Pipeline — 2026-09-04

## Purpose

This file is the execution-only ladder for resuming the main AURA–WISE–WM–AEGIS pipeline after the PID-benchmark detour was paused.

It does not replace scientific contracts. It summarizes the next executable sequence from the current qualified Option-B state.

## Current authority

```text
PHASE_0B2=CLOSED
PHASE_0B3_IMPLEMENTATION=CLOSED
PHASE_0B4_DETERMINISTIC_REGRESSION=CLOSED
PHASE_0B5=CLOSED_VALID

OPTION_B_LIVE_RUNTIME_QUALIFIED=true
OPTION_B_DOWNSTREAM_TRANSACTION_INTEGRATION=QUALIFIED_NOSCIENCE
RESET_AUTHORITY=AURA_C1_SOURCE_RESET

FRESH_RANDOMIZED_G_ACTION_PILOT=INCOMPLETE_INVALID_INFRASTRUCTURE_ROOTS
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

## Scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

## Step 1 — Infrastructure forensic closure

Resolve the latest retained failure classes without changing scientific semantics:

```text
fresh_25:
C1 mutation / continuous-trace completeness

fresh_26:
H1000 observer-retention timeout

fresh_27:
next_status_timeout; C1 child exited 1
```

For each, identify:

```text
first failing source frontier
owning component
expected state
observed state
session/reset identity
last valid causal observation
root cause vs downstream timeout chain
```

Do not increase timeouts as a first repair.

## Step 2 — Reverse-processing validity index

Implement an offline reverse scan over accepted source-time records.

For each row derive nearest future boundaries required by existing contracts, including where applicable:

```text
next_source_gap
next_source_reorder
next_source_invalid
next_source_stale
next_reset
next_session_change
next_reference_generation_change
next_reference_transition
next_AURA_invalid
next_C1_invalid
next_candidate_offer
next_candidate_accept
next_nonzero_action
next_release
next_disturbance_change
next_H1000_invalid_frontier
next_block_end
next_session_end
```

Compute a deterministic `valid_until_source_us` and `first_future_invalidator` for each anchor.

This is indexing/acceleration only. It does not create new validity criteria.

## Step 3 — Causal dependency graph

Build an offline DAG using actual frozen dependencies only.

Candidate node types:

```text
SOURCE_SAMPLE
AURA_STATE
C1_STATE
REFERENCE_STABILITY_WINDOW
NATIVE_EVENT
AURA_ONSET_BINDING
T_D
CANDIDATE_OFFER
ACK
ACCEPTED_ACTION
T_A
RELEASE
H1000_ENDPOINT
RESPONSE_HORIZON
ZERO_PAIR
TREATMENT_PAIR
BLOCK
SESSION
```

Do not infer an edge merely from temporal proximity.

## Step 4 — Peeling fixed-point validation

Seed the graph with direct hard-invalid observations already defined by frozen contracts.

Then iterate:

```text
remove direct invalid nodes
→ update mandatory dependents
→ remove newly unsupported nodes
→ repeat until fixed point
```

Tri-state precedence remains:

```text
FAIL > UNKNOWN > PASS
```

Output:

```text
VALID_CAUSAL_CORE
```

Every removed node must retain an invalidity lineage:

```text
node
reason
direct_or_propagated
upstream_invalid_node
causal_path
source_time
session/reset
block
```

## Step 5 — Deterministic qualification

Required properties:

```text
peeling(peeling(G)) == peeling(G)          # idempotence

adding hard-invalid seeds
must never enlarge VALID_CAUSAL_CORE        # monotonicity

node iteration order
must not change final classification        # determinism
```

Also test reverse indexes, horizon-frontier crossing, source/session/reset propagation, AURA→C1→H1000 propagation, pair invalidation and outcome-independence of eligibility.

## Step 6 — Pre-retry causal-core qualification

Before another scientific root, use the validity engine on a bounded non-scientific/preflight trace if deterministic evidence alone is insufficient.

Require:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
```

The core must contain valid source, AURA, C1, Option-B, candidate lifecycle, H1000 and status-observer lineages with no unexpected cross-row retention.

## Step 7 — Fresh randomized G_action pilot

Only after infrastructure readiness closes:

```text
new immutable Kingston root
complete frozen preflight
complete frozen 96-block matrix
stop on first invalid block
```

Do not:

```text
retry a failed block inside the root
skip/replace the block
resample its arm
pool incomplete roots
analyze partial roots as science
```

## Step 8 — Scientific admission

A root is eligible for scientific analysis only after the complete frozen matrix is valid.

Then:

```text
complete valid randomized root
→ scientific G_action analysis
→ causal dataset acceptance
→ action-conditioned World-Model training
→ WISE predictive refinement
→ AEGIS integration
```

## Hard boundaries

The reverse/peeling layer must not modify:

```text
AURA
FAST/T1/C1
Direct Guard
M_STABLE_US
W_MAX_US
PX4_BOOT_US authority
AURA_C1_SOURCE_RESET authority
T_D/T_A
H1000
P1/P2 profiles
ZERO semantics
randomization
CALM/GUST semantics
SEALED
production authority
```

## Final retry-readiness states

```text
READY_FOR_FRESH_RANDOMIZED_G_ACTION_PILOT_RETRY

INFRASTRUCTURE_BLOCKER_IDENTIFIED_REPAIR_REQUIRED

VALIDITY_ENGINE_SEMANTIC_MISMATCH_OWNER_REVIEW_REQUIRED
```
