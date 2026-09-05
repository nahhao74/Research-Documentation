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

Persistence remains a qualification gate.

## 4. uXRCE hot-path logging stall closed

Large SensorCombined delivery gaps were localized to synchronous high-rate logging/stdout flushing in the uXRCE update path. Hot-path diagnostics were bounded/removed while low-overhead counters were retained.

Verbose tracing remains disabled in scientific runtime when it perturbs timing.

## 5. Native source authority separated from mapped time

An apparent mapped-time gap was shown to be a Timesync offset transition while native source continuity remained intact.

The resulting rule is:

```text
SOURCE_CONTINUITY = native PX4 source time + generation
CLOCK_ALIGNMENT   = explicit causal mapping provenance
```

A mapping transition is not a native source gap.

## 6. Atomic SensorCombined provenance wire qualified

`SensorCombinedStampedV1` carries native source identity plus the exact sender mapping tuple captured atomically with the sample. Standard SensorCombined semantics were not changed.

## 7. StateBank startup barrier closed

A fresh root previously requested a session-start snapshot before all required streams, especially AURA, were present.

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

The generated scientific world could start before the exact world-scoped create service was discoverable, causing a bridge startup timeout.

Repair waits for exact service readiness with a bounded fail-closed probe instead of enlarging the bridge's internal timeout.

## 10. H1000 bootstrap lifecycle defect closed

A session bootstrap status was incorrectly retained as a candidate release anchor.

Repair restored:

```text
SESSION_START_BOOTSTRAP != candidate release
H1000 = candidate-history only
REFRACTORY_US=1_000_000
```

## 11. Canonical native-disturbance world frozen

The plugin-bearing generated world became the canonical vNext scientific world:

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

Frozen values:

```text
OPTION_B_SELECTED_ELIGIBILITY_ENGINE=DIRECT_GUARD
M_STABLE_US=100000
W_MAX_US=1000000
AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
RESET_AUTHORITY=AURA_C1_SOURCE_RESET
```

This is mechanism/infrastructure qualification, not treatment-effect evidence.

## 13. Reverse processing + peeling validity engine implemented

`WM_CAUSAL_VALIDITY_ENGINE` now provides offline/preflight/post-run validity acceleration:

```text
raw trace
→ reverse validity index
→ causal dependency graph
→ direct invalid seeds
→ fixed-point peeling
→ VALID_CAUSAL_CORE
```

Current canonical graph shape is 21 nodes / 34 edges with Tarjan cycle checks. Tri-state precedence is fail-closed:

```text
FAIL > UNKNOWN > PASS
```

The engine does not run in the control hot path and does not create new scientific validity rules.

## 14. Status-observer/source-frontier blocker closed

Later fresh roots showed valid native statuses could exist while the observer path did not retain the required successor status.

The canonical E8 mirror was repaired to expose every contract-valid native status on the reliable observer path without weakening strict source/session/reset lookup semantics.

Bounded non-scientific qualification established:

```text
PRE_RETRY_VALID_CAUSAL_CORE=true
```

Historical failed roots remain immutable.

## 15. Continuous-C1 replay/recovery blocker closed

`fresh_31` exposed missing replayable C1 lifecycle references and a downstream host-only `reset_transition_order_invalid` diagnostic.

Forensic result preserved the distinction:

```text
source-bound missing lifecycle evidence
vs
host-only terminal recovery exception
```

The diagnostic trace path was qualified with:

```text
TRACE_QOS_DEPTH=4096
RELIABLE
VOLATILE
DIAGNOSTIC_ONLY
```

A later bounded qualification produced zero missing replayable C1 lifecycle references, zero writer errors/drops/gaps and valid causal core.

## 16. Post-reset E8 source-causal pairing closed

`fresh_32` exposed a structural problem in E8: asynchronous AURA/C1 pairing used one mutable latest-AURA slot, making the selected source-causal AURA record unprovable across reset transitions.

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

Qualification `_12` demonstrated a real reset transition, source-causal post-reset pairing, accepted status, source-forward successor and:

```text
POST_RESET_E8_SOURCE_CAUSAL_HANDOFF=PASS
PRE_RETRY_VALID_CAUSAL_CORE=true
```

## 17. `fresh_33` — current latest randomized pilot attempt

Immutable root:

```text
/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260905_fresh_33
```

Identity:

```text
Git HEAD=a6cebe4d8f1e99a941ab7886efdd7d6a446143c3
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
MANIFEST_SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
TRACE_QOS=4096_RELIABLE_VOLATILE_DIAGNOSTIC_ONLY
```

Preflight passed, including source-causal E8 pairing and `PRE_RETRY_VALID_CAUSAL_CORE=true`.

Acquisition reached:

```text
5 session rows attempted
4 CALM rows completed
first GUST_E row entered
```

The first invalid boundary occurred in GUST_E block 3:

```text
block 2 native event still ACTIVE
→ block 3 arm requested
→ collector rejects PREVIOUS_EVENT_STILL_ACTIVE
→ no valid block-3 native event
→ downstream gust_event_window_timeout
```

The arm rejection is a host-side diagnostic. Block-2 clear is retained in Gazebo simulation time. Those clock domains are not subtracted or converted into a fabricated PX4 source timestamp.

Infrastructure that remained healthy:

```text
C1 replay complete
missing replayable C1 lifecycle=0
writer errors/drops/gaps=0/0/0
E8 ledger not invalid seed
status observer not invalid seed
```

Reverse/peeling remained structurally valid but the root causal core became false from the infrastructure invalid seed.

Final classification:

```text
FRESH_RANDOMIZED_G_ACTION_PILOT_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

The four CALM rows are runtime evidence only; they are not pooled or analyzed as scientific treatment data.

## 18. Current blocker — native-event inter-block lifecycle

The next repair target is not E8/C1/status retention. It is the block-to-block native-event lifecycle:

```text
previous exact native event ACTIVE
→ observe exact matching canonical CLEAR / retirement
→ only then permit next native-event arm
```

The repair must be implementation-preserving and must not modify frozen within-block timing, GUST profile, `M_STABLE_US`, `W_MAX_US`, `T_D`, `T_A`, H1000, randomization or treatment arms.

Forbidden shortcut:

```text
fixed sleep
force-clear state
ignore PREVIOUS_EVENT_STILL_ACTIVE
overlap native events
```

## 19. Required next qualification

After deterministic repair, run a new bounded non-scientific consecutive-event qualification proving at least:

```text
block1 arm → onset → clear
block2 arm only after block1 clear → onset → clear
block3 arm only after block2 clear → onset → clear

PREVIOUS_EVENT_STILL_ACTIVE_REJECTIONS=0
OVERLAPPING_NATIVE_EVENTS=0
EVENT_IDENTITY_MATCH=PASS
INTER_BLOCK_CLEAR_GATE=PASS
PRE_RETRY_VALID_CAUSAL_CORE=true
```

Even a PASS does not automatically authorize the next randomized pilot; owner review remains required.

## 20. Remaining scientific gate

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

## 21. Forward development path

```text
native-event lifecycle closure
→ bounded nonscience qualification
→ owner-authorized fresh complete randomized root
→ causal dataset acceptance
→ G_action identification
→ F_nominal + G_action World Model
→ WISE bounded predictive planning
→ latency/AoI/freshness work
→ uncertainty calibration
→ CALE causal-learning/adaptation research
→ stronger AEGIS runtime assurance
```

See `../04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`.

## 22. Evidence retention policy

GitHub keeps architecture, contracts, roadmap and compact audit trail. Detailed runtime reports, raw roots, telemetry, counters, replay bundles and scientific data remain in the project artifact hierarchy and Kingston storage.

For forensic reconstruction, prefer exact local source/build identities and retained raw evidence over generic prose or upstream documentation.
