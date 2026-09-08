# Current Execution Ladder — Phase-D B0 — 2026-09-08

This is the authoritative execution ladder for the current Phase-D closure program.

## State entry

```text
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_REVIEW
QUALIFICATION_ADMISSION_CONTRACT_CHANGED=true
```

## L0 — Frozen science remains unchanged

Bind and preserve:

```text
B0 = PX4 + AURA + current FAST/T1/C1
PHASE_D_B0_METRICS_V1_1
original eight scientific conditions
no performance thresholds
no adaptive changes
no retry-until-favorable
no FAST challenger
```

Current qualification-contract revision does not change B0, disturbance/reference, F0–F4 metric semantics, or scientific condition design.

## L1 — Canonical V3 readiness semantics

Retain the canonical V3 predicates/dispositions and separate:

```text
V2 start-event eligibility
TRACE_MEASUREMENT_READY
prewind admission decision
end-of-row finalization
formal D0 V3 terminal result
```

Do not replace canonical readiness with motor readiness, PID/process liveness, source counters, or TRACE_MEASUREMENT_READY alone.

## L2 — Prewind Qualification V2

The earlier prospective live population `[V2_ELIGIBLE,F0)` is superseded for prospective qualification because ordinary Phase-D cannot know exact physical F0 before command emission and cannot prove the late pre-F0 suffix is live-complete before that emission.

Current admission population:

```text
start = first canonical V2-eligible evaluation
end_exclusive = fixed source-owned checkpoint C
population = [V2_ELIGIBLE, C)
```

`C` must be prospectively frozen, source-owned, control-independent, non-adaptive, and immutable for the bound execution design.

No dynamic selection of `C` from latest-complete evidence is permitted.

## L3 — Bind exact checkpoint C

Before implementation/runtime, identify the exact canonical owner of `C`.

Preference order:

```text
1. existing canonical source-owned lifecycle frontier
2. existing deterministic evaluation-sequence frontier
3. explicitly versioned new qualification frontier
```

Do not choose a numeric offset merely to improve harness feasibility.

If no source-owned/control-independent frontier can be bound without changing scientific/control/timing semantics beyond the approved qualification revision, return to owner review.

## L4 — Interval separation

Freeze:

```text
[V2_ELIGIBLE, C) = PREWIND_ADMISSION_POPULATION
[C, physical_F0) = TRANSITION_OBSERVATION
[physical_F0, ...) = PHASE_D_SCIENTIFIC_RESPONSE
```

Physical F0 remains native Gazebo application truth.

`[C,F0)` is recorded and audited but is not retroactively inserted into the admission population.

## L5 — Prefix completeness mechanism

For every evaluation `< C`, prove before disturbance authorization:

```text
owner upper-watermark through C
writer-owned flush/accounting through C
C1 persistence/reconciliation through C
E8 persistence/reconciliation through C
source/reset/generation/session identities complete
no unresolved mandatory omission/duplicate/contradiction
```

Persistence counters that advance before actual flush are insufficient by themselves.

This prefix-completeness seal is control-inert measurement/orchestration only.

## L6 — One-shot disturbance authorization

At the existing disturbance opportunity:

```text
canonical V3 evaluation over [V2,C)
+ complete prefix seal
        ↓
PASS -> emit existing disturbance command
FAIL / UNKNOWN / incomplete / infra invalid -> no disturbance; retain root; stop row
```

Forbidden:

```text
waiting
retry-until-PASS
F0 shift
favorable-state selection
dynamic C selection
dynamic prefix shortening
window restart
20-second dwell
```

## L7 — Post-row finalization remains independent

A prewind prefix seal does not replace:

```text
trace writer finalization
complete C1 callback/persistence accounting
writer drop/error/gap accounting
E8 finalization
collector completion
lifecycle cleanup
post-row exact correspondence
strict JSON
result/report serialization
```

Do not weaken shutdown/finalization semantics to obtain an early gate PASS.

## L8 — Historical comparability audit

Historical slots 1–5 remain:

```text
UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

After `C` is concretely bound, replay historical evidence separately as:

```text
HISTORICAL_COMPARABILITY_UNDER_PREWIND_V2=PASS | FAIL | UNKNOWN
```

Do not fabricate historical live seals and do not overwrite historical statuses.

## L9 — Exact execution harness

Before science, exercise the production path through:

```text
_run_row
→ execute_row
→ trace startup
→ runtime attestation
→ checkpoint-C membership
→ owner upper-watermark
→ writer flush
→ C1/E8 reconciliation
→ prefix seal
→ gate decision
→ disturbance authorization boundary
→ collector/runtime path
→ ActuatorMotors extraction
→ postprocessor
→ strict JSON
→ validators
→ result/report serialization
→ controlled cleanup/finalization
```

Mandatory results:

```text
C_MEMBERSHIP_FIXED_BEFORE_ROW=PASS
C_CONTROL_INDEPENDENT=PASS
OWNER_UPPER_WATERMARK_THROUGH_C=PASS
WRITER_FLUSH_THROUGH_C=PASS
C1_RECONCILIATION_THROUGH_C=PASS
E8_RECONCILIATION_THROUGH_C=PASS
PREWIND_PREFIX_SEAL=PASS
FAIL_ALLOWED_DISTURBANCE_COUNT=0
UNKNOWN_ALLOWED_DISTURBANCE_COUNT=0
INCOMPLETE_PREFIX_ALLOWED_DISTURBANCE_COUNT=0
DYNAMIC_C_SELECTION_COUNT=0
DYNAMIC_PREFIX_SHORTEN_COUNT=0
FAVORABLE_STATE_RETRY_COUNT=0
DISTURBANCE_SCHEDULE_SHIFT_COUNT=0
```

Adversarial delayed-last-evaluation `< C` must abort with no disturbance.

Any production path not genuinely exercised is `RUNTIME_ONLY_UNQUALIFIED`, not PASS.

## L10 — Astra independent audit

Astra independently checks:

```text
source semantics
raw evidence
source/receipt timestamp domains
owner watermark semantics
flush/persistence semantics
reset/generation/session identities
validator logic
accounting denominators
hashes/provenance
process lifecycle
```

Luna supplies implementation/evidence; Luna does not decide scientific disposition.

## L11 — Execution binding

Only after L1–L10 pass:

- determine exactly which historical slots remain admissible under the approved comparability policy;
- freeze only the actually missing scientific conditions as new prospective attempts;
- preserve every historical invalid/unknown attempt in the ledger;
- bind metric/postprocess/C1/readiness/checkpoint-C/source identities.

Do not automatically rerun all eight rows unless evidence proves all eight require reacquisition.

## L12 — Phase-D runtime

Execute frozen rows sequentially with:

```text
STOP_ON_FIRST_INFRASTRUCTURE_INVALID=true
RETRY_UNTIL_FAVORABLE=false
ADAPTIVE_CHANGES=false
```

Every valid row requires complete prewind qualification V2, F0–F4 evidence, C1/E8 accounting, lifecycle, postprocessing, and finalization.

## L13 — Combined characterization

When all eight planned scientific conditions have contract-qualified usable bindings, produce the frozen combined characterization:

```text
latency decomposition
ONSET / SUSTAINED / CLEAR_RECOVERY
tracking error
AoI/freshness
W20/FAST behavior
control effort/headroom/saturation
recovery/censoring
STEADY vs GUST
worker/repeat consistency
```

## L14 — Bottleneck classification and owner review

Use only:

```text
AURA_DETECTION_OR_FRESHNESS
AEGIS_COMPUTE_OR_EMISSION
PX4_ACCEPTANCE
ACTUATOR_PATH
CONTROL_HEADROOM_OR_SATURATION
SUSTAINED_ESTIMATION_DECAY_OR_RECENTER
RECOVERY_CONTROL_BEHAVIOR
MULTIFACTOR
UNRESOLVED
```

`UNRESOLVED` is acceptable if complete evidence genuinely cannot discriminate the mechanism.

FAST challenger design remains a separate owner-authorized program.
