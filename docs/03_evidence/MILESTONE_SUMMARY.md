# Milestone and Root-Cause Summary

This file replaces the previous large collection of report copies in the documentation tree. It records only the milestones needed to understand the current architecture, why key contracts exist, and what has already been qualified.

Detailed raw reports and runtime roots remain outside this repository.

## 1. Incremental candidate mechanism qualified

### Problem

The candidate path had to be proven as an additive bounded augmentation to the active FAST/T1/C1 baseline rather than a hidden replacement path.

### Closure

Live mechanism qualification established:

```text
NO_CANDIDATE_LIVE_PARITY=PASS
ZERO_CANDIDATE_LIVE_PARITY=PASS
NONZERO_INCREMENTAL_LIVE_PATH=PASS
STALE_FAIL_TO_BASELINE_LIVE=PASS
RESET_NO_STALE_REUSE_LIVE=PASS
H1000_V2_LIVE_REGRESSION=PASS
```

The exact requested composition boundary is additive; physical effect attribution remains nonlinear and requires randomized experiment evidence.

### Architectural consequence

Current scientific baseline remains active:

```text
B = PX4 + AURA + FAST/T1/C1
```

and candidate is incremental.

---

## 2. E8 pending-ACK precedence repaired

### Problem

A valid candidate generation waiting for exact ACK could be superseded by unrelated baseline publication before accepted exposure was certified.

### Closure

Pending-ACK precedence was repaired and qualified. Candidate identity remains generation/session/reset/frontier bound.

### Architectural consequence

Exact P1/P2 treatment exposure is defined by accepted candidate cycles, not by intended publication alone.

---

## 3. Strict qualified-record serialization repaired

### Problem

Auxiliary non-finite values could cause strict JSON persistence to fail after otherwise valid runtime activity.

### Closure

Serializer was repaired so auxiliary non-finite diagnostics are represented safely while required scientific non-finite fields still fail closed.

### Architectural consequence

Persistence is an explicit qualification gate; data cannot be silently lost or retroactively reconstructed.

---

## 4. uXRCE cross-topic head-of-line stall closed

### Observed symptom

SensorCombined delivery showed large runtime gaps during scientific attempts.

### Deep cause

Per-event diagnostics localized a same-thread stall after topic processing. Synchronous hot-path `PX4_INFO`/stdout flushing introduced head-of-line blocking in the uXRCE update path.

### Repair

High-rate synchronous diagnostics were bounded/removed from the hot path while low-overhead counters were retained.

### Qualification consequence

Subsequent source-counter qualifications showed native SensorCombined progression without the reproduced stall mechanism. Verbose Agent tracing remains prohibited in scientific runtime because it perturbs timing.

---

## 5. 183018-us apparent SensorCombined gap reclassified

### Observed symptom

A later fresh root showed a mapped SensorCombined delta of `183018 us`.

### Source-grounded reconstruction

Native PX4 source progression remained continuous:

```text
native delta = 8000 us
```

while Timesync offset changed by:

```text
-175018 us
```

which created the apparent mapped jump.

### Architectural consequence

Mapped timestamp continuity was retired as native source authority.

---

## 6. Dual-domain timestamp contract frozen

### Decision

Source continuity and clock alignment became separate predicates.

```text
SOURCE_CONTINUITY_DOMAIN = native PX4 source time + generation
CLOCK_ALIGNMENT_DOMAIN   = explicit causal mapping provenance
```

### Important rule

A mapping transition is not a native source gap.

### Scientific consequence

Cross-domain science requires both valid native source identity and valid clock alignment. Native/local diagnostics can be reasoned about separately where explicitly allowed.

---

## 7. Atomic SensorCombined provenance wire qualified

### Gap

The existing PX4/uXRCE wire did not expose enough native identity and exact sender mapping provenance for the receiver to certify a transition sample causally.

### Repair

A versioned observational message was added:

```text
SensorCombinedStampedV1
```

It carries native source identity plus the exact sender mapping tuple captured atomically with the observation. The standard SensorCombined topic remains semantically unchanged.

### Qualification

- deterministic interface/CDR checks passed;
- one live qualification session passed;
- eight live lifecycle soak sessions passed;
- source/provenance paths remained clean.

### Remaining conservative rule

Natural live Timesync transition-time science has not been accepted as a certifiable path; a transition during science still fails closed.

---

## 8. StateBank/AURA bootstrap readiness race closed

### Failure

A fresh pilot root stopped before science with:

```text
snapshot_stream_missing:aura
```

### Root cause

The session-start snapshot request was allowed before StateBank had a source-bound AURA sample.

### Repair

```text
explicit all-required-stream readiness barrier
+ atomic snapshot-barrier recheck
```

Required streams remain:

```text
aura, imu, attitude, local_state, reference, controller, actuator
```

No required stream was made optional and scientific validity was not weakened.

### Qualification

Eight fresh bootstrap-only lifecycle sessions passed with 7/7 streams and valid ACKs.

---

## 9. Specialized pilot bootstrap-only call-site closed

### Failure

The next owner-authorized root again stopped before scientific `T_D`, even though StateBank/AURA bootstrap readiness passed.

### Root cause

The specialized randomized-pilot runner invoked its `block_index=-1` session-start transaction without:

```text
bootstrap_only=True
```

so it fell into the candidate/C1 offer-frontier path.

### Repair

The specialized call site now passes `bootstrap_only=True`. A follow-on observer guard was also fixed so the intentional bootstrap `accepted_status=None` is handled as excluded pre-science state.

### Qualification

```text
RUN_TRANSACTION_CALLSITE_AUDIT=PASS
REGRESSION_TEST_RESULT=PASS_51
STATEBANK_REQUIRED_STREAMS_PRESENT=7/7
BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED
BOOTSTRAP_RETURNED_BEFORE_CANDIDATE_PATH=true
CANDIDATE_OFFER_COUNT=0
CANDIDATE_PUBLISH_COUNT=0
C1_OFFER_FRONTIER_WAIT_COUNT=0
E8_CANDIDATE_TRANSACTION_COUNT=0
FIRST_SCIENTIFIC_T_D=NOT_REACHED
MANIFEST_SLOTS_CONSUMED=0
```

### Current state

```text
READY_FOR_OWNER_FRESH_RANDOMIZED_PILOT_RETRY_REVIEW
```

---

## 10. What these closures do not prove

The infrastructure closures above do **not** prove that P1/P2 have adequate physical effect or that `G_action` is learnable at the chosen horizons.

That question requires one complete valid randomized campaign and frozen causal analysis.

Current unresolved scientific gate:

```text
8/8 valid sessions
96/96 valid blocks
-> E0/E1/session-cluster diagnostics
-> treatment SNR / carryover / FAST-candidate interaction review
-> owner causal-identification decision
```

## 11. Evidence retention policy

This GitHub summary is the human-readable audit trail. Exact raw roots, detailed reports, telemetry, counters and replay evidence remain in the project artifact hierarchy and `/media/nahhao74/KINGSTON`.

If a future issue requires forensic reconstruction, use the source registry and original artifact lineage rather than expanding this repository back into a report dump.