# Timing, Causality and StateBank Contract

## 1. Why timing is explicit

The current pipeline crosses several timing domains: PX4 native boot/source time, uXRCE serialized time, ROS/host receipt time and Timesync-derived mapped time. Scientific causality cannot rely on one overloaded timestamp field.

The canonical design therefore separates **source continuity** from **cross-domain clock alignment**.

## 2. Native source authority

Source continuity is defined by:

```text
native PX4 source timestamp
+ native publication/source generation
```

Current native source-gap threshold:

```text
20,000 us
```

Mapped or host time is never allowed to create a native source gap by itself.

## 3. Dual-domain timestamp model

Current observational record:

```text
WM1_DUAL_DOMAIN_TIMESTAMP_RECORD_V2
```

Current wire:

```text
SensorCombinedStampedV1
```

The wrapper preserves the standard SensorCombined topic while adding exact per-message provenance required by scientific consumers.

Conceptually each relevant record contains:

```text
native_source_timestamp_us
native_publication_generation
wire_timestamp_us
sender_mapping_generation
sender_mapping_epoch
sender_mapping_version
sender_offset_used_us
raw_timestamp_domain / representation
session/reset/source identity
host receipt provenance
clock validity / source validity reason codes
```

The sender mapping tuple is captured atomically with the serialized observation. A receiver must not guess the sender mapping by looking at a later asynchronous TimesyncStatus sample.

## 4. Canonical false-gap lesson

A previously observed mapped jump of `183018 us` was not physical producer loss. It decomposed as:

```text
native source progression = 8000 us
offset transition         = -175018 us
apparent mapped jump       = 183018 us
```

The architecture therefore enforces:

```text
Timesync mapping transition != native source loss
```

If exact mapping provenance is missing or invalid, clock alignment fails closed while native source continuity may still remain valid.

## 5. Clock transition policy

Natural live transition-time scientific certification has not yet been established as an accepted scientific path.

Current conservative policy:

```text
mapping epoch/generation transition during science
    -> CLOCK_ALIGNMENT_VALID=false
    -> SCIENTIFIC_BINDING_VALID=false
    -> stop root fail-closed
```

Forbidden repairs include:

- future Timesync lookup;
- retrospective remapping using information not available at the decision frontier;
- interpolation/smoothing across epochs;
- silently skipping the invalid sample and extending treatment;
- re-dose or post-hoc block repair.

## 6. T_D — causal decision frontier

`T_D` is the native source frontier at which a candidate decision is causally defined.

It must use already available causal state only. Host receipt and mapping provenance can certify availability/alignment, but they do not replace native source identity.

No future state, future offset or post-treatment record may be used to construct `X(T_D)`.

## 7. T_A — accepted action frontier

`T_A` is the native accepted-controller-cycle frontier for the actual candidate exposure.

This distinction is important:

```text
T_D -> decision/assignment frontier
T_A -> actual accepted treatment frontier
```

Training targets and action-conditioned outcomes are aligned to actual accepted treatment, not merely to the time an action was proposed.

## 8. Future targets

For horizon `h`, the future target is selected relative to native `T_A`:

```text
first valid local_state with native source time >= T_A + h
```

Current core horizons include H40 and H80 in the action-conditioned pilot analysis, with earlier WM work also using broader horizons such as 20/40/80 ms.

Rules:

- no interpolation;
- same session/reset lineage;
- no future leakage into decision state;
- no mapped-time replacement for native target identity.

## 9. StateBank role

StateBank is the always-warm causal memory used by WM/WISE and scientific transactions.

Required current streams:

```text
aura
imu
attitude
local_state
reference
controller
actuator
```

StateBank records source/session/reset identity and enforces snapshot causality.

## 10. Session-start readiness

A prior implementation race allowed the session-start snapshot request to occur before StateBank had received an AURA sample. The current contract is:

```text
bootstrap_ready =
    StateBank healthy
    AND every mandatory stream has current same-session source-bound presence
    AND native identity is valid
```

At the actual snapshot barrier, readiness is rechecked atomically. A stream disappearing or becoming identity-invalid cannot be hidden by an earlier readiness status.

The session-start transaction uses:

```text
block_index=-1
bootstrap_only=true
```

It returns immediately after the accepted source-complete snapshot ACK and must not enter candidate/C1/E8/scientific T_D paths.

## 11. Scientific validity vs bootstrap presence

Bootstrap readiness checks that required streams are present and source-bound. It does **not** promote a sample to scientifically valid simply because it exists.

Scientific transactions still enforce the relevant downstream predicates, including AURA validity, source continuity, clock alignment, session/reset/frame identity and target causality.

This separation prevents two errors:

1. starting a session before required streams exist;
2. weakening scientific validity merely to make startup succeed.

## 12. H1000

H1000 is exactly:

```text
1,000,000 native source microseconds
```

It is a candidate-history/refractory contract, not proof that every physical plant/controller effect has disappeared.

No interpolation, resampling or forward-fill is used to manufacture H1000 state.

Physical carryover is assessed separately using pre-treatment state and randomized outcome diagnostics.

## 13. Failure classification

Timing failures must remain separated:

```text
NATIVE_SOURCE_GAP
SOURCE_GENERATION_DISCONTINUITY
CLOCK_MAPPING_EPOCH_TRANSITION
CLOCK_ALIGNMENT_INVALID
ATOMIC_PROVENANCE_INVALID
SESSION/RESET_IDENTITY_FAILURE
STATEBANK_REQUIRED_STREAM_NOT_READY
```

This separation is critical for deciding whether a failure is transport, time mapping, bootstrap infrastructure or scientific-contract invalidation.