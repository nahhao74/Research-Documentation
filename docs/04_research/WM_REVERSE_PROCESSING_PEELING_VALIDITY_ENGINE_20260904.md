# WM Reverse Processing + Peeling Validity Engine — 2026-09-04

## Status

```text
OWNER_SELECTED_DIRECTION=true
IMPLEMENTATION_STATUS=PENDING
SCIENTIFIC_CONTRACT_CHANGE=false
RUNTIME_HOT_PATH_CHANGE=false
```

This note records the selected use of **reverse processing** and **peeling (remove → update → repeat)** as an offline validity/forensic layer for the main WM randomized `G_action` pipeline.

The goal is to accelerate deterministic validation, explain propagation of invalid evidence, and identify the earliest causal divergence behind infrastructure failures. The algorithms do not change the estimand or relax existing contracts.

## Current pipeline position

```text
PX4
→ AURA
→ FAST/T1/C1
→ Option-B Direct Guard
→ randomized candidate action
→ accepted action
→ release / H1000
→ response horizons
→ G_action dataset
→ World Model
→ WISE
→ AEGIS
```

Current randomized-pilot blocker:

```text
fresh_25 = C1 mutation / continuous-trace completeness
fresh_26 = H1000 observer-retention timeout
fresh_27 = next_status_timeout; C1 child exited 1
```

No complete valid 96-block scientific root exists.

## Reverse processing

### Objective

For each accepted source-time record `t`, precompute the nearest future contract-relevant invalidation boundaries.

Representative fields:

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
next_disturbance_clear
next_H1000_invalid_frontier
next_block_end
next_session_end
```

Optional nearest-prior indices may include:

```text
prev_valid_AURA
prev_valid_C1
prev_reset
prev_disturbance_onset
prev_accepted_action
prev_valid_ZERO
```

### Frontier semantics

For an anchor, define the earliest future invalidation frontier only from conditions already forbidden by frozen contracts.

```text
T_NEXT_INVALID = min(contract-relevant future boundaries)
```

A response horizon can survive the temporal-frontier check only if it does not cross the relevant invalidation boundary.

This is an indexing optimization, not a new scientific rule.

### Intended artifact

```text
reverse_validity_index.jsonl
```

Candidate record:

```text
source_us
session_id
reset_generation
block_id
next_reset_source_us
next_source_gap_us
next_reference_change_us
next_AURA_invalid_us
next_C1_invalid_us
next_action_change_us
next_disturbance_change_us
next_block_end_us
valid_until_source_us
first_future_invalidator
```

## Causal dependency DAG

The validator should build an offline dependency DAG from actual contract ownership.

Candidate nodes:

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
RESPONSE_H0
RESPONSE_H20
RESPONSE_H40
RESPONSE_H80
ZERO_PAIR
TREATMENT_PAIR
BLOCK
SESSION
```

Representative chain:

```text
source
→ AURA valid
→ C1 valid
→ reference-stability eligibility
→ native event
→ AURA binding
→ T_D
→ candidate offer
→ ACK
→ accepted action
→ release / H1000
→ response horizon
→ treatment/zero pair
→ G_action dataset row
```

Temporal adjacency alone must never create a causal edge.

## Peeling

### Initial invalid seeds

Seed only directly observed failures already covered by existing contracts, for example:

```text
source gap / reorder / invalid timestamp
session mismatch
reset mismatch
AURA invalid/stale
C1 invalid/stale
reference-stability failure
native/AURA binding failure
candidate/ACK mismatch
accepted-action mismatch
release invalid
H1000 failure
response horizon crossing an existing forbidden frontier
block lifecycle failure
```

### Fixed-point rule

```text
S0 = all nodes not directly invalid

S(k+1) = nodes in S(k)
         whose mandatory dependencies remain admissible in S(k)

stop when S(k+1) == S(k)
```

Equivalent operational form:

```text
remove
→ update dependents
→ remove newly unsupported nodes
→ repeat
```

Output:

```text
VALID_CAUSAL_CORE
```

### Tri-state semantics

Retain fail-closed evidence semantics:

```text
FAIL > UNKNOWN > PASS
```

Rules:

```text
mandatory dependency FAIL
→ dependent FAIL

mandatory dependency UNKNOWN
→ dependent UNKNOWN unless another hard FAIL exists

optional diagnostic UNKNOWN
→ does not automatically invalidate
```

Missing evidence must never be coerced to PASS.

## Explainability requirements

Every peeled node must retain:

```text
node_id
node_type
direct_or_propagated
first_invalid_reason
upstream_invalid_node
causal_path
source_time
session_id
reset_generation
block_id
```

The validator must distinguish:

```text
ROOT_CAUSE
vs
DOWNSTREAM_CONSEQUENCE
```

This is especially important for failures reported as generic timeouts.

## Use on current infrastructure failures

Apply the engine to surviving logs/evidence for the latest failure classes.

### fresh_25

Determine the earliest break behind:

```text
C1 mutation / continuous-trace completeness
```

Classify among source mutation, collector loss, C1 publisher lifecycle, reset, process restart, serialization, consumer retention or validator interpretation.

### fresh_26

Trace:

```text
accepted candidate
→ release
→ H1000 observer creation
→ observer retention
→ source progression
→ endpoint
```

Determine whether the observer was never created, retired early, lost binding, starved after process failure, or remained present but unconsumed.

### fresh_27

Trace C1 child process lifecycle and classify `next_status_timeout` as root cause or downstream symptom.

If C1 child exit precedes the parent timeout, preserve that causal ordering explicitly.

## Dataset construction use

After a complete randomized root exists, the same engine may qualify the scientific dataset.

Reverse indices accelerate construction of contract-valid horizon and pairing candidates. Peeling propagates upstream invalidity to dependent response rows and treatment/ZERO pairs.

Pair selection itself remains controlled by the frozen randomized dataset contract. Nearest-neighbor indexing must not silently redefine baseline pairing.

## Required tests

At minimum:

```text
reverse next-reset index
reverse next-source-gap index
reverse next-action index
horizon crossing invalidation
one-hop peeling
multi-hop peeling
fixed-point convergence
FAIL > UNKNOWN > PASS precedence
session/reset propagation
AURA→C1→H1000 propagation
ZERO/treatment pair invalidation
outcome independence of eligibility
iteration-order determinism
```

Required algebraic properties:

```text
peeling(peeling(G)) == peeling(G)
```

and adding new hard-invalid seeds must never enlarge `VALID_CAUSAL_CORE`.

## Performance target

Report:

```text
rows
nodes
edges
reverse-index runtime
peeling runtime
peak RAM
comparison with current validator where available
classification equivalence on known-valid roots
```

The intended benefit is:

```text
faster validation
less repeated scanning
deterministic fixed-point behavior
better root-cause explainability
```

not a different scientific acceptance result.

## Hard scientific boundaries

Do not modify:

```text
G_action estimand
AURA semantics
FAST/T1/C1 semantics
Direct Guard
M_STABLE_US=100000
W_MAX_US=1000000
PX4_BOOT_US timing authority
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
T_D / T_A
H1000 scientific semantics
P1/P2 treatment profiles
ZERO semantics
randomization
CALM/GUST context definitions
SEALED
production authority
```

## Admission rule before fresh retry

Before a new full randomized pilot, the repaired infrastructure/preflight trace should satisfy:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
```

Only then proceed to one new immutable 96-block root.

Reverse processing answers:

> What contract-relevant invalidating event comes next?

Peeling answers:

> If this evidence is invalid, which downstream scientific evidence becomes unusable?

Together:

```text
RAW TRACE
→ REVERSE FRONTIERS
→ CAUSAL DEPENDENCY DAG
→ INVALID SEEDS
→ PEELING FIXED POINT
→ VALID_CAUSAL_CORE
→ G_action DATASET
```
