# Current Status — 2026-08-29

## Executive state

The Moving-Mode pipeline has converged from broad infrastructure/debugging into a narrow scientific-readiness state. Major source, timestamp, provenance and bootstrap defects have been closed and qualified. The latest exact specialized-runner preflight passed with no candidate path entered and no scientific boundary crossed.

Current state:

```text
PILOT_BOOTSTRAP_ONLY_CALLSITE_REPAIR_QUALIFIED_READY_FOR_OWNER_FRESH_PILOT_RETRY_REVIEW
```

This is a **readiness** state, not a scientific result.

## What is already qualified

### Runtime/source transport

- PX4 / uXRCE SensorCombined source path is live-qualified.
- Earlier cross-topic head-of-line blocking caused by synchronous hot-path logging was mechanically removed.
- Source counters and writer diagnostics have subsequently shown clean transport in qualification roots.
- Agent verbose mode that perturbs timing remains disabled for scientific runtime.

### Timestamp authority

The old single mapped-time interpretation was retired after the canonical `183018 us` apparent gap was decomposed into:

```text
native source delta = 8000 us
timesync offset step = -175018 us
mapped apparent jump = 183018 us
```

Current authority is dual-domain:

```text
source continuity -> native PX4 source timestamp + generation
clock alignment    -> separate causal mapping/provenance predicate
```

`SensorCombinedStampedV1` carries atomic native source identity and exact sender mapping provenance. Standard SensorCombined semantics remain unchanged.

### StateBank/AURA bootstrap

StateBank requires seven source streams at bootstrap:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

A prior pre-science root requested the snapshot before AURA had entered StateBank. The repair added an explicit all-required-stream readiness barrier plus an atomic barrier recheck. Eight fresh bootstrap-only lifecycles passed.

### Specialized pilot bootstrap path

A later root proved StateBank/AURA readiness but the specialized pilot runner omitted `bootstrap_only=True` for `block_index=-1`, causing accidental entry into the candidate/C1 frontier path before science.

The call site and bootstrap observer were repaired. Current exact preflight evidence:

```text
STATEBANK_REQUIRED_STREAMS_PRESENT=7/7
BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED
BOOTSTRAP_ONLY_FLAG_LIVE=PASS
BOOTSTRAP_RETURNED_BEFORE_CANDIDATE_PATH=true
CANDIDATE_OFFER_COUNT=0
CANDIDATE_PUBLISH_COUNT=0
C1_OFFER_FRONTIER_WAIT_COUNT=0
E8_CANDIDATE_TRANSACTION_COUNT=0
FIRST_SCIENTIFIC_T_D=NOT_REACHED
MANIFEST_SLOTS_CONSUMED=0
REGRESSION_TEST_RESULT=PASS_51
```

## What is not yet scientifically closed

No accepted root has yet completed the frozen randomized campaign:

```text
8 sessions
4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

Therefore the project does not yet have an accepted treatment-SNR result for `G_action`, and no final action-conditioned model training decision should be inferred from infrastructure qualification.

## Immediate next gate

Owner review may authorize **exactly one fresh randomized pilot root** using the already frozen manifest and current qualified runtime identities.

The next root must preserve:

- FAST/T1/C1 baseline active;
- exact ZERO/P1/P2 exposure;
- native T_D / T_A authority;
- dual-domain clock fail-closed behavior;
- H1000 native-time semantics;
- first-invalid-root stop policy;
- SEALED lock;
- `production_authority=false`.

If the full root is valid, only then compute the frozen E0/E1/session-cluster bootstrap diagnostics and classify P1/P2 as `CLEAR`, `WEAK`, or `NOT_RESOLVED`.

## Transition to World Model training

Action-conditioned WM training is gated by a **complete valid randomized pilot**, not by absence of runtime errors alone.

The intended progression is:

```text
qualified runtime
    -> complete valid randomized pilot
    -> treatment SNR / carryover / interaction review
    -> owner causal-identification review
    -> action-conditioned dataset acceptance
    -> G_action model training / DEV evaluation
    -> WISE predictive refinement
```

If the pilot is valid but the action signal is weak, the next step is experiment-design review rather than automatic amplitude increase or training on weak labels.

## Authority boundaries

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
MODEL_TRAINING=NOT_AUTHORIZED_YET
R1=NOT_AUTHORIZED_YET
production_authority=false
```

Historical failed roots remain immutable and are summarized in `../03_evidence/MILESTONE_SUMMARY.md`; detailed runtime artifacts remain outside this repository.