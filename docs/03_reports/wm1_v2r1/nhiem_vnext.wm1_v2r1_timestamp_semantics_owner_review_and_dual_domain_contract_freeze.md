# VNEXT WM1 V2R1 — timestamp semantics owner review and dual-domain contract

Date: 2026-08-28  
Scope: semantic design/owner decision only. No runtime, build, pilot,
training, R1, SEALED access or implementation patch was performed.

```text
RECOMMENDATION=RECOMMEND_DUAL_DOMAIN_NATIVE_SOURCE_CONTINUITY_PLUS_SEPARATE_CLOCK_ALIGNMENT
IMPLEMENTATION_AUTHORITY=NOT_AUTHORIZED
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
production_authority=false
```

## CURRENT_TIMESTAMP_CONTRACT

The current local runtime has one operational source-gap predicate. In
`moving_runtime_node.py`, the SensorCombined callback selects
`_sensor_timestamp_us(message)` (`timestamp_sample` when present, otherwise
`SensorCombined.timestamp`), calls
`CausalTimesyncTimestampMapper.canonicalize(raw, callback_monotonic_ns)`, and
passes the returned value to `CausalSourceTimestampGuard.gap_exceeds(...,
20_000)`. The mapper uses the latest already-received nonzero Timesync offset;
raw values below `AGENT_EPOCH_FLOOR_US` remain native PX4 values, while
epoch-sized values are mapped by `raw + offset`.

The existing source contract separately states that the physical accelerometer
time is `timestamp + accelerometer_timestamp_relative` in PX4 boot
microseconds. The existing clock/reset contract also separates
`ACTION_STATE_IDENTITY` from `CLOCK_MAPPING_IDENTITY` and retains raw wire and
canonical fields. The contradiction is that the live AURA gap guard currently
operates on the canonical mapped value rather than on the native source
progression. Thus a sender-side offset transition can be reported as a source
gap before the receiver has the corresponding offset provenance.

```text
CURRENT_TIMESTAMP_CONTRACT=
single mapped PX4-boot-compatible frontier from prior-only Timesync offset;
one 20,000-us guard is used for both source continuity and mapping changes
```

This review does not change that contract in code and does not recertify the
failed root.

```text
SOURCE_CONTINUITY_DEFINITION=NATIVE_PX4_SOURCE_TIME_PLUS_PUBLICATION_GENERATION
CLOCK_ALIGNMENT_DEFINITION=CAUSAL_CROSS_DOMAIN_MAPPING_WITH_EPOCH_PROVENANCE_AND_UNCERTAINTY
SOURCE_CONTINUITY_DOMAIN=NATIVE_PX4_SOURCE_TIME_AND_GENERATION
CLOCK_ALIGNMENT_DOMAIN=CAUSAL_CROSS_DOMAIN_MAPPING_WITH_EXPLICIT_EPOCH_PROVENANCE
```

## ROOT_CAUSE_EVIDENCE

Authoritative evidence is
`giai_doan/giai_doan_vnext/nhiem_vnext.wm1_v2r1_canonical_sensorcombined_timestamp_mapping_discontinuity_root_cause_closure.md`.
Its immutable scientific root is:

```text
ROOT_VALIDITY_CLASS=INVALID_SCIENTIFIC_CONTRACT
ROOT=/media/nahhao74/KINGSTON/wm1_v2r1_within_run_randomized_action_20260828_uxrce_logging_hol_retry_01
INCIDENT_PAIR_NATIVE=32420000 -> 32428000 us
INCIDENT_PAIR_WIRE=1787910190946130 -> 1787910191129148 us
RUNTIME_CANONICAL=32420000 -> 32603018 us
RAW_GENERATION_CONTINUITY=PASS_NATIVE_PX4
RAW_SENSORCOMBINED_MAX_DELTA_US=8000
RAW_SENSORCOMBINED_GAP_GT_20MS=0
TIMESYNC_OFFSET_BEFORE=-1787910158526130 us
TIMESYNC_OFFSET_AFTER=-1787910158701148 us
TIMESYNC_OFFSET_STEP=-175018 us
HOST_RECEIPT_DELTA_US=8050.009 ROS; 8372.364 AURA
```

The exact arithmetic is:

```text
1787910190946130 - 1787910158526130 = 32420000
1787910191129148 - 1787910158526130 = 32603018
1787910191129148 - 1787910158701148 = 32428000
183018 + (-175018) = 8000
```

PX4 `SC_PUBLISH` and uXRCE `POLLIN/COPY/PREPARE/SEND/FLUSH` are present at the
pair; no producer, poll, transport or ROS loss is proven. The AURA
`physical_residual_unqualified`/reprime and C1/E8 failures are downstream
fail-closed responses. The old scientific root remains immutable and is not
retrospectively certified or pooled.

## SOURCE_CONTINUITY_DEFINITION

```text
SOURCE_CONTINUITY_DOMAIN=NATIVE_PX4_SOURCE_TIME_AND_GENERATION
```

`SOURCE_CONTINUITY_VALID` is true only when, within one source session and
reset/frame lineage:

1. the native SensorCombined source time is
   `timestamp + accelerometer_timestamp_relative` in `px4_boot_us`;
2. the publication-derived generation is causally monotonic (a documented
   subscriber decimation is not itself a missing-source claim);
3. source time is strictly forward for accepted observations, with no
   duplicate/reversed native identity and no native delta greater than the
   unchanged `20,000 us` gate; and
4. the required source/reset/calibration identity is valid.

The native generation and timestamp are the quantities used for this
predicate. A serialized agent-epoch representation, a Timesync offset, or a
host receipt interval cannot make this predicate false or true. In particular:

```text
TIMESYNC_OFFSET_CHANGE_MUST_NOT_BE_INTERPRETED_AS_NATIVE_SOURCE_LOSS=true
```

The threshold remains exactly `20,000 us`; this review does not add a grace
period, interpolation, fill, or a new source-rate assumption.

## CLOCK_ALIGNMENT_DEFINITION

```text
CLOCK_ALIGNMENT_DOMAIN=CAUSAL_CROSS_DOMAIN_MAPPING_WITH_EXPLICIT_EPOCH_PROVENANCE
```

`CLOCK_ALIGNMENT_VALID` is a separate predicate. It is true only when a
cross-process observation has:

- a declared raw representation/domain (`px4_boot_us` or `agent_epoch_us`);
- matching source protocol, controller session, mapping version and mapping
  epoch;
- an offset/provenance observation causally available at or before the sample,
  or an exact per-message sender provenance equivalent;
- uncertainty within the already/future owner-frozen clock bound (no numeric
  bound is invented here); and
- no mixed representation, same-epoch rollback, or unresolved epoch
  transition.

It is therefore valid to represent:

```text
SOURCE_CONTINUITY_VALID=true
CLOCK_ALIGNMENT_VALID=false
```

That state means the source exists and progresses, but a cross-domain join or
mapped timing claim is not certifiable yet. It is not a source-loss event.

## TIMESYNC_MAPPING_EPOCH_CONTRACT

```text
TIMESYNC_MAPPING_EPOCH_CONTRACT=EXPLICIT_SESSION_PROTOCOL_MAPPING_VERSION_SENDER_GENERATION_AND_REPRESENTATION_IDENTITY
```

The proposed explicit mapping identity is:

```text
TIMESYNC_MAPPING_EPOCH=
controller_session_start_us + source_protocol + mapping_version +
sender_mapping_generation + raw_representation
```

The currently recorded `clock_epoch_id` already contains session, protocol and
mapping version. The missing `sender_mapping_generation` (or an exact
source-equivalent provenance token) is a semantic requirement for future
cross-domain certification, not an implementation silently assumed by this
review.

Epoch rules to freeze after owner approval:

1. A controller-session, source-protocol or mapping-version change always
   starts a new mapping epoch. AEGIS `reset_generation` remains a separate
   action/state identity and does not by itself reset the clock epoch.
2. A representation transition (native-sized to agent-epoch-sized or back), or
   an offset update that changes the applicable mapping relation, advances the
   mapping epoch/provenance. The transition sample retains raw value, mapping
   identity and offset provenance and is labelled
   `CLOCK_MAPPING_EPOCH_TRANSITION`.
3. A mapped delta across two incompatible epochs is never compared as a native
   source delta. The native generation/time ledger may continue independently.
4. A mapping observation must be prior to the sample; future Timesync samples,
   interpolation, forward fill and retrospective smoothing are forbidden.
5. Cross-domain joins remain unavailable until the new epoch and its causal
   provenance are known. No fixed waiting interval is invented. A real
   same-epoch rollback is `CLOCK_MAPPING_NONMONOTONIC`; it is not repaired by
   relabelling as a source gap.
6. Old mapping state cannot cross a new session or mapping version. Raw source
   values are immutable evidence; mapped values are derived projections.

This rule makes an offset transition observable without allowing it to poison
the native source-continuity predicate.

## AURA_FAST_PATH_REQUIREMENTS

```text
AURA_FAST_PATH_REQUIREMENTS=NATIVE_SOURCE_CONTINUITY_LOCAL_RECEIPT_FRESHNESS_RESET_FRAME_CALIBRATION_AND_SAFETY
```

Under the recommended contract, the AURA fast path requires native source
continuity for each mandatory local source, local receipt freshness, the
existing reset/frame/calibration checks, finite values and existing safety
conditions. A mapping-epoch change alone is not a native source-loss reason.

For the explicit state `SOURCE_CONTINUITY_VALID=true` and
`CLOCK_ALIGNMENT_VALID=false`:

- AURA may continue collecting the native source ledger and computing a
  native-only diagnostic where all required inputs share a valid native source
  identity. It must not mark a cross-domain joined disturbance as valid merely
  because a mapped number is available.
- If the disturbance calculation needs a cross-domain join whose epoch is not
  certifiable, its deployable/scientific output is invalid or masked with an
  explicit clock reason; no synthetic timestamp, interpolation or future
  sample is allowed.
- Whether that native-only result is allowed to drive the existing C1/FAST
  control output is a control-validity decision, not a consequence of the
  timestamp arithmetic. The recommended policy is to retain the existing
  native/local safety gates and reject only the uncertified cross-domain
  candidate path.

The last bullet changes the behavior of the current implementation at an
offset transition (the current path reprimes on the mapped gap). Therefore it
is presented for owner approval, not silently applied.

```text
SOURCE_CONTINUITY_VALID=true_AND_CLOCK_ALIGNMENT_VALID=false:
AURA_DISTURBANCE_ESTIMATION=continue_native_only_or_diagnostic;do_not_certify_cross_domain_join
C1_FAST_BASELINE=continue_only_if_existing_native_local_source_and_safety_gates_pass
CANDIDATE_SCIENTIFIC_CERTIFICATION=STOP_UNTIL_CLOCK_ALIGNMENT_VALID
STATEBANK_WM_CAUSAL_BINDING=STOP_OR_MASK_UNTIL_MAPPING_EPOCH_PROVEN
```

## C1_FAST_REQUIREMENTS

```text
C1_FAST_REQUIREMENTS=EXISTING_NATIVE_LOCAL_SOURCE_AND_AUTHORITY_GATES_WITH_SEPARATE_CLOCK_REASON
```

```text
C1_FAST_REQUIREMENTS=
existing native/local source validity, receipt freshness, finite bridge and
existing authority/safety predicates; mapping epoch transition is a distinct
reason and is not renamed NATIVE_SOURCE_GAP
```

FAST/T1/C1 values, controller law and candidate envelope remain frozen. The
recommended separation permits the baseline to remain active when its native
and local gates pass, while a candidate or bridge that requires an
uncertified cross-domain join remains fail-closed. If the owner instead wants
the current `AURA.valid=false` behavior during every mapping transition, that
is a different, explicitly frozen policy; it is not inferred here.

## SCIENTIFIC_BINDING_REQUIREMENTS

```text
SCIENTIFIC_BINDING_REQUIREMENTS=SOURCE_CONTINUITY_VALID_AND_CLOCK_ALIGNMENT_VALID_AND_CAUSAL_IDENTITY_VALID
```

Scientific certification requires both predicates:

```text
SCIENTIFIC_BINDING_VALID=
SOURCE_CONTINUITY_VALID
AND CLOCK_ALIGNMENT_VALID
AND session/reset/clock/frame/tau identity valid
AND causal T_D/T_A/X_t binding valid
```

If alignment is false, the candidate record is not certified and no treatment
exposure, target or contrast is reconstructed post hoc. A source-continuous
interval may be retained as diagnostic evidence, but it cannot silently pass
the frozen timing gate.

## STATEBANK_WM_REQUIREMENTS

```text
STATEBANK_WM_REQUIREMENTS=NATIVE_CANDIDATE_HISTORY_RELEASE_H1000_IDENTITY_PLUS_VALID_CROSS_DOMAIN_EPOCH_FOR_JOINS
```

StateBank candidate history, release and H1000 use native source time,
candidate generation, session/reset identity and accepted-action identity.
The frozen `1,000,000 us` H1000 duration is not changed. Cross-domain StateBank
or World Model joins additionally require a valid mapping epoch/provenance;
otherwise they remain unavailable/fail-closed. No future observation can enter
`T_D`, `T_A`, `X_t`, H1000 or a target.

## T_D_TIME_DOMAIN

```text
T_D_TIME_DOMAIN=
native_px4_boot_us_source_frontier + causal host-receipt provenance;
mapped projection is permitted only when CLOCK_ALIGNMENT_VALID=true
```

The native `source_frontier_us`/generation is the decision identity. The host
receipt timestamp and any mapped value are auxiliary alignment evidence, not a
replacement for the native frontier.

## T_A_TIME_DOMAIN

```text
T_A_TIME_DOMAIN=
native_px4_boot_us accepted-status source_frontier_us/generation;
raw status timestamp and host receipt are retained separately
```

The accepted PX4 status identity remains the source of `T_A`. Any tau or
cross-process latency calculation may use a mapped projection only within one
valid mapping epoch and only with prior causal observations. Existing reset
scoping of action history is unchanged.

## TARGET_TIME_DOMAIN

```text
TARGET_TIME_DOMAIN=
native_px4_boot_us T_A plus the frozen H40/H80 horizon; host/mapped time is
alignment provenance only
```

The first valid future local-state sample at/after `T_A+h`, unchanged reset
lineage, and no interpolation/fill remain the frozen target rules. `G_action`
therefore uses native source/exposure identity and requires clock alignment for
any cross-process join; it does not use a future Timesync sample to repair a
past record.

## OPTION_A_ASSESSMENT

```text
OPTION_A=DUAL_DOMAIN_NATIVE_SOURCE_CONTINUITY_PLUS_SEPARATE_CLOCK_ALIGNMENT
```

This is the recommended semantic model. It matches the already frozen AURA
source contract and the existing identity separation: native generation/time
answers “did the source progress?”, while epoch/provenance answers “can the
observation be joined across clocks?”. It has low steady-state latency and
does not change native PX4/ROS message meaning, but it requires a versioned
AURA/StateBank record contract carrying both predicates and epoch provenance.
For exact cross-domain joins, its implementation may still need the
per-message sender provenance described by Option B. It is the smallest model
that prevents a clock transition from masquerading as physical source loss,
while remaining fail-closed for science.

```text
causal_correctness=highest separation of physical and clock claims
latency=low after provenance is available; transition joins temporarily masked
failure_modes=explicit source vs clock reasons; no cross-epoch comparison
complexity=moderate dual fields/epoch ledger
interface=native PX4 message unchanged; versioned AURA/record provenance added
WM_TD_TA=compatible when native frontiers remain authoritative
FAST_PATH=can remain native/local, subject to owner control-validity approval
science=conservative; requires both predicates
replay=deterministic from native ledger plus epoch/provenance
```

## OPTION_B_ASSESSMENT

```text
OPTION_B=ATOMIC_PER_MESSAGE_TIMESYNC_PROVENANCE
```

Each serialized source sample carries or is exactly joined to the sender
offset/mapping generation used for serialization. The receiver can reconstruct
the native source value without the asynchronous stale-offset race. This
preserves the current mapped-frontier shape more closely than Option A, but it
does not independently prove native generation continuity unless the native
generation ledger is also retained. It requires a versioned provenance
interface (or a lossless topic-bound side ledger) and exact sender/receiver
identity accounting.

```text
causal_correctness=strong for cross-domain reconstruction; native source still needs a ledger
latency=low steady state; transition metadata travels with the sample
failure_modes=missing/mismatched provenance fails closed; no silent fallback
complexity=high sender/transport/receiver provenance plumbing
interface=versioned metadata or topic-bound side channel; native payload should not be mutated casually
WM_TD_TA=compatible if native frontier fields remain separate
FAST_PATH=least change to mapped arithmetic, but exact metadata loss can close it
science=can certify both predicates when native ledger and provenance exist
replay=excellent when per-message identity is retained
```

Option B is an implementation technique that may be needed under Option A; it
is not, by itself, the semantic distinction the incident requires.

## OPTION_C_ASSESSMENT

```text
OPTION_C=SENDER_OFFSET_FREEZE_OR_REPRESENTATION_FREEZE
```

Keep one timestamp representation and one sender offset for a session, or
require a session restart before changing it. This avoids the observed mixed
representation transition with little receiver logic, but makes clock drift,
startup convergence and reconnect behavior a timing-policy problem. It can
delay or reject otherwise usable data and does not give a native source
continuity predicate. Changing the sender's offset policy or poll/session
behavior is not a mechanical observation fix.

```text
causal_correctness=adequate only while the fixed mapping remains valid
latency=low steady state; potentially long/unavailable transitions
failure_modes=stale mapping and drift; session restart can lose continuity
complexity=low receiver, high operational/timing policy risk
interface=small, but runtime/session semantics change
WM_TD_TA=requires explicit session/epoch boundaries
FAST_PATH=may unnecessarily stop on mapping maintenance
science=fail-closed but less data-efficient
replay=deterministic within a fixed session, weak across transitions
```

Option C is rejected as the default because it hides the distinction by
constraining runtime timing rather than expressing the two claims.

## FAILED_EVENT_COUNTERFACTUAL_TABLE

The exact failed event is replayed conceptually from the retained values:
native `32420000→32428000`, raw wire
`1787910190946130→1787910191129148`, old offset, then a `-175018 us` offset
step. The table is a semantic counterfactual, not a promotion of the old
root.

| contract | SOURCE_CONTINUITY_VALID | CLOCK_ALIGNMENT_VALID | AURA_FAST_PATH_VALID | SCIENTIFIC_BINDING_VALID | reason at transition |
|---|---|---|---|---|---|
| current single mapped guard | `NOT_SEPARATELY_REPRESENTED` (native evidence is continuous, but mapped guard sees `183018 us`) | `NOT_REPRESENTED` | `false` in observed runtime (`reprime`, `valid=false`) | `false` | mapped source gap / reversed next sample |
| Option A dual domain | `true` (`8000 us`, native generation continues) | `false` for the transition sample without atomic sender provenance | `true` for an explicitly native/local-only control baseline; cross-domain disturbance `false` | `false` until epoch provenance is valid | `CLOCK_MAPPING_EPOCH_TRANSITION` |
| Option B per-message provenance | `true` when native generation ledger is retained | `true` for each exact-provenance sample | `true` subject to existing source/safety gates | `true` subject to existing T_D/T_A/tau gates | no false gap; provenance loss fails closed |
| Option C fixed representation | `true` | `true` only within a valid fixed mapping/session | `true` subject to existing gates | `true` only within fixed epoch/session | mapping change requires explicit new session/epoch |

Option A deliberately does not turn a not-yet-alignable sample into a
scientific record. It does, however, prevent that sample from being labelled a
native source loss. Option B is the strongest way to make the alignment bit
true at the transition, at the cost of provenance plumbing. Option C is
runtime-policy heavy.

## FAILURE_REASON_TAXONOMY

These reason codes must remain distinct in future records and reports:

| reason | meaning | native source continuity affected? | cross-domain alignment affected? |
|---|---|---:|---:|
| `NATIVE_SOURCE_GAP` | native source delta exceeds the unchanged `20,000 us` gate | yes | possibly, but independently |
| `SOURCE_GENERATION_DISCONTINUITY` | native publication/generation identity reverses, duplicates incompatibly or breaks its declared lineage | yes | possibly |
| `CLOCK_MAPPING_UNAVAILABLE` | no causally usable mapping for an epoch-like sample | no | yes |
| `CLOCK_MAPPING_EPOCH_TRANSITION` | representation/mapping provenance changed; cross-epoch delta is incomparable | no | yes, temporarily |
| `CLOCK_MAPPING_NONMONOTONIC` | mapped value rolls back inside one declared epoch | no by itself | yes |
| `CLOCK_MAPPING_UNCERTAINTY_EXCEEDED` | owner-frozen uncertainty bound is exceeded | no | yes |
| `SOURCE_RECEIPT_STALE` | local receipt/freshness requirement is stale even though source time may progress | no by itself | local readiness/joins |
| `SOURCE_DOMAIN_MISMATCH` | field is declared in the wrong timestamp domain | no conclusion about production | yes; fail closed |
| `MIXED_TIMESTAMP_REPRESENTATION` | native and epoch-like values are compared without compatible provenance | no conclusion about production | yes; fail closed |

`SOURCE_GAP` is not a catch-all label for these conditions.

## OWNER_POLICY_BOUNDARY

```text
RECOMMENDED_TIMESTAMP_CONTRACT=OPTION_A_DUAL_DOMAIN_NATIVE_SOURCE_CONTINUITY_PLUS_SEPARATE_CLOCK_ALIGNMENT
REQUIRES_CONTROL_SEMANTIC_CHANGE=true
REQUIRES_SCIENTIFIC_TIMING_SEMANTIC_CHANGE=true
REQUIRES_INTERFACE_CHANGE=true
OWNER_CONTROL_SEMANTICS_APPROVAL_REQUIRED=true
```

The three `true` flags are intentional:

- The control law, FAST/T1/C1 values, QoS and authority need not change, but
  allowing a native-only baseline to remain operational where the current
  mapped guard reprimes changes AURA/C1 control-validity behavior.
- The scientific timing contract changes from one mapped gap predicate to
  separate native continuity and clock-alignment predicates.
- AURA/StateBank/qualified records need explicit predicate and mapping-epoch
  provenance fields. Native PX4/uXRCE payload semantics need not be changed,
  but the versioned application record interface does.

No implementation is authorized until the owner accepts these implications or
selects Option B/C with its corresponding trade-offs. This report does not
change the current 20-ms rule, H1000, T_D/T_A definitions, FAST/T1/C1, P1/P2,
QoS, reference, targets, safety radius or authority.

## REQUIRED_AUTHORITY

```text
IMPLEMENTATION_AUTHORITY=NOT_AUTHORIZED
PILOT_RETRY_AUTHORITY=NOT_AUTHORIZED
SEALED_STATE=LOCKED_PRE_EVALUATION
SEALED_PAYLOAD_OPENED=false
production_authority=false
NEXT_STATE=OWNER_TIMESTAMP_CONTRACT_DECISION
RESEARCH_REQUEST=NONE
```

No runtime, non-scientific qualification, randomized pilot, training or
SEALED read was performed. The immutable incident remains
`INVALID_SCIENTIFIC_CONTRACT`; this design review neither fixes nor
retrospectively certifies it.
