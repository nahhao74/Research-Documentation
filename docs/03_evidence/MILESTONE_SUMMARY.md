# Milestone and Root-Cause Summary

This file is the compact audit trail for the Moving-Mode AURA–WISE–World Model–AEGIS vNext pipeline. Detailed runtime roots, telemetry, replay bundles and large artifacts remain outside this repository, primarily under `/media/nahhao74/KINGSTON`.

For current execution state, `CURRENT_STATUS.md` and the latest WM execution ladder outrank this summary.

## 1. Incremental candidate mechanism qualified

The candidate path is bounded additive augmentation to the active FAST/T1/C1 baseline rather than hidden replacement.

```text
B = PX4 + AURA + FAST/T1/C1
u_total_requested = u_baseline + u_candidate
```

Requested composition is additive; physical closed-loop response is not assumed linear.

## 2. E8 pending-ACK precedence closed

A valid candidate generation waiting for exact ACK could previously be superseded by unrelated baseline publication. Pending-ACK precedence was repaired and live-qualified.

Treatment exposure is defined by exact accepted candidate cycles bound to generation/session/reset/frontier identity, not intended publication alone.

## 3. Strict persistence and hot-path logging defects closed

Auxiliary non-finite diagnostics could break strict JSON persistence, and synchronous high-rate logging could perturb uXRCE delivery. Persistence and hot-path logging were repaired while required scientific invalid values still fail closed.

## 4. Native source authority separated from mapped time

An apparent mapped-time gap was shown to be a Timesync offset transition while native source continuity remained intact.

```text
SOURCE_CONTINUITY = native PX4 source time + generation
CLOCK_ALIGNMENT   = explicit causal mapping provenance
```

Host, Gazebo and PX4 source clocks are never silently collapsed.

## 5. Atomic SensorCombined provenance wire qualified

`SensorCombinedStampedV1` carries native source identity plus the exact sender mapping tuple captured atomically with the sample. Standard SensorCombined semantics were not changed.

## 6. StateBank startup barrier and bootstrap path closed

Session startup now requires all mandatory same-session streams before the causal snapshot barrier:

```text
aura, imu, attitude, local_state, reference, controller, actuator
```

The randomized-pilot bootstrap uses `bootstrap_only=true`, consumes no scientific manifest slot and does not seed candidate H1000.

## 7. Gazebo scientific-world readiness and identity closed

The runner waits for exact world-scoped create-service readiness before runtime use. Canonical world identity is:

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

```text
CALM   = identical plugin-bearing world; zero/no nonzero disturbance command
GUST_E = identical world/plugin bytes; frozen predeclared +E disturbance
```

## 8. H1000 bootstrap lifecycle closed

```text
SESSION_START_BOOTSTRAP != candidate release
H1000 = candidate-history only
REFRACTORY_US=1_000_000
```

## 9. Option-B / Direct Guard closed

The reference-stability delayed-launch mechanism reached bounded non-scientific qualification.

```text
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
```

This is mechanism/infrastructure qualification, not treatment-effect evidence.

## 10. Canonical causal-validity engine implemented

`WM_CAUSAL_VALIDITY_ENGINE` provides the shared offline/preflight/post-run validity pipeline:

```text
raw/source-grounded evidence
→ reverse validity index
→ canonical causal dependency graph
→ Tarjan SCC validation
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

Canonical graph shape is 21 nodes / 34 edges. Tri-state precedence is:

```text
FAIL > UNKNOWN > PASS
```

The engine does not run in the control hot path.

## 11. Continuous-C1 replay/recovery closed

`fresh_31` exposed missing replayable lifecycle evidence. The qualified diagnostic path is:

```text
TRACE_QOS_DEPTH=4096
RELIABLE
VOLATILE
DIAGNOSTIC_ONLY
```

Later qualification produced zero missing replayable C1 lifecycle references and zero writer errors/drops/gaps.

## 12. Post-reset E8 source-causal pairing closed

`fresh_32` exposed asynchronous AURA/C1 pairing through a mutable latest-AURA slot.

Canonical repair:

```text
bounded immutable AURA callback ledger
→ newest received positive-source nonfuture AURA record
→ existing exact session/reset/validity/freshness/provenance/clock gates
```

Hard rule:

```text
newer invalid/reset-mismatched AURA
must not be skipped for an older favorable record
```

Qualification demonstrated source-causal post-reset pairing, accepted status, source-forward successor and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

## 13. fresh_33 — native-event inter-block overlap

`fresh_33` passed preflight and later stopped when a subsequent GUST block attempted to arm before the previous native event reached exact canonical CLEAR:

```text
previous event ACTIVE
→ next arm request
→ PREVIOUS_EVENT_STILL_ACTIVE
→ downstream event-window timeout
```

Classification:

```text
INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

Partial CALM rows were never scientific dataset credit. The owner later deleted the raw root for storage management; the historical classification remains immutable in canonical summaries.

## 14. Native-event CLEAR lifecycle repair closed

Canonical lifecycle is now:

```text
arm
→ consume/onset
→ exact native CLEAR
→ clear
→ complete
→ retire
→ next block may arm
```

Qualified root:

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

## 15. fresh_34 — next_status successor-frontier failure

Fresh immutable root:

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

## 16. fresh_34 forensic — mirror predicate mismatch

Canonical `next_status` successor predicate:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

Native evidence contained 4568 same-lineage records and 338 strict-future candidates satisfying the lookup contract. The old E8 mirror filtered statuses on additional source-health/application-authority fields before observer publication.

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

## 17. next_status mirror repair and qualification closed

Canonical E8 repair:

```text
native status
→ mirror.publish(native_status)
→ existing ingress/source-health/ACK gate
```

No timeout, QoS, timestamp, session/reset, control, treatment or scientific semantic changed.

Qualified root:

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

Closure:

```text
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
```

## 18. fresh_35 — accepted-cycle successor unavailable to probe

The owner-authorized `fresh_35` root used the same frozen manifest and qualified infrastructure.

Root:

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

Preflight passed, including `PRE_RETRY_VALID_CAUSAL_CORE=true`, Direct Guard, E8 source-causal pairing, native CLEAR lifecycle, continuous-C1 replay and status-observer qualification.

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

## 19. fresh_35 first proven boundary

The native trace contains a valid same-lineage status:

```text
source frontier=38916000 PX4_BOOT_US
generation/session/reset=900706/632000/0
source_valid=true
source_fresh=true
gate_valid=true
applied_authority=true
```

It is `340000 us` after the requested frontier, inside the existing `500000 us` source-match budget. Producer absence is therefore not proven.

Observer diagnostics:

```text
thread_alive=true
error=null
status_count=2000
lineage_count=2
matching_statuses=[]
```

Current implementation evidence shows:

```text
accepted-cycle matcher scans self.statuses[-2000:]
per-lineage latest slot exists
find_cycle does not use the per-lineage latest slot
```

Callback receipt versus later retention loss was not independently persisted. Therefore the correct classification is:

```text
INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN
```

Do not upgrade this to proven ring eviction or callback loss without new evidence.

## 20. fresh_35 supporting health

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

Offline causal-validity artifacts:

```text
/media/nahhao74/KINGSTON/Detect_and_Response/
wm_causal_validity_engine/fresh_35_20260905_01
```

```text
graph=21 nodes / 34 edges
SCC=21 singleton components
forbidden cycles=0
graph_valid=true
reverse compact records=621
peeling iterations=10
VALID_CAUSAL_CORE=false
```

## 21. Current next gate — accepted-cycle visibility/retention forensic

The next owner decision is a separate forensic/minimal-repair task. It must distinguish:

```text
status never reached observer callback
vs
callback received but bounded retention removed it
vs
callback retained but matcher/indexing failed to select it
```

Required policy:

```text
no timeout increase
no QoS increase/change merely to obtain PASS
no source-time predicate weakening
no session/reset semantic change
no scientific retry before independent qualification
```

A repair must be implementation-preserving, followed by deterministic regression, bounded non-scientific qualification and canonical reverse/Tarjan/peeling before owner review.

## 22. Current scientific state

No accepted root has completed:

```text
8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

Therefore:

```text
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

No action-conditioned WM training is scientifically authorized.

## 23. FAST and WM research boundary

Current Phase-0 baseline remains:

```text
B = PX4 + AURA + FAST/T1/C1
```

FAST improvement may proceed only as separate simulator shadow/replay research. No replacement algorithm is currently selected, and the earlier residual-PI proposal is not the current main-pipeline direction.

WM structure remains:

```text
Y_future = F_nominal(X,h) + G_action(X,U_plan,h)
```

Because `G_action` is defined relative to baseline `B`, any future material FAST promotion requires a versioned baseline/model contract and re-evaluation of the action-conditioned model under that baseline.

## 24. Evidence retention policy

GitHub retains architecture, contracts, roadmap and compact audit trail. Large runtime roots and traces remain under project/Kingston storage.

Failed-root classifications remain immutable even if raw storage is later removed by explicit owner cleanup.
