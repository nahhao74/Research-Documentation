# Milestone and Root-Cause Summary

This is the compact canonical audit trail. Large runtime roots and telemetry remain under `/media/nahhao74/KINGSTON`; detailed evidence is indexed under `docs/03_evidence/`.

For current state use:

```text
../00_overview/CURRENT_STATUS.md
../00_overview/CURRENT_EXECUTION_LADDER_PHASE_D_20260908.md
phase_d/README.md
```

## Historical foundation retained

Before the current Phase-D program, the project established:

```text
bounded additive AEGIS candidate architecture
PX4 control authority
exact candidate/exposure identity
native-source vs clock-mapping separation
StateBank startup/causal barriers
WM reverse-index → graph → Tarjan SCC → fixed-point peeling validity engine
continuous-C1 replay/recovery
post-reset E8 source-causal pairing
native-event CLEAR lifecycle
```

The randomized WM1 `G_action` track remains separately gated; Phase-D currently characterizes the active B0 baseline before any FAST challenger decision.

## D0 V3 closure — 2026-09-06/07

D0 V3 progressed through startup, owner-provenance, downstream-correspondence, E8-ingestion, explained-cause, and lifecycle defects without changing control semantics.

The fresh canonical qualification root finally closed formal readiness:

```text
ROOT=/media/nahhao74/KINGSTON/Detect_and_Response/d0_v3_calm_readiness_20260907_02
FORMAL_D0_DECISION=PASS_D0_V3_READINESS
TOTAL_REQUIRED=4000
ACCOUNTED=4000
VALID_CONTROL=2537
EXPLAINED_CONTROL_UNAVAILABLE=1463
NOT_READY=0
READINESS_FAILURE=0
INVARIANT=0
UNKNOWN=0
W20/C1/E8=4000/4000
```

Therefore:

```text
D0_INFRASTRUCTURE_CLOSED=true
READY_FOR_PHASE_D=true at the D0 qualification boundary
```

Detailed D0 evidence remains under `d0_v3/`.

## Phase-D B0 sequence — 2026-09-07/08

### 1. Frozen B0 contract and eight-condition campaign

```text
BASELINE_ID=B0_PX4_AURA_FAST_T1_C1_CURRENT
METRIC_CONTRACT_ID=PHASE_D_B0_METRICS_V1_1
METRIC_CONTRACT_SHA256=5928dceea0a6e8e745f94282f7834bc268d0c10ccf61f2ccc4b424d43b78ed93
CAMPAIGN_MANIFEST_SHA256=dfab451d58f45e087fc9b25ab5eb9866bbff8e8d8ac6bd0475dffe25c1a2d247
```

No performance thresholds, adaptive changes, favorable retries, or challenger selection were introduced.

### 2. Original campaign — four measured rows, then C1 retention failure

Slots 1–4 produced complete F0–F4 measurement outputs under the then-active tooling. Original slot 5 stopped on a C1 mutation-retention gap.

The defect was measurement infrastructure, not control behavior.

### 3. C1 callback→persistence accounting closure

Prospective repair introduced explicit callback/persisted/drop/error/gap/finalization accounting under `V3_C1_TRACE_WRITER_ACCOUNTING_V1`.

Historical original slot-5 failure remains immutable.

### 4. Option-B slot 5 — runtime complete, strict JSON failed

Later slot 5 completed runtime evidence but strict JSON postprocessing rejected expected non-finite `ActuatorMotors.control[4..11]` fixed-width padding.

Qualified repair preserved full channel positions using JSON `null` + explicit finite mask/count/index metadata; active-channel non-finite remains fail-closed.

Read-only derived requalification preserved F0–F4 and latency exactly.

### 5. Refreeze V2 slot 6 — precollector closure-binding bug

The first V2 slot-6 attempt stopped before collector/F0 on:

```text
UnboundLocalError: runtime_attestation_emitted
```

Minimal `nonlocal` repair qualified with affected D0/Phase-D/C1 regressions. Historical failed root remains immutable and is not reprocessable as a scientific row.

### 6. Prewind contract/wiring audit — current blocker

Source audit found that historical Phase-D execution did not canonical-enforce the complete V3/V2 prewind gate on the native-F0 path. Retained slots 1–5 also lack the canonical attestation marker and `TRACE_MEASUREMENT_READY` lifecycle transition.

Canonical descriptive replay proves their pre-F0 evaluation populations are eventually accountable with finalized downstream evidence, but does not prove contemporaneous live completeness before F0.

Current disposition:

```text
slots 1..5 = UNKNOWN_MISSING_FROZEN_PREWIND_EVIDENCE
HISTORICAL_CONTROL_FAILURE=NOT_INFERRED
RECORDED_USABLE_CONDITIONS=5_UNCHANGED_PENDING_AUDIT
```

### 7. Owner-approved prewind population/checkpoint

```text
population = every canonical V3 evaluation from first V2 eligibility to F0 exclusive
checkpoint = one-shot immediately before the frozen F0 opportunity
```

No 20-second dwell, no F0 movement, no retry-until-PASS, no favorable-state selection, no dynamic prefix shortening, and no window restart.

A control-inert prefix-completeness seal may be implemented to prove live persistence/reconciliation while writers remain active; end-of-row finalization stays mandatory and separate.

## Current milestone gate

```text
canonical V3 -> prewind semantic mapping
→ predicate-by-predicate historical audit
→ fixed-prefix completeness mechanism
→ minimal native-F0 wiring
→ exact execution-harness qualification
→ Astra independent audit
→ freeze only actually missing scientific conditions
→ Phase-D runtime
→ eight contract-qualified usable conditions
→ combined characterization
→ bottleneck classification
```

No FAST replacement algorithm is selected at this stage.

## Evidence retention rule

Formal roots, failed attempts, and historical results remain immutable. Prospective repairs and derived artifacts add lineage; they do not erase prior failures or transform unfavorable valid science into a rerunnable condition.
