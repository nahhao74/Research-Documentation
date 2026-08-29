# VNEXT_WM1_AEGIS_INCREMENTAL_CANDIDATE_LIVE_MECHANISM_QUALIFICATION

**Date:** 2026-08-27
**Scope:** live control-path mechanism qualification only
**Authority:** `production_authority=false`

## Executive result

The additive path is live-qualified on the complete immutable mechanism root
`/media/nahhao74/KINGSTON/wm1_v2r1_aegis_incremental_live_mechanism_20260827_031500`.
The root contains five valid, isolated, non-scientific cases (A--E); it did not
use the randomized pilot schedule and contains zero scientific blocks. A
bounded nonzero candidate was accepted by PX4 with exact source identity and
exact pre-projection baseline-plus-candidate arithmetic. Projection and
allocator saturation were zero in every case.

The D-case result was independently re-audited from immutable E8 evidence after
an implementation-only probe-validator bug (a disjunctive identity predicate)
was corrected to an exact conjunction. Raw E8 evidence has no accepted stale
candidate or candidate publish, so the D gate is PASS. The later fresh retry
root `..._20260827_034500` failed at B with intermittent
`accepted_status_timeout`; it is preserved and is not combined with the
complete root.

**Decision:**

~~~
VNEXT_WM1_AEGIS_INCREMENTAL_CANDIDATE_LIVE_MECHANISM_QUALIFIED_READY_FOR_RANDOMIZED_PILOT_OWNER_REVIEW
~~~

This is a mechanism result, not an efficacy result and not a physical
`G_action` attribution. It authorizes no pilot execution.

## 1. Frozen boundary and inputs

The authoritative design was
`giai_doan/giai_doan_vnext/nhiem_vnext.wm1_aegis_incremental_candidate_control_path_design_and_shadow_qualification.md`.
Its closed semantics are retained:

~~~
CURRENT_REPLACEMENT_PATH = CONFIRMED       (historical ARM_B path)
TARGET_INCREMENTAL_SEMANTICS = CLOSED
FAST_T1_BASELINE = RETAIN_ACTIVE
H1000_V2 = RETAIN
RANDOMIZATION_MANIFEST = RETAIN_UNCHANGED
~~~

The live qualification did not run the scientific schedule. The frozen pilot
manifest remains byte-identical:

~~~
giai_doan/giai_doan_vnext/wm1_v2r1_within_run_randomized_pilot_schedule_v1.json
SHA256 = 253b847d77e8675ce21d8199c17832c5f50a62c3738201cb0cf8339343d90bf7
~~~

No Q5/SEALED payload, target, feature, inference, or evaluation was opened.

## 2. Source-ground control path

The old matched-candidate ARM_B behavior replaced the C1 baseline. The new path
keeps ARM_C and composes in the correction domain:

~~~
AURA d_fast
  -> p_fast_ne = -d_fast_ne
  -> continuous C1 bridge (original bridge_n/e)
  -> E8AppliedNode._c1_callback
  -> exact session/reset/source-frontier offer match
  -> compose_incremental_candidate()
  -> ARM_C AegisAccelerationCorrection ingress
  -> PX4 PositionControl::_velocityControl()
  -> unchanged _accelerationControl() / tilt / thrust / allocation
  -> AegisAccelerationCorrectionStatus
  -> StateBank candidate-aware observation ledger
~~~

### Actual fields

| Quantity | Exact field/object | Contract meaning |
|---|---|---|
| `u_fast` | E8 `p_fast_ne`, status `p_fast_ne` | AURA FAST correction. |
| original `u_t1_or_c1` | C1 `bridge_n/e`, E8 `bridge` before composition | Continuous C1 baseline. |
| `u_baseline_requested` | E8 `publish_baseline_requested_ne`; StateBank `baseline_requested_correction_ned` | `p_fast + original C1 bridge`. |
| `u_candidate_requested` | Stage-1 offer `action_n/e`, `planned_action_id`, `explicit_zero`; StateBank `scientific_candidate_requested_correction_ned` | Pre-execution candidate plan with identity. |
| `u_total_requested` | PX4 status `requested_correction_ned` | Composed pre-projection request. |
| `u_baseline_applied` | StateBank `baseline_accepted_correction_ned` | Total minus candidate only for an exact valid offer; otherwise total. |
| `u_candidate_applied` | StateBank `scientific_candidate_accepted_correction_ned` | Candidate contribution only for an exact valid offer; otherwise zero. |
| `u_total_accepted` | PX4 status `accepted_correction_ned` | Authoritative native pre-projection accepted correction. |
| identity/gates | `generation`, `controller_session_start_us`, `reset_generation`, `source_frontier_us`, source/gate/freshness/authority fields | Exact causal provenance and fail-closed validity. |

### Ordering and composition

~~~
candidate parse/finite/identity/bound checks
  -> baseline source/freshness/C1/schema/policy checks
  -> baseline + candidate requested correction
  -> PX4 native validity/authority gate
  -> unchanged acceleration-to-thrust/tilt/allocation projection
~~~

The candidate envelope is independently bounded by
`validate_candidate_vector()`:

~~~
component limit = 0.012 m/s²
horizontal norm limit = 0.012 m/s²
bound epsilon = 1e-9 m/s²
explicit-zero tolerance = 1e-6 m/s²
~~~

No adaptive scaling, candidate clipping, FAST/T1 change, PX4 control-law
change, or authority increase was introduced. The tested C vector is
`[0.008, 0.0] m/s²`, inside the existing envelope.

The correction-domain identity is exact:

~~~
u_total_requested = u_baseline_requested + u_candidate_requested
~~~

For an exact valid offer, StateBank derives the corresponding pre-projection
accepted decomposition from the candidate offer and PX4 total status. This does
not extend additivity through PX4 `_accelerationControl()`, tilt/thrust
projection, allocator saturation, feedback, or plant evolution.

### Bridge-field compatibility audit

The native message has no separate candidate field. On a valid candidate cycle
E8 intentionally publishes `original_C1_bridge + candidate` in `bridge_n/e`,
while the E8 evidence and StateBank ledger retain separate baseline/candidate/
total vectors.

All consumers were checked:

* `e8_applied_gate.py` interprets `bridge_ne` as the ARM_C bridge presented
  to PX4 and validates `p_fast + bridge`; this is correct for the composed
  field, not a pure-C1 assumption.
* `e7r2r5_schema.py`, `c1_t1_runtime_characterization.py`, and
  `e7r1_c1_parity.py` consume the `/continuous_c1_shadow` diagnostic, whose
  `bridge` remains the original C1 bridge and is not the E8 ingress field.
* `c1_t1_vs_t2_d1_applied_evaluation.py` and `fast_path_latency_r1.py` read
  the status bridge as an ingress correction for observation-only diagnostics;
  they do not authorize control or overwrite the candidate ledger. Candidate
  cycles are interpreted through E8 `composition_version` and candidate
  state, not as scientific C1-only evidence.
* `statebank_v1_node.py` has explicit candidate-aware fields and computes
  baseline/candidate/total vectors from the exact offer/status identity.

Therefore the field meaning is explicit at each boundary and no validator or
control consumer silently treats a candidate `bridge_ne` as pure C1.

## 3. Immutable live root and preflight

Selected complete root:

~~~
/media/nahhao74/KINGSTON/wm1_v2r1_aegis_incremental_live_mechanism_20260827_031500
~~~

Preflight recorded:

~~~
pass = true
free_before_bytes = 108911132672  (>= 50 GiB)
free_after_bytes  = 105366945792
root_bytes        = 3514931946
scientific_schedule_used = false
scientific_blocks = 0
residual_processes = []
~~~

Preflight/runtime identities:

| Identity | Value |
|---|---|
| PX4 base commit | `85df8c2281c2466b30a121b22b0bf33dc69bcfe4` |
| native stimulus SHA256 | `eab30a1868a049109ea304ef02198d585c701174efd0111f3b30378b981c4915` |
| DDS topics header | `13b9195087e1ebef7cda48f72a793d4794f31c405b26ac2bc8d305587f83f5e0` |
| DDS topics YAML | `86cc86860bfac7e98156906519f7cb41ad3e584d9ba8c46f3456d5e667385f72` |
| `multicopter_position_control` | `1e08f4570b5011a6368675b51ee4676121a4f7e8772b06788f7b1452839583e6` |
| PX4 executable | `b228ae891544beb6f3ef5cccdf6505c5d2ddfd0b1d39f845cd1c2555f8a1fabc` |

Evidence file hashes:

~~~
preflight.json
  84546f9d87954d92ba0f34e9034ac3e80ffa11d2a40baa2c862f1922afd1158c
mechanism_results.json
  aa846297c8d8fd9b1fc047b2bf62a7e97db5c21b1515be3a73b02645e93dd463
manifests/mechanism_cases.json
  08886dfd7b9c78339eeb91fa6a094ab3835b877bf0802ce95fd08c9549aac7a5
~~~

Current audited source hashes (including the implementation-preserving
validator/retry repair):

~~~
3_AEGIS/aegis/e8_applied_node.py
  28d5889feb34bf16b997f86f01e76c88b37df9174d9b7fc13ddedf6fb7d1d32e
3_AEGIS/aegis/e8_incremental_candidate_path.py
  0f5ad2980f954b7dee31223ce88efdfa4386085f1f4fd68af5bbe69e90bb0bfb
1_AURA/aura/statebank_v1.py
  04834d13f145a523d4186b5c1f637d77e7a95a66433088bbbfc364fed45a57c1
1_AURA/aura_data_acquisition/wm1_stage1_probe.py
  8d650f61cbc9d4f092074f68a19542a2c0bdf3f169167f385117d70dee03675c
~~~

## 4. Live mechanism matrix

The root ran the five required isolated CALM / worker-A mechanism cases. A case
status is runtime validity, not a scientific treatment result.

| Case | Offer | Status evidence | Result | Key counts |
|---|---|---|---|---|
| A `NO_CANDIDATE_BASELINE` | none | ARM_C baseline; no candidate offer | **VALID** | 4,162 status rows; 1,154 valid baseline; 0 candidate publishes |
| B `EXPLICIT_ZERO_CANDIDATE` | `[0, 0]`, `explicit_zero=true` | candidate zero applied; total equals baseline | **VALID** | 4,397 status rows; 1,326 valid baseline; 3 repeated E8 evidence publishes; 1 accepted offer |
| C `BOUNDED_NONZERO_CANDIDATE` | `[0.008, 0]` m/s², one minimum mechanism cycle | candidate accepted on ARM_C; total equals baseline plus candidate | **VALID** | 4,117 status rows; 1,178 valid baseline; 3 repeated E8 evidence publishes; 1 accepted offer |
| D `STALE_INVALID_CANDIDATE` | stale source identity | no candidate publish/identity acceptance; baseline continues | **VALID** | 4,530 status rows; 1,100 valid baseline; 0 candidate publishes |
| E `RESET_GENERATION_CHANGE` | reset-mismatched `[0.008, 0]` | no candidate publish/identity acceptance across reset mismatch | **VALID** | 4,393 status rows; 1,449 valid baseline; 0 candidate publishes |

All five trace validators reported no trace errors, no writer drops, and no
projection invalid rows. The `accepted_nonzero_status_count` in the table is
the total FAST/T1/C1 correction and is intentionally not a scientific
candidate count.

### B: explicit ZERO proof

The accepted offer identity was:

~~~
plan_id       = WM1_STAGE1:WM1_INCREMENTAL_MECHANISM_B_EXPLICIT_ZERO_CANDIDATE:b00:z:c00
generation    = 900050
source        = 18012000 px4_boot_us
session       = 248000
reset         = 0
~~~

Representative E8 evidence contains:

~~~
baseline = [-0.000030062147575502245, 0.0012892949160447163]
candidate_applied = [0.0, 0.0]
total    = [-0.000030062147575502245, 0.0012892949160447163]
candidate_state = CANDIDATE_ZERO_APPLIED
~~~

PX4 accepted the same nonzero baseline total for the exact offer; the accepted
status was
`[-0.000030062103178352118, 0.0012892949162051082, 0]` with the same
generation/source/session/reset identity. This proves candidate ZERO does not
suppress or switch the active FAST/T1/C1 baseline.

### C: bounded incremental proof

The accepted offer identity was:

~~~
plan_id       = WM1_STAGE1:WM1_INCREMENTAL_MECHANISM_C_BOUNDED_NONZERO_CANDIDATE:b00:a:c00
action        = [0.008, 0.0] m/s²
generation    = 900000
source        = 16920000 px4_boot_us
session       = 256000
reset         = 0
arm           = ARM_PX4_PLUS_FAST_PLUS_C1 (2)
~~~

Two representative E8 observations (repeated callback evidence for one offer)
are:

~~~
baseline  = [ 0.0002871334085617212, 0.003817192119525745]
candidate = [ 0.008,                  0.0               ]
total     = [ 0.008287133408561721, 0.003817192119525745]

baseline  = [-0.002303698600808892, 0.00393371794923211]
candidate = [ 0.008,                  0.0              ]
total     = [ 0.005696301399191108, 0.00393371794923211]
~~~

The PX4 accepted status for that exact identity was:

~~~
requested_correction_ned = [0.005696301348507404, 0.003933717962354422, 0.0]
accepted_correction_ned  = [0.005696301348507404, 0.003933717962354422, 0.0]
~~~

The status is the total pre-projection correction; the approximately `5e-11`
float difference from the JSON evidence is serialization rounding. No ARM_B
replacement occurred.

### D: stale fail-to-baseline proof

The stale offer was:

~~~
offer:  generation=910000, source=20848000, session=260000, reset=1
target: current source=19848000, session=260000, reset=1
~~~

The immutable E8 evidence has 200 match-evaluation records with
`offer_identity_match=false`, `offer_applied=false`, and zero candidate
publish events. Valid ARM_C baseline statuses continue, including a sample
accepted correction
`[-0.00010740780271589756, 0.0016633303603157401, 0]`.

The original probe summary incorrectly used a disjunctive identity predicate and
reported `candidate_applied=true`. That was a validator-only false positive;
the implementation was repaired to require generation **and**
source-frontier **and** session **and** reset equality. No raw runtime evidence
was changed.

### E: reset no-stale-reuse proof

The reset-mismatched offer was:

~~~
offer:  generation=920000, source=17668000, session=268000, reset=1
target: current source=17668000, session=268000, reset=0
~~~

The E8/StateBank evidence has zero candidate identity status and zero candidate
publish events, while 46 valid baseline statuses continue. Thus an old-reset
candidate cannot reappear on the current reset generation.

## 5. H1000 V2 and provenance regression

`H1000_SCIENTIFIC_CANDIDATE_REFRACTORY_V2` remains unchanged. The source
implementation classifies status as:

~~~
ARM_C with no exact offer -> FAST_T1_C1_BASELINE,
  scientific_candidate_accepted_correction_ned = [0,0,0]

exact valid candidate offer -> SCIENTIFIC_CANDIDATE,
  candidate vector bound to plan/session/reset/generation/source frontier
~~~

Candidate history is separate from total accepted correction:

* baseline FAST/T1/C1 nonzero corrections remain causal pre-treatment context;
* explicit candidate ZERO is candidate history zero even when total status is
  nonzero;
* a valid nonzero candidate is recorded as scientific candidate history;
* rejected, stale, or reset-mismatched offers contribute zero and do not block
  the baseline;
* accepted offers are retired by exact PX4 status identity; reset/session
  changes retire pending offers.

The mechanism root was not a randomized scientific block and did not attempt a
full H1000 treatment experiment. The PASS below is the semantic/ledger
regression check, supported by the live B--E identity cases and existing
candidate-history unit tests, not an efficacy claim.

## 6. Projection, safety, and FAST interaction

All cases retained the native PX4 path. Per-case counts from the complete root
were:

~~~
projection_invalid_count = 0 for A/B/C/D/E
allocator_saturation_rows = 0 for A/B/C/D/E
candidate vector tested = [0.008, 0.0] m/s² <= [0.012 norm/component] envelope
~~~

No candidate envelope, total feasibility, tilt, thrust, allocator, or native
projection limit activated in the mechanism evidence. There is no observed
immediate FAST counter-reaction, cancellation, amplification, or oscillation
pathology in the captured trace. This small mechanism run is not sufficient to
call the interaction stable or to identify a physical action effect; no
FAST/T1 or candidate tuning was performed.

The physical response attribution remains nonlinear because PX4 performs
acceleration-to-attitude/thrust projection and the closed-loop controller and
plant respond afterward. A randomized experiment is required for that
attribution.

## 7. Implementation-preserving repairs and checks

Only plumbing/qualification fixes were made; scientific/control semantics were
not changed:

1. explicit-zero validation checks the candidate contribution, not the total
   accepted FAST/T1/C1 correction;
2. E8 holds an already-published offer through the bounded source retry window
   when C1 temporarily re-enters, while still retiring it on identity/reset
   mismatch;
3. the probe D/E identity audit uses the exact four-field conjunction;
4. the live C case is the minimum one-cycle nonzero mechanism vector; the
   scientific 5C schedule is not run here, and release/reset behavior is
   covered independently by B/D/E.

Focused checks after the repair:

~~~
source scripts/runtime_env.sh && pytest -q \
  3_AEGIS/tests/test_e8_incremental_candidate_path.py \
  3_AEGIS/tests/test_e8_native_insertion_contract.py \
  1_AURA/tests/test_wm1_stage1_acquisition.py \
  1_AURA/tests/test_wm1_v2_scientific_action_history.py

24 passed in 0.27s
~~~

## 8. Preserved failed/stopped roots

No failed root was overwritten or combined with the complete `031500` root.
All remain under Kingston. The failed/stopped inventory is:

| Root group | Result |
|---|---|
| `20260826_231542`, `20260827_020000` | stopped / no final result artifact; not used |
| `20260826_231800`, `232200`, `233500`, `234500`, `235500`, `235900`, `20260827_001000`, `003500`, `004500`, `010000`, `011500`, `013000` | stopped at B (accepted-status/infra failure); `003500` was a callback KeyError and `004500` a probe NameError |
| `20260827_001500` | B `explicit_zero_not_accepted` |
| `20260827_002500` | A callback `TypeError: _int() takes from 1 to 2 positional arguments but 3 were given` |
| `20260827_021500`, `20260827_024000` | stopped at C (`accepted_status_timeout`; `024000` also retained the earlier release-probe issue) |
| `20260827_034500` | fresh post-validator-repair retry stopped at B (`accepted_status_timeout`) |

These infrastructure roots contain no scientific randomized blocks and no
nonzero scientific treatment. They are provenance for repair effort only.

## 9. Required verdicts

~~~
BRIDGE_FIELD_SEMANTIC_COMPATIBILITY = PASS
CORRECTION_DOMAIN_ATTRIBUTION = EXACT
PHYSICAL_EFFECT_ATTRIBUTION = NONLINEAR_RANDOMIZED_EXPERIMENT_REQUIRED

NO_CANDIDATE_LIVE_PARITY = PASS
ZERO_CANDIDATE_LIVE_PARITY = PASS
NONZERO_INCREMENTAL_LIVE_PATH = PASS
STALE_FAIL_TO_BASELINE_LIVE = PASS
RESET_NO_STALE_REUSE_LIVE = PASS
H1000_V2_LIVE_REGRESSION = PASS

CANDIDATE_CONSTRAINT_ACTIVITY = NONE
FAST_CANDIDATE_IMMEDIATE_INTERACTION = NO_PATHOLOGY_OBSERVED

FAST_T1_BASELINE = RETAIN_ACTIVE
OPERATING_RADIUS_35M = RETAIN
RANDOMIZATION_MANIFEST = RETAIN_UNCHANGED
PILOT_RETRY = NOT_AUTHORIZED
MODEL_TRAINING = NOT_AUTHORIZED
SEALED_PAYLOAD_OPENED = false
production_authority=false
~~~

## 10. Boundary and next action

The live additive mechanism is qualified for owner review before any scientific
randomized pilot retry. No randomized pilot, P1/P2 scientific campaign,
training, SEALED evaluation, dynamic-reference acquisition, FAST/T1 change,
authority increase, or 35 m change occurred.

Next boundary: owner review of the live mechanism evidence and explicit
authorization of a fresh frozen randomized pilot, if desired.
