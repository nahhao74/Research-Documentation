# Phase-D B0 Evidence Index

This directory is the canonical compact evidence package for **Phase-D B0 characterization**. Large runtime roots and telemetry remain under `/media/nahhao74/KINGSTON`; Git stores compact audit reports, lineage, decisions, and closure summaries.

## Current state — 2026-09-10

Formal Phase-D qualification is preserved but temporarily paused while engineering work moves to FAST response characterization and challenger selection.

```text
CURRENT_MODE=ENGINEERING_FAST_CHARACTERIZATION
CURRENT_FAST_BASELINE=PX4_AURA_FAST_T1_C1_CURRENT
TAKEOFF_OFFBOARD_RUNTIME_BASELINE=PASS
FAST_STRUCTURE_SELECTION=NOT_COMPLETED
READY_FOR_FRESH_Q1_COMPONENT_QUALIFICATION=true
Q1_COMPONENT_QUALIFICATION_EXECUTION=PAUSED_BY_OWNER
READY_FOR_PHASE_D_RUNTIME=false
FULL_B0_V3_READY=false
```

For authoritative current status, read `../../00_overview/CURRENT_STATUS.md` first.

## Latest closure

`PHASE_D_TAKEOFF_OFFBOARD_RUNTIME_SMOKE_V1` passed on 2026-09-10 and closed the Gazebo/offboard/takeoff prerequisite after an implementation-preserving DART loader repair.

See:

`closures/TAKEOFF_OFFBOARD_RUNTIME_SMOKE_20260910.md`

The smoke established PX4 startup, DDS connectivity, setpoint streaming, offboard acceptance, arming, takeoff altitude attainment, stable hover, clean finalization, and zero scientific disturbance. It did not qualify Q1 or authorize Phase-D runtime.

## Q1 checkpoint retained

The limited Q1 component contract is implemented and passed offline qualification:

```text
MINIMUM_PREPARED_CONTRACT_ID=PHASE_D_MINIMUM_PREPARED_Q1_SCOPE_V1
Q1_RESULT_SCHEMA=PHASE_D_Q1_COMPONENT_QUALIFICATION_V1
OWNER_WATERMARK_PRODUCTION_BINDING=PASS_OFFLINE
OWNER_CLOSURE_REMAINS_MANDATORY=true
TRACE_BARRIER_PREDECISION_POLICY=AUXILIARY
E8_INACTIVE_Q1_POLICY=NOT_APPLICABLE
FULL_B0_V3_CONTRACT_UNCHANGED=true
FINAL_PERSISTENCE_RECONCILIATION=PASS_OFFLINE
```

Attempt15 remains an immutable fail-closed qualification root because the active opportunity was never reached. The later smoke closed its takeoff/runtime prerequisite but does not rewrite attempt15.

## Directory map

```text
phase_d/
├── README.md
├── campaign/
│   └── CAMPAIGN_STATUS.md
├── closures/
│   ├── CLOSURE_SUMMARY.md
│   └── TAKEOFF_OFFBOARD_RUNTIME_SMOKE_20260910.md
└── prewind/
    ├── PHASE_D_PREWIND_HISTORICAL_AUDIT_20260908.md
    ├── PREWIND_PREDICATE_AUDIT_20260908.md
    ├── PREWIND_CHECKPOINT_DECISION_20260908.md
    ├── PREWIND_CHECKPOINT_C_DECISION_20260908.md
    ├── SOURCE_FRONTIER_BINDING_AUDIT_20260908.md
    ├── SCHEDULED_FRONTIER_AUDIT_20260908.md
    └── evidence/
        ├── HISTORICAL_EVIDENCE_HASHES_20260908.json
        └── REPLAY_INPUT_HASHES_20260908.json
```

Frozen scientific/qualification contracts remain under:

```text
../../../05_scientific_contracts/phase_d/
```

## Historical evidence lineage

### 1. D0 V3 and Phase-D infrastructure

Formal D0 V3 readiness was previously closed. Subsequent Phase-D work exposed and repaired several infrastructure/serialization/binding defects without inferring scientific or control failure.

Historical failed roots remain immutable.

### 2. C1 trace retention closure

The original slot-5 C1 callback/persistence gap was closed prospectively with explicit accounting. Historical rows were not rewritten.

### 3. Strict-JSON actuator padding closure

Expected non-finite fixed-width inactive actuator padding was represented as strict JSON-safe `null + mask/count/index`; active channels remain required finite.

### 4. Prewind checkpoint-C qualification lineage

Source/frontier analysis established that physical F0 cannot itself serve as a guaranteed pre-emission population endpoint in the ordinary path. The prospective prewind population therefore moved to:

```text
PREWIND_POPULATION_V2=[V2_ELIGIBLE,C)
```

with fixed source-owned checkpoint `C` and unchanged physical F0 semantics.

### 5. Observational continuity simplification

The qualification claim was narrowed from absolute hidden-state exclusion to:

```text
NO_OBSERVED_CONTINUITY_VIOLATION_UNDER_BOUND_OBSERVATION_MODEL
```

under explicit limitations. Event-complete internal EKF/incarnation proof is not a default Phase-D gate.

### 6. Production binding closures through attempt13

Fresh live work closed, in order, the pre-C PX4 baseline binding, DiagnosticStatus serialization, fixed-C observational continuity, trace replay, and evaluation ingestion. Attempt13 reached 1,507 handoff evaluations but did not complete the prepared seal.

### 7. Minimum Q1 component contract

A narrow necessity review retained owner closure as scientifically mandatory for fixed-population completeness, made the pre-decision exact trace barrier auxiliary for the limited Q1 component result, and made E8 prefix not applicable only when E8 is explicitly inactive in that Q1 branch.

This changed the qualification admission contract, not the scientific experiment, PX4 control, FAST semantics, physical F0 semantics, or Phase-D metrics.

### 8. Attempt15 and runtime smoke closure

Attempt15 stopped before V2/C/Q1 because the vehicle did not complete the takeoff prerequisite. A bounded engineering smoke then isolated and repaired the Gazebo DART plugin loader issue and proved the normal PX4/Gazebo/ROS 2/offboard/takeoff/hover path functional.

The current owner decision is to pause further qualification and use the now-stable runtime baseline to characterize FAST candidates.

## Current engineering boundary

Qualification mechanisms remain implemented and must be restored for formal scientific evidence. They are not on the exploratory engineering critical path.

```text
ENGINEERING_RUNS_ARE_SCIENTIFIC_EVIDENCE=false
FORMAL_Q1_AND_PHASE_D_CONTRACTS_RETAINED=true
PX4_FIRMWARE_MODIFIED=false
READY_FOR_PHASE_D_RUNTIME=false
```

Current engineering sequence:

```text
Tkinter response monitor
→ current FAST reference response
→ bounded FAST challengers
→ select/freeze best FAST structure
→ resume formal B0 characterization
```

## Scientific target retained

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + selected FAST baseline
```

World Model / WISE identification should proceed against a stable selected FAST baseline rather than one that is still being changed during challenger exploration.

## Read order inside Phase-D

1. `../../00_overview/CURRENT_STATUS.md`
2. `../../00_overview/CURRENT_STATE_CHECKPOINT_20260910.md`
3. `README.md` (this file)
4. `closures/TAKEOFF_OFFBOARD_RUNTIME_SMOKE_20260910.md`
5. `../../../05_scientific_contracts/phase_d/README.md`
6. `../../../05_scientific_contracts/phase_d/PREWIND_QUALIFICATION_V2.md`
7. `prewind/PREWIND_CHECKPOINT_C_DECISION_20260908.md`
8. `prewind/SOURCE_FRONTIER_BINDING_AUDIT_20260908.md`
9. `prewind/SCHEDULED_FRONTIER_AUDIT_20260908.md`
10. `prewind/PREWIND_PREDICATE_AUDIT_20260908.md`
11. `prewind/PHASE_D_PREWIND_HISTORICAL_AUDIT_20260908.md`
12. `closures/CLOSURE_SUMMARY.md`
13. `campaign/CAMPAIGN_STATUS.md`

## Integrity rule

Historical reports/results and decisions remain immutable lineage. New closures, reduced qualification scopes, engineering pivots, or later FAST selections must not overwrite prior classifications. Large runtime evidence remains on KINGSTON; this repository stores compact canonical documentation only.
