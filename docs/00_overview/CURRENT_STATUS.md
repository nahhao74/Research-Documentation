# Current Status — 2026-08-29

## Executive state

The Moving-Mode pipeline remains in **vNext-1 scientific-system closure**. Major transport, timestamp, provenance, bootstrap, Gazebo/world, H1000 and candidate-mechanism defects are closed. The latest fresh randomized pilot still stopped **before `FIRST_SCIENTIFIC_T_D`**, so no scientific block, treatment exposure or manifest slot was consumed.

Current blocking state:

```text
ROOT_VALIDITY_CLASS=INVALID_INFRASTRUCTURE_PRE_SCIENCE
FAILURE_BOUNDARY=PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT
C1_FRONTIER_DEEP_ROOT_CAUSE=UNRESOLVED
```

This is an integration/runtime blocker, not a scientific failure and not evidence that P1/P2 lack physical effect.

## What is already closed

### Candidate mechanism

The bounded incremental candidate path is live-qualified on top of the active FAST/T1/C1 baseline:

```text
B = PX4 + AURA + FAST/T1/C1
u_total_requested = u_baseline + u_candidate
```

No-candidate parity, ZERO parity, nonzero incremental path, stale fail-to-baseline, reset handling and H1000 regression all passed. Physical superposition is not assumed.

### Source transport and provenance

- uXRCE cross-topic hot-path logging head-of-line blocking was removed.
- Native SensorCombined source continuity is qualified with explicit source counters.
- `SensorCombinedStampedV1` carries atomic native source identity plus sender mapping provenance.
- Historical malformed wrapper SHA-256 was proven to be a ledger transcription error; the canonical current digest is:

```text
5cb31c66d07c9c2bdaa9a4a7c243f10e4e9e70423b28957dee87b10862a59d06
```

### Dual-domain time authority

```text
source continuity -> native PX4 source time + generation
clock alignment    -> separate causal mapping/provenance predicate
```

Mapped-time discontinuity is not treated as native source loss. A live clock-mapping transition during science remains fail-closed unless separately qualified.

### Gazebo/world startup

A pre-science `gz_bridge` model-spawn timeout was traced to a service-discovery race. The startup path now waits for the exact world-scoped create service before invoking the bridge.

The scientific world is now owner-frozen as the deterministic plugin-bearing world:

```text
WORLD_NAME=sim_world_a
CANONICAL_WORLD_SHA256=8b26be57f07380455071fe8f4f81797e8ca3b946bf407158ff91f0ac110f3b91
```

`NativeDisturbanceSystem` is present in both CALM and GUST_E contexts. CALM uses zero/no disturbance command; GUST_E uses the frozen +E scientific stimulus contract. The final generated world is hashed after preparation and before runtime launch.

A fresh non-scientific CALM requalification passed Gazebo/PX4/topics/source attestation/StateBank bootstrap, observed zero native disturbance events, stayed within the 35 m radius and cleaned up successfully.

### StateBank/bootstrap and H1000

StateBank session bootstrap requires all seven source-bound streams:

```text
aura, imu, attitude, local_state, reference, controller, actuator
```

The AURA bootstrap race and specialized `bootstrap_only=True` call-site defect are closed.

The later H1000 timeout was traced to a lifecycle bug: a baseline bootstrap-only status was incorrectly seeded as a candidate-release anchor. The repaired probe leaves `_last_release=None` after session-start bootstrap and only applies candidate-only H1000 after an actual candidate release. `REFRACTORY_US=1_000_000` and candidate-only semantics were unchanged.

## Latest randomized-pilot attempt

Latest root:

```text
/media/nahhao74/KINGSTON/
wm1_v2r1_within_run_randomized_action_20260829_native_disturbance_world_freeze_01
```

Pre-science gates that passed:

```text
WORLD_IDENTITY_GATE=PASS
GZ_SERVICE_READY=PASS
PX4_STARTUP=PASS
REQUIRED_RUNTIME_TOPICS=PASS
SOURCE_ATTESTATION=PASS
SOURCE_CONTINUITY=PASS_PRE_SCIENCE_ATTESTATION
CLOCK_ALIGNMENT=PASS_PRE_SCIENCE_ATTESTATION
ATOMIC_PROVENANCE=PASS_PRE_SCIENCE_ATTESTATION
STATEBANK_REQUIRED_STREAMS_PRESENT=7/7
BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED
BLOCK1_SNAPSHOT_ACK=VALID_ACCEPTED
```

The failure occurred after the first block snapshot while waiting for a valid C1 offer frontier:

```text
PRE_SCIENCE_C1_VALID_OFFER_FRONTIER_TIMEOUT
C1 callbacks continued
continuous-C1 records continued
published/canonical C1 source frontier remained 0
candidate offers = 0
candidate publishes = 0
E8 candidate transactions = 0
FIRST_SCIENTIFIC_T_D=NOT_REACHED
MANIFEST_SLOTS_CONSUMED=0
```

Three isolated diagnostics also reported:

```text
unexpected_domain expected=epoch received=boot
```

This is a strong timestamp-domain/binding hypothesis, but it is **not yet promoted to the deep root cause**.

## Immediate next task

Do **not** run another randomized pilot yet.

The next task is:

```text
C1_VALID_OFFER_FRONTIER_ROOT_CAUSE
+
FULL_PRE_SCIENCE_CORRIDOR_CLOSURE
```

The exact current pilot path must be exercised through:

```text
startup
-> source attestation
-> StateBank 7/7
-> session bootstrap_only
-> first-block snapshot
-> C1 valid-offer frontier
-> pre-offer source/clock/provenance checks
-> INTENTIONAL STOP immediately before FIRST_SCIENTIFIC_T_D
```

This corridor must use the real pilot runner/probe and must not enter candidate publication, E8 treatment, accepted exposure or manifest consumption.

The process rule going forward is:

```text
repair
-> deterministic regression
-> component qualification
-> FULL INTEGRATED PRE-SCIENCE CORRIDOR
-> only then randomized scientific root
```

The randomized pilot must no longer serve as an integration test.

## Scientific campaign still pending

The frozen campaign remains unchanged:

```text
worker A
8 sessions = 4 CALM + 4 GUST_E
12 scientific blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

No accepted root has completed this campaign, therefore there is still no accepted P1/P2 treatment-SNR result and no authorization to train the action-conditioned World Model.

## Long-term direction

The current future implementation roadmap is documented in:

```text
docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

The intended order after scientific closure is:

```text
valid G_action identification
-> end-to-end latency decomposition
-> T_D -> T_A delay/state baseline
-> AURA detector shadow bake-off
-> F_B + G_action model ladder
-> history ablation
-> uncertainty calibration
-> WISE-0 bounded candidate enumeration
-> event-triggered WISE
-> TinyMPC / Koopman / RTI only if justified
-> AEGIS safety-envelope specification
-> low-dimensional adaptation
-> formal safety projection
```

## Authority boundaries

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED_YET
R1=NOT_AUTHORIZED_YET
production_authority=false
```

Historical failed roots remain immutable. Large runtime/capture/dataset artifacts remain under `/media/nahhao74/KINGSTON` rather than this repository.
