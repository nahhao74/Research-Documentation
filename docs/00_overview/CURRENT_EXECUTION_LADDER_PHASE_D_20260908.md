# Current Execution Ladder — Phase-D B0 — 2026-09-08

This is the authoritative execution ladder for the current Phase-D closure program. It supersedes the D0-only ladder as the active sequence; older dated ladders remain historical evidence.

## State entry

```text
D0_INFRASTRUCTURE_CLOSED=true
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
READY_FOR_PHASE_D_RUNTIME=false
EXACT_EXECUTION_HARNESS_GATE=NOT_COMPLETED
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_PREWIND_AUDIT
```

## Ladder

### L0 — Frozen science remains unchanged

Verify and bind:

```text
B0 = PX4 + AURA + current FAST/T1/C1
PHASE_D_B0_METRICS_V1_1
original eight-condition campaign
no performance thresholds
no adaptive changes
no retry-until-favorable
no challenger
```

Any material control/scientific/metric change returns to owner review.

### L1 — Canonical prewind semantic mapping

Determine from canonical V3 source:

```text
V3_PREDICATES
V3_DISPOSITIONS
V3_REQUIRED_EVIDENCE
V3_PREWIND_DECIDABLE_PREDICATES
V3_POST_ROW_FINALIZATION_OBLIGATIONS
V3_FORMAL_FINAL_PASS_REQUIREMENTS
```

Do not equate V2 eligibility, `TRACE_MEASUREMENT_READY`, motor readiness, or process liveness with V3 PASS unless the exact semantics prove it.

### L2 — Fixed prewind population

Prospective owner-approved population:

```text
start = first canonical V3 evaluation satisfying V3_RUNTIME_START_EVENT_V2
end_exclusive = frozen scheduled native F0 source frontier
population = [start, F0)
```

No 20-second dwell requirement.

No dynamic shortening to the latest fully closed prefix.

### L3 — Historical predicate audit

For slots 1–5, evaluate each applicable frozen predicate from retained raw pre-F0 evidence.

Per slot classify exactly one:

```text
PASS_FROZEN_PREWIND_OBLIGATIONS_FROM_RAW_EVIDENCE
FAIL_FROZEN_PREWIND_OBLIGATION
UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
```

Do not fabricate a historical marker or retroactive live prefix seal.

Do not change scientific accounting until Astra independently audits the predicate results.

### L4 — Prefix-completeness mechanism

If existing canonical semantics support the mapping, implement only a control-inert measurement/orchestration mechanism sufficient to close the fixed prewind prefix while writers remain live.

It must prove, for every member of the population, required identity/evidence persistence and reconciliation with no known omission/duplicate/contradiction.

This mechanism must not change control output, freshness, rate, QoS, disturbance timing, or science.

### L5 — Native-F0 authorization wiring

Place the gate on the actual native-disturbance emission path:

```text
canonical runtime evidence
→ fixed-prefix V3 evaluation/accounting
→ prefix completeness seal
→ persisted gate decision
→ PASS / FAIL / UNKNOWN
→ PASS only opens native F0 eligibility
```

At the single frozen F0 opportunity:

```text
PASS -> emit eligible F0
FAIL/UNKNOWN/INFRA_INVALID -> no F0, retain root, stop row
```

Forbidden:

```text
moving F0
waiting for later evidence
retry-until-PASS
favorable-state selection
window restart
```

### L6 — Exact execution harness

Before science, exercise the production path as far as possible:

```text
_run_row
→ execute_row
→ trace startup
→ runtime attestation
→ prewind mapping
→ prefix seal
→ gate decision
→ F0 authorization boundary
→ collector
→ C1
→ E8
→ ActuatorMotors extraction
→ postprocessor
→ strict JSON
→ validators
→ result/report serialization
→ controlled cleanup/finalization
```

Mandatory counters:

```text
FALSE_PREWIND_PASS_COUNT=0
F0_BEFORE_PREWIND_PASS_COUNT=0
FAIL_ALLOWED_F0_COUNT=0
UNKNOWN_ALLOWED_F0_COUNT=0
INCOMPLETE_PREFIX_ALLOWED_F0_COUNT=0
DYNAMIC_PREFIX_SHORTEN_COUNT=0
FAVORABLE_STATE_RETRY_COUNT=0
WINDOW_RESTART_COUNT=0
F0_SCHEDULE_SHIFT_COUNT=0
```

Any path not genuinely exercised is `RUNTIME_ONLY_UNQUALIFIED`, not PASS.

### L7 — Astra independent audit

Astra verifies:

```text
source semantics
raw evidence
source/receipt timestamp domains
reset/generation/session identities
validator logic
accounting denominators
hashes/provenance
process lifecycle
```

Luna supplies implementation/evidence; Luna does not decide scientific disposition.

### L8 — Execution binding

Only after L1–L7 pass:

- determine exactly which historical slots remain admissible;
- freeze only the actually missing conditions as new prospective attempts;
- preserve every historical invalid/unknown attempt in the ledger;
- bind metric/postprocess/C1/readiness/source identities.

Do not automatically rerun all eight rows unless the audit proves all eight require reacquisition.

### L9 — Phase-D runtime

Execute frozen rows sequentially with:

```text
STOP_ON_FIRST_INFRASTRUCTURE_INVALID=true
RETRY_UNTIL_FAVORABLE=false
ADAPTIVE_CHANGES=false
```

Every valid row requires complete readiness, F0–F4, C1/E8, lifecycle, postprocessing, and finalization evidence.

### L10 — Combined characterization

When all eight planned scientific conditions have contract-qualified usable bindings, produce the combined characterization using the frozen metrics.

Evaluate:

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

### L11 — Bottleneck classification and owner review

Use only the frozen taxonomy:

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

`UNRESOLVED` is acceptable if the complete evidence genuinely cannot discriminate the mechanism.

FAST challenger design remains a separate owner-authorized program.
