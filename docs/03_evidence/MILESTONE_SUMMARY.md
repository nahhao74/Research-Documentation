# Milestone and Root-Cause Summary

This file is the compact current audit trail for the Moving-Mode AURA–WISE–World Model–AEGIS vNext pipeline. Detailed reports, telemetry and runtime roots remain outside this repository, primarily under `/media/nahhao74/KINGSTON`.

For current execution state, `CURRENT_STATUS.md` and the latest WM execution ladder outrank this summary. Historical milestone text remains recoverable through Git history and retained runtime evidence.

## 1. Incremental candidate mechanism qualified

The candidate path was proven as bounded additive augmentation to the active FAST/T1/C1 baseline rather than hidden replacement.

```text
B = PX4 + AURA + FAST/T1/C1
u_total_requested = u_baseline + u_candidate
```

Requested composition is additive; physical closed-loop response is not assumed linear.

## 2. E8 pending-ACK precedence closed

A valid candidate generation waiting for exact ACK could previously be superseded by unrelated baseline publication. Pending-ACK precedence was repaired and live-qualified.

Treatment exposure is defined by exact accepted candidate cycles bound to generation/session/reset/frontier identity, not intended publication alone.

## 3. Strict persistence/serialization closed

Auxiliary non-finite diagnostics could break strict JSON persistence after otherwise valid runtime activity. Persistence was repaired while required scientific non-finite fields still fail closed.

## 4. uXRCE hot-path logging stall closed

Large SensorCombined delivery gaps were localized to synchronous high-rate logging/stdout flushing in the uXRCE update path. Hot-path diagnostics were bounded/removed while low-overhead counters were retained.

## 5. Native source authority separated from mapped time

An apparent mapped-time gap was shown to be a Timesync offset transition while native source continuity remained intact.

```text
SOURCE_CONTINUITY = native PX4 source time + generation
CLOCK_ALIGNMENT   = explicit causal mapping provenance
```

## 6. Atomic SensorCombined provenance wire qualified

`SensorCombinedStampedV1` carries native source identity plus the exact sender mapping tuple captured atomically with the sample. Standard SensorCombined semantics were not changed.

## 7. StateBank startup barrier closed

A fresh root previously requested a session-start snapshot before all required streams were present.

Repair:

```text
all-required-stream readiness barrier
+
atomic snapshot-barrier recheck
```

Required streams:

```text
aura, imu, attitude, local_state, reference, controller, actuator
```

## 8. Specialized `bootstrap_only` call site closed

The randomized-pilot session-start call previously omitted `bootstrap_only=True`, causing accidental entry into candidate/C1 paths before science.

The call site and observer were repaired; bootstrap consumes no scientific manifest slot and does not seed candidate H1000.

## 9. Gazebo create-service readiness closed

The generated scientific world could start before the exact world-scoped create service was discoverable. Repair waits for exact service readiness with a bounded fail-closed probe instead of enlarging the bridge timeout.

## 10. H1000 bootstrap lifecycle defect closed

A session bootstrap status was incorrectly retained as a candidate release anchor.

```text
SESSION_START_BOOTSTRAP != candidate release
H1000 = candidate-history only
REFRACTORY_US=1_000_000
```

## 11. Canonical native-disturbance world frozen

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

```text
CALM   = identical plugin-bearing world; zero/no nonzero disturbance command
GUST_E = identical world/plugin bytes; frozen predeclared +E disturbance
```

## 12. Option-B / Direct Guard closed

The reference-stability delayed-launch direction reached bounded non-scientific qualification.

```text
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
```

This is mechanism/infrastructure qualification, not treatment-effect evidence.

## 13. Reverse processing + Tarjan + peeling validity engine implemented

`WM_CAUSAL_VALIDITY_ENGINE` provides offline/preflight/post-run validity acceleration:

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

## 14. Continuous-C1 replay/recovery blocker closed

`fresh_31` exposed missing replayable C1 lifecycle references and a downstream host-only recovery diagnostic.

The qualified evidence transport became:

```text
TRACE_QOS_DEPTH=4096
RELIABLE
VOLATILE
DIAGNOSTIC_ONLY
```

Later bounded qualification produced zero missing replayable C1 lifecycle references and zero writer errors/drops/gaps.

## 15. Post-reset E8 source-causal pairing closed

`fresh_32` exposed asynchronous pairing through a mutable latest-AURA slot.

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

Qualification demonstrated a real reset transition, source-causal post-reset pairing, accepted status, source-forward successor and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

## 16. `fresh_33` native-event overlap — historical failure

`fresh_33` passed preflight, completed four CALM rows and entered the first GUST row. It stopped at GUST block 3:

```text
block 2 native event still ACTIVE
→ block 3 arm requested
→ PREVIOUS_EVENT_STILL_ACTIVE
→ block 3 native event not launched
→ downstream event-window timeout
```

Classification remains:

```text
INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
```

The four CALM rows were never scientific dataset credit.

The owner later deleted the raw `fresh_33` root during storage cleanup. The historical classification remains immutable and is preserved by canonical summaries; raw replay is unavailable.

## 17. Native-event CLEAR lifecycle repair closed

Canonical lifecycle owners were audited and the failure was localized to block completion/retirement occurring before the exact native CLEAR was confirmed.

New invariant:

```text
arm
→ consume/onset
→ exact native CLEAR
→ clear
→ complete
→ retire
→ next block may arm
```

The repair is implementation-preserving:

```text
INTER_BLOCK_LIFECYCLE_SEMANTIC_DELTA=NONE
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
C1 missing lifecycle=0
writer errors/drops/gaps=0/0/0
PRE_RETRY_VALID_CAUSAL_CORE=true
```

No scientific blocks/actions, manifest slots, SEALED access or production authority were used.

## 18. `fresh_34` — next-status successor-frontier failure

Fresh scientific root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_within_run_randomized_action_20260905_fresh_34
```

Identity:

```text
Git HEAD=a6cebe4d8f1e99a941ab7886efdd7d6a446143c3
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
MANIFEST_SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
```

Preflight passed, including the qualified native-CLEAR lifecycle.

The root stopped in the first CALM row before any scientific block was admitted:

```text
FIRST_INVALID_COMPONENT=NEXT_STATUS_SOURCE_FRONTIER_UNAVAILABLE
previous_timestamp_us=81280000
session=360000
reset=1
recorded_failure_frontier=79896000
terminal=RuntimeError:next_status_timeout
sessions valid=0/8
scientific blocks valid=0/96
```

No GUST, candidate, `T_D`, ACK, exposure, H1000 completion, manifest slot or SEALED access occurred.

C1 replay remained healthy:

```text
records=19630
missing lifecycle references=0
writer errors/drops/sequence gaps=0/0/0
```

Reverse/peeling was graph-valid with zero forbidden cycles, but the root causal core became false from the infrastructure invalid seed.

Historical classification:

```text
FRESH_RANDOMIZED_G_ACTION_PILOT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

## 19. fresh_34 forensic — valid successors existed

Exact canonical successor predicate:

```text
timestamp_us > previous_timestamp_us
AND controller_session_start_us == expected_session
AND reset_generation == expected_reset
AND timestamp_ready
```

The native publisher contained 4568 same-lineage records before the observer deadline and 338 strict-future candidates satisfying this predicate. The first strict-future candidate mapped to `timestamp_us=81300000` with the expected session/reset and `timestamp_ready=true`.

The old E8 mirror applied additional filtering before publication:

```text
source_valid
source_fresh
gate_valid
applied_authority
```

Those fields are not part of `next_status` lookup semantics.

First implementation chain:

```text
native strict-future status exists
→ old E8 mirror health filter rejects it
→ observer cannot see contract-valid successor
→ bounded next_status lookup
→ next_status_timeout
```

Classification:

```text
NEXT_STATUS_FORENSIC_CLASS=B_STATUS_PUBLISHED_NOT_MIRRORED_FILTER_MISMATCH
CAPACITY_CAUSALITY=NOT_PROVEN
WATERMARK_VALIDITY=VALID
MIXED_DOMAIN_SENSOR_SAMPLE=INDEPENDENT
```

## 20. Next-status mirror repair and qualification closed

Canonical E8 repair:

```text
native status
→ mirror.publish(native_status)
→ existing ingress/source-health/ACK gate
```

No timeout, QoS, timestamp, session/reset, control, treatment or scientific semantic changed.

```text
SCIENTIFIC_SEMANTIC_DELTA=NONE
INFRASTRUCTURE_RETENTION_ONLY
```

Qualified root:

```text
/media/nahhao74/KINGSTON/Detect_and_Respond/
wm1_v2r1_next_status_frontier_qualification_20260905_03
```

Qualification result:

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

Final closure state:

```text
NEXT_STATUS_SOURCE_FRONTIER_REPAIR=QUALIFIED_IMPLEMENTATION_PRESERVING
NEXT_STATUS_SUCCESSOR_QUALIFICATION=VALID_NONSCIENTIFIC
NEXT_STATUS_SUCCESSOR_FRONTIER_PASS_READY_FOR_OWNER_PILOT_REVIEW
```

## 21. Owner authorizes the next fresh scientific pilot

After reviewing the next-status qualification, the owner authorized exactly one new fresh randomized scientific root using the frozen manifest and all qualified infrastructure unchanged.

Authorized:

```text
new immutable root
full preflight
8 sessions / 96 blocks
stop on first invalid block
canonical reverse/Tarjan/peeling validation
```

Not authorized:

```text
0B.5 rerun
retry/skip/replace/resample inside a root
hot-fix during root
partial-root science
World-Model training
SEALED opening
production authority
```

Current state:

```text
OWNER_FRESH_RANDOMIZED_PILOT_AUTHORIZED=true
NEW_FRESH_RANDOMIZED_PILOT_EXECUTED=false
NEXT_STATE=OWNER_AUTHORIZED_NEW_FRESH_RANDOMIZED_PILOT_READY_TO_EXECUTE
```

## 22. Remaining scientific gate

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

Therefore there is still no accepted treatment-SNR result for `G_action` and no scientifically authorized action-conditioned WM training.

Only a complete 96/96 root can proceed to a separate causal-dataset admission audit.

## 23. Post-Phase-0 FASTv2 research direction

The independent PID benchmark branch is now being used as a research reference, not as current runtime authority.

Most feasible future challenger identified so far:

```text
PX4 firmware PID/inner loops unchanged
+
AURA disturbance feedforward (-d_hat)
+
T1/C1 temporal continuation
+
bounded residual 2-DOF PI at the qualified acceleration-correction boundary
```

Initial design rule:

```text
fixed robust PI first
anti-windup required
bounded output
source/session/reset/lifecycle-aware integrator
identify the residual closed-loop plant with current PX4+AURA+FAST/T1/C1 active
no gain scheduling until a deployable causal scheduling state is supported
INDI only as a later ablation challenger
DOB/ESO/ANN only after a measured residual failure class remains
```

This research must not alter the current frozen baseline `B` before Phase-0 closes.

## 24. Forward development path

```text
owner-authorized fresh complete randomized root
→ causal dataset acceptance
→ G_action identification
→ F_nominal + G_action World Model
→ WISE bounded predictive planning
→ latency/AoI/freshness work
→ uncertainty calibration
→ FASTv2 / CALE / adaptation research under new versioned contracts
→ stronger AEGIS runtime assurance
```

## 25. Evidence retention policy

GitHub keeps architecture, contracts, roadmap and compact audit trail. Detailed runtime reports, raw roots, telemetry, counters, replay bundles and scientific data remain in the project artifact hierarchy and Kingston storage.

Historical classifications remain immutable even when raw failed roots are later deleted by the owner for storage management.
