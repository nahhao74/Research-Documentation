# WM Causal Validity Engine

## Status

```text
WM_CAUSAL_VALIDITY_ENGINE=IMPLEMENTED_AND_TESTED
CANONICAL_GRAPH=21_NODES_34_EDGES
TARJAN_EXPECTATION=NO_FORBIDDEN_CYCLES
RUNTIME_HOT_PATH_CHANGE=false
SCIENTIFIC_CONTRACT_CHANGE=false
```

This file documents the implemented offline/preflight/post-run validity mechanism used by the WM randomized `G_action` pipeline.

## Canonical pipeline

```text
raw/source-grounded evidence
→ reverse validity indexing
→ canonical source-grounded dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

The engine accelerates deterministic validation and root-cause explanation. It does not change the estimand, treatment definition, controller semantics or acceptance rules.

## Reverse processing

For each source-ordered record, the validator precomputes relevant future invalidation boundaries such as:

```text
next source gap/reorder/invalidity
next reset/session change
next reference transition
aura/c1 invalidity
candidate/action/release boundaries
disturbance change/clear
block/session end
```

The resulting `valid_until_source_us` and first invalidator are indexing aids only. They never create a new scientific rule.

Canonical artifact:

```text
artifacts/reverse_validity_index.jsonl
```

## Causal dependency graph

The graph encodes mandatory causal ownership, not temporal adjacency.

Current canonical shape:

```text
21 nodes
34 mandatory edges
```

Representative scientific path:

```text
source/session/reset
→ AURA / C1 / reference / event validity
→ T_D / candidate offer / ACK
→ accepted action / T_A
→ release / H1000
→ H0/H20/H40/H80 response
→ block/session scientific admissibility
```

Canonical artifact:

```text
artifacts/causal_dependency_graph.json
```

## Tarjan SCC validation

Tarjan is static/offline graph validation only.

Expected valid structure:

```text
21 singleton SCCs
0 forbidden self-cycles
0 forbidden multi-node cycles
graph_valid=true
```

Tarjan never runs in AURA/AEGIS/PX4 callbacks.

## Fixed-point peeling

Direct failures seed invalidity. Mandatory dependents then fail/unknown according to the existing graph until a fixed point is reached.

Tri-state precedence:

```text
FAIL > UNKNOWN > PASS
```

Rules:

```text
mandatory FAIL    → dependent FAIL
mandatory UNKNOWN → dependent UNKNOWN unless another hard FAIL exists
optional diagnostic UNKNOWN → does not invalidate parent by itself
```

Required properties:

```text
idempotent
monotone under additional hard-invalid seeds
iteration-order deterministic
outcome-blind eligibility
```

Canonical artifacts:

```text
artifacts/peeling_result.json
artifacts/invalidity_propagation.jsonl
```

## Explainability

Every removed node retains:

```text
node identity/type
direct vs propagated invalidity
first reason
upstream invalid node
causal path
source/session/reset/block identity
```

The validator must distinguish:

```text
ROOT_CAUSE
vs
DOWNSTREAM_CONSEQUENCE
```

A terminal timeout is not automatically the root cause.

## Current `fresh_35` interpretation

For the immutable `fresh_35` root:

```text
graph=21 nodes / 34 edges
Tarjan SCC=21 singleton components
forbidden cycles=0
graph_valid=true
reverse compact records=621
peeling iterations=10
VALID_CAUSAL_CORE=false
```

The direct infrastructure failure seed is the accepted-cycle/probe visibility failure. Downstream scientific nodes are invalidated by peeling.

This does not imply a treatment or `G_action` negative result.

## Current use

Use this engine for:

```text
bounded non-scientific qualification readiness
scientific-root post-run validity
failed-root causal explanation
future complete-root causal dataset admission
```

Do not create task-local replacement graph/peeling implementations when the canonical engine applies.

## Hard boundaries

The engine must not modify:

```text
G_action estimand
AURA semantics
FAST/T1/C1 semantics
Direct Guard
M_STABLE_US
W_MAX_US
PX4_BOOT_US authority
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
T_D / T_A
H1000
ZERO/P1/P2 treatment semantics
randomization
CALM/GUST definitions
SEALED
production authority
```

## Current readiness rule

A new scientific root may only be considered after the relevant infrastructure qualification reaches:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
```

That condition is necessary but never sufficient for scientific authorization; owner review remains separate.
