# Current Status — 2026-09-05

## Executive state

The AURA–WISE–World Model–AEGIS main pipeline remains active. The latest owner-authorized randomized `G_action` root, `fresh_35`, passed the frozen preflight and then stopped fail-closed in the first CALM row before any scientific block, candidate exposure, GUST event or manifest slot was admitted.

The first proven invalid boundary is **accepted-cycle status visibility to the probe matcher**, not `next_status` and not treatment response. A same-lineage valid native status exists within the existing source-match budget, so producer absence is not proven. Callback receipt versus bounded-ring eviction was not independently persisted, so eviction is also not proven.

```text
MAIN_PIPELINE=ACTIVE
PID_BENCHMARK_TRACK=RESEARCH_REFERENCE_ONLY

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
M_STABLE_US=100000
W_MAX_US=1000000
EVENT_SCHEDULING_POLICY=DELAY_RESCHEDULE_WITHIN_BLOCK
FAIL_CLOSED_TIMEOUT=true
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US

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
AEGIS_AUTHORITY=0
ROUND_Z_AUTHORITY=0
production_authority=false

AUTHORIZE_NEW_SCIENTIFIC_ROOT=false
AUTHORIZE_TIMEOUT_INCREASE=false
AUTHORIZE_QOS_CHANGE=false
AUTHORIZE_WM_TRAINING=false
AUTHORIZE_FAST_BASELINE_CHANGE=false
AUTHORIZE_SEALED_OPEN=false

NEXT_STATE=OWNER_ACCEPTED_CYCLE_STATUS_VISIBILITY_RETENTION_FORENSIC_REVIEW_REQUIRED
```

## Frozen scientific target

The scientific estimand and baseline are unchanged:

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

The current Phase-0 contract continues to freeze:

```text
AURA semantics
FAST/T1/C1 semantics
PX4 authority
Direct Guard
M_STABLE_US=100000
W_MAX_US=1000000
PX4_BOOT_US timing authority
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
T_D / T_A
H1000
ZERO/P1/P2 treatment profiles
randomization
CALM/GUST context semantics
SEALED boundary
production authority
```

Authoritative pilot identity remains:

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

## Qualified infrastructure retained before fresh_35

The following closures remain valid and were present in `fresh_35` preflight:

```text
Option-B / Direct Guard
continuous-C1 replay/recovery
post-reset E8 source-causal AURA/C1 pairing
native-event exact CLEAR/retirement lifecycle
next_status mirror/successor-frontier repair
TRACE_QOS=4096/RELIABLE/VOLATILE diagnostic evidence path
canonical WM reverse-index / graph / Tarjan / peeling engine
```

`fresh_35` preflight reported `PRE_RETRY_VALID_CAUSAL_CORE=true` before runtime execution.

## Latest immutable scientific-root attempt — fresh_35

Root:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm1_v2r1_within_run_randomized_action_20260905_fresh_35
```

Provenance:

```text
Git HEAD=a6cebe4d8f1e99a941ab7886efdd7d6a446143c3
Manifest SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
worktree entries=397
dirty fingerprint SHA256=c6d0bb85b2d07c8db90c564a183866d1915c10dc22a81ab8c2aeb4bacad9385c
```

Execution stopped in the first row:

```text
row=WM1V2R1_RAND_A_CALM_R1
sessions attempted/valid=1/0 of 8
blocks attempted/valid=1/0 of 96
requested cycle source frontier=38576000 PX4_BOOT_US
previous accepted timestamp=38232000
generation/session/reset=900706/632000/0
terminal=RuntimeError:accepted_cycle_timeout
next_status lookups/timeouts=0/0
```

No native GUST, scientific candidate, `T_D`, ACK, accepted exposure, H1000 completion, manifest slot or SEALED access occurred.

## First proven invalid boundary

The native status trace contains a same-lineage valid status:

```text
source frontier=38916000 PX4_BOOT_US
generation=900706
session=632000
reset=0
source_valid=true
source_fresh=true
gate_valid=true
applied_authority=true
```

The source delta from the requested cycle frontier is:

```text
38916000 - 38576000 = 340000 us
```

which is inside the existing `500000 us` source-match budget.

Observer diagnostics at timeout:

```text
thread_alive=true
error=null
status_count=2000
lineage_count=2
matching_statuses=[]
```

Current source behavior relevant to the failure:

```text
accepted-cycle matcher scans self.statuses[-2000:]
per-lineage latest status exists separately
find_cycle does not use the per-lineage latest slot
```

Therefore the evidence supports only:

```text
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN
```

Do not relabel this as proven eviction or callback loss until receipt/retention is independently instrumented.

## Other fresh_35 health evidence

```text
native status trace records=9997
E8 accepted-status events=25
E8 offer publications before failure=211
C1 replay records=19537
C1 positive frontiers=12651
C1 missing mutations=0
writer errors/drops/gaps=0/0/0
native GUST=0
scientific candidate/exposure credited=0
```

H1000 and GUST lifecycle were not reached in this CALM failure.

## Reverse processing / Tarjan / peeling

Offline causal-validity artifacts:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm_causal_validity_engine/fresh_35_20260905_01
```

Result:

```text
graph=21 nodes / 34 edges
Tarjan SCC=21 singleton components
forbidden cycles=0
graph_valid=true
reverse compact records=621
peeling iterations=10
VALID_CAUSAL_CORE=false
```

Direct failure seeds propagate through downstream scientific nodes; this remains an infrastructure-invalid root and is not pooled or reinterpreted as science.

## Immediate owner gate

The exact next task is a **separate forensic and minimal implementation review** of accepted-cycle status callback visibility and bounded retention.

The forensic must distinguish at least:

```text
A — contract-valid status never reached the observer callback
B — callback received it but bounded retention removed it before find_cycle
C — callback received/retained it but matcher/indexing logic could not select it
```

Required evidence should persist callback receipt identity and retention/index state for the exact generation/session/reset/source frontier. The repair, if any, must reuse canonical status semantics and remain implementation-preserving.

Forbidden before this boundary is proven and separately qualified:

```text
increase accepted-cycle timeout
increase/change QoS merely to obtain PASS
change source-time predicate
change session/reset semantics
run another randomized scientific root
patch inside fresh_35
pool partial roots
```

## FAST research boundary

The current FAST baseline is frozen for Phase-0. No FAST challenger is promoted into the scientific baseline now.

Separate shadow/replay research may compare the current FAST against bounded challengers on the same simulator to answer one question:

> Can immediate wind response, tracking and recovery be materially improved over the current `-d_hat + T1/C1` path without changing PX4 firmware control semantics?

No primary replacement algorithm is currently selected. The earlier residual-PI proposal is not current main-pipeline direction. Any future FAST promotion would redefine baseline `B` and require a new versioned control/scientific contract.

## World Model / WISE boundary

The canonical WM structure remains unchanged:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

with `G_action` defined relative to the frozen active baseline `B`.

World-Model training remains blocked because no complete randomized causal dataset has been accepted. Infrastructure/tooling/model-design work may continue, but no partial or failed root is training authority.

If a future FAST challenger is materially promoted after current Phase-0 closes, the production `G_action` model must be re-evaluated under the newly frozen baseline rather than assuming action-response invariance across different FAST semantics.

## Storage and authority boundaries

Large runtime/capture/dataset/training/intermediate artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

Failed-root classifications are immutable scientific history even if raw storage is later cleaned by explicit owner decision.

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
MODEL_TRAINING_ACTION_RESPONSE=BLOCKED_PENDING_CAUSAL_DATA_ACCEPTANCE
production_authority=false
```
