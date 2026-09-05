# Current Execution Ladder — WM Main Pipeline — 2026-09-05

## Purpose

This file is the execution-only ladder for the current AURA–WISE–WM–AEGIS main pipeline after the `fresh_33` randomized `G_action` pilot stopped fail-closed on a native-event lifecycle overlap.

It does not replace scientific contracts. It summarizes the next executable sequence from the latest qualified infrastructure state.

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
STATUS_OBSERVER_SOURCE_FRONTIER_REPAIR=CLOSED_QUALIFIED
CONTINUOUS_C1_REPLAY_RECOVERY=CLOSED_BOUNDED_QUALIFICATION
POST_RESET_E8_SOURCE_CAUSAL_PAIRING=QUALIFIED
POST_RESET_E8_SOURCE_CAUSAL_HANDOFF_QUALIFICATION=VALID_NONSCIENTIFIC

PRE_RETRY_VALID_CAUSAL_CORE_BEFORE_FRESH33=true

FRESH_RANDOMIZED_G_ACTION_PILOT=INCOMPLETE_INVALID_INFRASTRUCTURE_ROOT
FRESH_RANDOMIZED_G_ACTION_PILOT_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false

NEXT_STATE=INFRASTRUCTURE_REPAIR_REQUIRED_BEFORE_FRESH_RANDOMIZED_PILOT
```

## Frozen scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

No step below may change the estimand, treatment profiles, randomization, Direct Guard, `M_STABLE_US=100000`, `W_MAX_US=1000000`, `PX4_BOOT_US`, `AURA_C1_SOURCE_RESET`, `T_D/T_A`, H1000, SEALED, or production authority.

## Latest root — fresh_33

Immutable root:

```text
/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260905_fresh_33
```

Preflight passed with the qualified trace configuration and source-causal E8 handoff.

Acquisition reached:

```text
4 valid CALM session rows
+ first GUST_E row
```

The first GUST_E row stopped at block 3 because the collector rejected the new native-event arm while block 2 was still active:

```text
PREVIOUS_EVENT_STILL_ACTIVE
```

The downstream `gust_event_window_timeout:GUST_E:block:3:event:1` is not the first implementation divergence.

No partial root is scientific credit.

## Step 1 — Native-event lifecycle ownership audit

Identify the canonical owners for:

```text
native event arm request
native active state
native onset
native clear
active-state retirement
PREVIOUS_EVENT_STILL_ACTIVE rejection
block completion
next-block readiness
```

Extract the actual lifecycle state machine from source. Do not invent state names from the report.

Required output:

```text
exact state proving previous event is no longer active
exact event identity bound to that state
exact component that currently advances block N+1 before this state
```

## Step 2 — Scientific-timing semantic audit

Separate:

```text
INTER_BLOCK_READINESS
```

from:

```text
WITHIN_BLOCK_PREDECLARED_EVENT_TIMING
```

A repair is implementation-preserving only if waiting for the previous event's canonical CLEAR/retirement is an existing lifecycle prerequisite and does not silently change the frozen within-block schedule.

Require:

```text
INTER_BLOCK_LIFECYCLE_SEMANTIC_DELTA=NONE
```

Otherwise return to owner review before code changes.

## Step 3 — Implement canonical clear/retirement readiness

If Step 2 passes, add the smallest reusable synchronization rule:

```text
previous exact native event ACTIVE
→ observe exact matching CLEAR / retirement
→ next block becomes eligible to arm native event
```

Forbidden shortcuts:

```text
fixed sleep
force-clear collector state
overwrite active event
allow concurrent events
ignore PREVIOUS_EVENT_STILL_ACTIVE
arm-dependent delay
outcome-dependent delay
```

Do not use arithmetic across host monotonic, Gazebo simulation time, and PX4 boot time to infer readiness. Use lifecycle state/identity.

## Step 4 — Deterministic regression

Before live qualification, cover at least:

```text
ACTIVE → next arm rejected before repair
ACTIVE → matching CLEAR → next arm allowed after repair
clear already observed
missing clear → fail closed
wrong event-id clear → reject
duplicate clear
late stale clear
onset without clear
clear without matching onset
CALM → CALM
CALM → GUST
GUST → CALM
GUST → GUST
two consecutive GUST events
no arm-specific behavior
```

Existing status-observer, continuous-C1, E8 source-causal pairing, Option-B, H1000 and WM validity-engine regressions must remain unchanged.

## Step 5 — Bounded non-scientific consecutive-event qualification

Create a new task-specific qualification root. Do not run the 96-block scientific pilot.

Exercise enough consecutive native events to reproduce the problematic boundary:

```text
GUST block 1
→ onset
→ clear
→ GUST block 2 arm only after block 1 clear
→ onset
→ clear
→ GUST block 3 arm only after block 2 clear
→ onset
→ clear
```

Required evidence:

```text
PREVIOUS_EVENT_STILL_ACTIVE_REJECTIONS=0
OVERLAPPING_NATIVE_EVENTS=0
EVENT_IDENTITY_MATCH=PASS
INTER_BLOCK_CLEAR_GATE=PASS
BLOCK_ORDER=PASS
```

Also retain the already qualified infrastructure:

```text
TRACE_QOS_DEPTH=4096
TRACE_QOS_RELIABILITY=RELIABLE
TRACE_QOS_DURABILITY=VOLATILE
TRACE_QOS_ROLE=DIAGNOSTIC_EVIDENCE_ONLY

E8_SOURCE_CAUSAL_PAIRING=PASS
POST_RESET_E8_HANDOFF=PASS
C1_MISSING_REPLAYABLE_LIFECYCLE=0
STATUS_OBSERVER=PASS
WRITER_ERRORS=0
WRITER_DROPS=0
SEQUENCE_GAPS=0
```

No scientific block/action, manifest slot, SEALED access or production authority is permitted in this qualification.

## Step 6 — Reverse processing + peeling

Run the WM causal-validity engine offline on the new qualification root.

Require:

```text
graph_valid=true
forbidden_cycles=0
PRE_RETRY_VALID_CAUSAL_CORE=true
```

Do not run reverse processing or peeling in the control hot path.

## Step 7 — Owner review

Even after qualification PASS:

```text
DO NOT launch the next randomized pilot automatically
```

Return the qualification evidence to the owner.

The owner must separately authorize the next fresh scientific root.

## Step 8 — Next fresh randomized G_action pilot

Only after owner authorization:

```text
new immutable Kingston root
complete frozen preflight
8 sessions
96 total blocks
start from block 1
stop on first invalid block
```

Do not:

```text
retry failed block inside root
skip/replace block
resample arm
hot-fix code in root
change QoS in root
pool incomplete roots
analyze partial root as science
```

## Step 9 — Complete-root scientific admission

Only a complete valid root can enter scientific admission:

```text
8/8 sessions valid
96/96 blocks valid
source/session/reset continuity PASS
AURA/C1/E8/Direct Guard/H1000 PASS
C1 replay completeness PASS
VALID_CAUSAL_CORE complete
```

Then perform the separate causal-dataset audit:

```text
randomization integrity
context/arm support
ZERO/P1/P2 execution
T_D/T_A ordering
H1000 completeness
H0/H20/H40/H80 response availability
washout/contamination
no post-treatment conditioning
response construction
```

Output only:

```text
CAUSAL_DATASET_ACCEPTANCE=PASS | FAIL | INSUFFICIENT
```

## Step 10 — G_action / World Model boundary

If and only if causal-dataset acceptance passes:

```text
READY_FOR_G_ACTION_IDENTIFICATION
```

World-Model training still requires separate authorization.

The forward ladder is:

```text
complete valid randomized root
→ causal-dataset acceptance
→ G_action identification
→ action-conditioned World Model
→ WISE predictive refinement
→ AEGIS integration
```

## Hard boundaries

Never modify the experiment merely to obtain 96/96.

```text
failed root = immutable evidence
partial valid rows ≠ scientific dataset
infrastructure failure ≠ scientific negative result
```

## Current final state

```text
INFRASTRUCTURE_REPAIR_REQUIRED_BEFORE_FRESH_RANDOMIZED_PILOT
```
