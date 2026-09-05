# Document Authority and AI Onboarding Policy

## Purpose

This file defines how humans and AI agents must read `main` without mixing current authority, architecture, evidence and future research.

The repository should keep **one clear current truth** in `main`. Superseded states and rejected research remain recoverable through Git history rather than being duplicated as competing current documents.

## Authority order

When documents disagree, use this order:

```text
1. docs/00_overview/CURRENT_STATUS.md
2. docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
3. docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md
4. docs/01_architecture/*
5. docs/03_evidence/MILESTONE_SUMMARY.md
6. docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
7. source registry / research references
```

Do not infer current execution state from research notes, source-registry metadata, old commit messages or historical Git revisions.

## Canonical entry points for a new AI

Read in this exact order:

```text
README.md
→ docs/00_overview/CURRENT_STATUS.md
→ docs/00_overview/CURRENT_EXECUTION_LADDER_WM_20260905.md
→ docs/01_architecture/SYSTEM_ARCHITECTURE.md
→ docs/01_architecture/CONTROL_ACTION_PATH.md
→ docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md
→ docs/01_architecture/WORLD_MODEL_WISE.md
→ docs/05_scientific_contracts/WM1_RANDOMIZED_IDENTIFICATION.md
→ docs/03_evidence/MILESTONE_SUMMARY.md
→ docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

## Current pipeline model

A new AI must preserve this separation:

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

As of 2026-09-05 the latest randomized root is `fresh_35`.

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

The exact next task is the accepted-cycle callback visibility/retention forensic described in the latest execution ladder.

No new randomized root is currently authorized.

## Evidence semantics

Failed roots remain immutable evidence:

```text
infrastructure-invalid root != scientific negative result
partial valid rows != scientific dataset
terminal timeout != necessarily first causal divergence
capacity pressure != eviction proof
native producer evidence != observer receipt proof
```

A later repair may explain a historical failure more precisely but never converts that root into scientific data.

## Runtime vs research authority

Research documents answer:

```text
what should be benchmarked later?
what hypothesis is worth testing?
what algorithm might solve a measured limitation?
```

They do **not** authorize runtime/scientific changes.

The active research scope is defined only by:

```text
docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

Rejected or superseded research should not remain as an active alternative in `main`; Git history is the provenance mechanism.

Current active control research is limited to simulator shadow/replay comparison of the current FAST baseline against the smallest justified challengers. No new residual PI/PID loop, cross-airframe adaptive controller or online gain-tuning scheme is an active main-pipeline direction.

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

Before proposing code, experiment or model changes, an AI must be able to answer:

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

If these cannot be answered consistently, stop and resolve documentation ambiguity before changing runtime or scientific semantics.
