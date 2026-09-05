# Milestone and Root-Cause Summary

This file is the compact evidence trail for the Moving-Mode AURA–WISE–World Model–AEGIS vNext pipeline.

Detailed runtime roots, telemetry and replay bundles remain outside GitHub, primarily under `/media/nahhao74/KINGSTON`.

For current execution state, use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
```

This file records evidence and historical closures only. It does not define active research direction.

## 1. Closed-loop candidate architecture established

The candidate path is bounded additive augmentation to the active baseline:

```text
B = PX4 + AURA + FAST/T1/C1
u_total_requested = u_baseline + u_candidate
```

Requested composition is additive. Physical closed-loop superposition is not assumed.

PX4 remains authoritative downstream of the qualified acceleration-correction boundary.

## 2. Exact candidate identity and exposure qualified

Pending-ACK precedence was repaired so a valid candidate generation awaiting exact ACK cannot be silently superseded by unrelated baseline publication.

Scientific treatment is defined by exact accepted cycles bound to:

```text
generation
controller session
reset identity
source frontier
candidate identity
```

Intended publication alone is not exposure.

## 3. Source/time provenance infrastructure closed

Key closures include:

```text
strict JSON persistence without masking scientific invalidity
bounded hot-path logging / uXRCE evidence path
native source continuity separated from mapped time
atomic SensorCombined timestamp/provenance wire
StateBank seven-stream startup barrier
bootstrap_only session-start path
Gazebo scientific-world readiness and identity
H1000 candidate-history bootstrap semantics
```

Canonical source rule:

```text
SOURCE_CONTINUITY = native PX4 source time + generation
CLOCK_ALIGNMENT   = explicit causal mapping provenance
```

## 4. Option-B / Direct Guard qualified

```text
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
```

This is bounded infrastructure/mechanism qualification, not treatment-effect evidence.

## 5. Canonical causal-validity engine implemented

The shared validity path is:

```text
raw/source-grounded evidence
→ reverse validity index
→ canonical 21-node / 34-edge dependency graph
→ Tarjan SCC / forbidden-cycle validation
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

Tri-state precedence:

```text
FAIL > UNKNOWN > PASS
```

This engine is offline/preflight/post-run only; it does not run in the control hot path.

## 6. Continuous-C1 replay/recovery closed

`fresh_31` exposed missing replayable lifecycle evidence.

The qualified diagnostic transport became:

```text
TRACE_QOS_DEPTH=4096
RELIABLE
VOLATILE
DIAGNOSTIC_ONLY
```

Subsequent bounded qualification demonstrated zero missing replayable C1 lifecycle references and zero writer errors/drops/gaps.

## 7. Post-reset E8 source-causal pairing closed

`fresh_32` exposed asynchronous AURA/C1 pairing through a mutable latest-state pattern.

Canonical repair:

```text
bounded immutable AURA callback ledger
→ newest received positive-source nonfuture AURA record
→ exact session/reset/validity/freshness/provenance/clock gates
```

Hard rule:

```text
newer invalid/reset-mismatched AURA
must not be skipped for an older favorable record
```

A bounded qualification demonstrated source-causal post-reset pairing and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

## 8. fresh_33 — native-event lifecycle failure

`fresh_33` stopped after a subsequent GUST block attempted to arm while the previous native event was still active.

```text
previous event ACTIVE
→ next arm request
→ PREVIOUS_EVENT_STILL_ACTIVE
→ downstream event-window failure
```

Classification remains:

```text
INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

Partial CALM rows were never scientific dataset credit.

The raw root was later removed by explicit owner storage cleanup; the historical classification remains immutable.

## 9. Native-event CLEAR lifecycle repair qualified

Canonical inter-block lifecycle:

```text
arm
→ consume/onset
→ exact native CLEAR
→ clear
→ complete
→ retire
→ next block may arm
```

Qualification root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_native_event_clear_lifecycle_qualification_20260905_01
```

Result:

```text
3/3 consecutive GUST blocks PASS
PREVIOUS_EVENT_STILL_ACTIVE_REJECTIONS=0
OVERLAPPING_NATIVE_EVENTS=0
INTER_BLOCK_CLEAR_GATE=PASS_3_OF_3
PRE_RETRY_VALID_CAUSAL_CORE=true
SCIENTIFIC_SEMANTIC_DELTA=NONE
```

## 10. fresh_34 — next_status successor-frontier failure

Immutable root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_within_run_randomized_action_20260905_fresh_34
```

It failed in the first CALM row before scientific admission:

```text
FIRST_INVALID_COMPONENT=NEXT_STATUS_SOURCE_FRONTIER_UNAVAILABLE
previous_timestamp_us=81280000
terminal=RuntimeError:next_status_timeout
sessions valid=0/8
blocks valid=0/96
```

No GUST, candidate, `T_D`, ACK, exposure, H1000 completion, manifest slot or SEALED access occurred.

## 11. fresh_34 forensic — mirror predicate mismatch

Canonical successor predicate:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

Native evidence contained contract-valid strict-future successors, but the old E8 mirror applied additional runtime health/application-authority filtering before observer publication.

First proven chain:

```text
native strict-future status exists
→ old E8 mirror filter rejects it
→ observer cannot see contract-valid successor
→ bounded next_status lookup
→ next_status_timeout
```

Classification:

```text
NEXT_STATUS_FORENSIC_CLASS=B_STATUS_PUBLISHED_NOT_MIRRORED_FILTER_MISMATCH
CAPACITY_CAUSALITY=NOT_PROVEN
WATERMARK_VALIDITY=VALID
```

## 12. next_status mirror repair qualified

Canonical E8 repair:

```text
native status
→ mirror.publish(native_status)
→ existing ingress/source-health/ACK gate
```

No timeout, QoS, timestamp, session/reset, control, treatment or scientific semantic changed.

Qualification root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_next_status_frontier_qualification_20260905_03
```

Result:

```text
native callbacks=800
mirror callbacks=794
strict-future successor lookups=689
next_status timeouts=0
C1 records=19280
C1 missing lifecycle=0
writer errors/drops/gaps=0/0/0
graph_valid=true
forbidden_cycles=0
PRE_RETRY_VALID_CAUSAL_CORE=true
```

## 13. fresh_35 — accepted-cycle successor unavailable to probe

Owner-authorized immutable root:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm1_v2r1_within_run_randomized_action_20260905_fresh_35
```

Provenance:

```text
Git HEAD=a6cebe4d8f1e99a941ab7886efdd7d6a446143c3
MANIFEST_SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
worktree entries=397
dirty fingerprint SHA256=c6d0bb85b2d07c8db90c564a183866d1915c10dc22a81ab8c2aeb4bacad9385c
```

Frozen preflight passed, including:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
Direct Guard
E8 source-causal pairing
native CLEAR lifecycle
continuous-C1 replay
status-observer qualification
```

Execution stopped in the first CALM row:

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

## 14. fresh_35 first proven boundary

Native trace contains a valid same-lineage status:

```text
source frontier=38916000 PX4_BOOT_US
generation/session/reset=900706/632000/0
source_valid=true
source_fresh=true
gate_valid=true
applied_authority=true
```

It is `340000 us` after the requested frontier, inside the existing `500000 us` source-match budget.

Therefore:

```text
PRODUCER_ABSENCE=NOT_PROVEN
```

Observer diagnostics:

```text
thread_alive=true
error=null
status_count=2000
lineage_count=2
matching_statuses=[]
```

Implementation evidence:

```text
accepted-cycle matcher scans self.statuses[-2000:]
per-lineage latest slot exists
find_cycle does not use the per-lineage latest slot
```

Callback receipt versus later retention loss was not independently persisted.

Correct classification:

```text
INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN
```

Do not upgrade this to proven callback loss or proven eviction without new evidence.

## 15. fresh_35 supporting health and validity

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

Offline validity artifacts:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm_causal_validity_engine/fresh_35_20260905_01
```

Result:

```text
graph=21 nodes / 34 edges
SCC=21 singleton components
forbidden cycles=0
graph_valid=true
reverse compact records=621
peeling iterations=10
VALID_CAUSAL_CORE=false
```

## 16. Current scientific state

No accepted root has completed the frozen 8-session / 96-block campaign.

Therefore:

```text
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

No action-conditioned World-Model training is scientifically authorized.

## 17. Current next gate

The next task is a separate accepted-cycle callback visibility/retention forensic that distinguishes:

```text
callback not received
vs
received then removed from bounded retention
vs
retained but not selectable by matcher/indexing
```

Policy:

```text
no timeout increase
no QoS change merely to obtain PASS
no source-time predicate weakening
no session/reset semantic change
no new scientific root before independent qualification
```

A repair must be implementation-preserving and followed by deterministic regression, bounded non-scientific qualification, canonical validity audit and owner review.

## 18. Evidence retention rule

GitHub retains canonical architecture, contracts, active roadmap and compact audit trail. Large runtime evidence remains in project/Kingston storage.

Failed-root classifications remain immutable even if raw artifacts are later removed by explicit owner storage cleanup.
