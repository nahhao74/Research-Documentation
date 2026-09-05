# Document Authority and AI Onboarding Policy

## Purpose

This file defines how humans and AI agents must read `main` without mixing current authority, architecture, evidence and future research.

The repository keeps **one current truth** in `main`. Superseded states and rejected research remain recoverable through Git history rather than existing as competing active documents.

## Authority order

When documents disagree, use this order:

```text
1. docs/00_overview/CURRENT_STATUS.md
2. docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
3. docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md
4. docs/01_architecture/*
5. docs/03_evidence/MILESTONE_SUMMARY.md
6. docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
7. docs/02_source_registry/*
```

Do not infer current execution state from research references, source-registry metadata, commit history or deleted/superseded documents.

## Required read order for a new AI

```text
README.md
→ CURRENT_STATUS.md
→ CURRENT_EXECUTION_LADDER_WM_20260905.md
→ SYSTEM_ARCHITECTURE.md
→ CONTROL_ACTION_PATH.md
→ TIMING_CAUSALITY_STATEBANK.md
→ WM_CAUSAL_VALIDITY_ENGINE.md
→ WORLD_MODEL_WISE.md
→ WM1_RANDOMIZED_IDENTIFICATION.md
→ MILESTONE_SUMMARY.md
→ FUTURE_IMPLEMENTATION_ROADMAP.md
```

## Canonical pipeline model

```text
FAST PATH
Sensors/PX4/Reference
→ AURA
→ FAST/T1/C1
→ bounded AEGIS acceleration-correction path
→ PX4
→ UAV

PREDICTIVE PATH
causal StateBank
→ World Model
→ WISE bounded candidate plan
→ AEGIS candidate path
→ PX4
```

Hard invariants:

```text
PX4 remains authoritative
World Model must not block first response
current Phase-0 FAST/T1/C1 baseline remains active
candidate action is bounded incremental augmentation
StateBank is causal and always warm
stale/unsupported WM plan -> candidate ZERO/unavailable
```

## Current scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + current FAST/T1/C1 baseline
```

This is closed-loop action identification, not open-loop airframe identification.

A material FAST change changes baseline `B` and therefore requires a versioned review of the action-conditioned data/model contract.

## Current execution state

As of 2026-09-05:

```text
LATEST_FAILED_SCIENTIFIC_ROOT=fresh_35
FRESH35_RESULT=INVALID_INFRASTRUCTURE_NEW_ROOT_IMMUTABLE
PRIMARY_BOUNDARY=ACCEPTED_CYCLE_STATUS_SUCCESSOR_UNAVAILABLE_TO_PROBE
PRODUCER_ABSENCE=NOT_PROVEN
CAPACITY_CAUSALITY=LIKELY_BUT_NOT_PROVEN

G_ACTION_PILOT_RESULT=NOT_EVALUATED
CAUSAL_DATASET_ACCEPTANCE=BLOCKED
SCIENTIFIC_EXECUTION=NOT_RUN
MANIFEST_SLOTS_CONSUMED=0
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

The exact next task is the accepted-cycle callback visibility/retention forensic defined by the latest execution ladder.

No new randomized root is currently authorized.

## Evidence semantics

```text
infrastructure-invalid root != scientific negative result
partial valid rows != scientific dataset
terminal timeout != necessarily first causal divergence
capacity pressure != eviction proof
native producer evidence != observer receipt proof
```

A later repair may explain a historical failure but never converts the failed root into scientific data.

## Runtime vs research authority

The active research scope is defined only by:

```text
docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

Research material answers what should be benchmarked later; it does not authorize runtime/scientific changes.

Methods not present in the active roadmap must be treated as background reference only, regardless of whether they appear in the source registry or Git history.

## Time-domain rule

Keep these domains distinct unless an explicit qualified mapping exists:

```text
PX4 native / PX4_BOOT_US
Gazebo simulation time
host monotonic / receipt time
mapped cross-domain time
```

Never fabricate a PX4 source timestamp for a host-only diagnostic.

## AI change checklist

Before proposing code, experiment or model changes, an AI must answer:

```text
1. What is the current estimand?
2. What is active baseline B?
3. What is the latest immutable root?
4. What is the first proven blocker?
5. What has already been qualified?
6. What exact next task is authorized?
7. What is explicitly forbidden?
8. Does the proposed change alter baseline B or scientific semantics?
9. Is the proposal current work or future research?
10. Which canonical document supports each answer?
```

If these answers are inconsistent, resolve documentation/state ambiguity before changing runtime or scientific semantics.
