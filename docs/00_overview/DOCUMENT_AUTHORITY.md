# Document Authority and AI Onboarding Policy

## Purpose

This file defines how humans and AI agents must read `main` without mixing current authority, architecture, evidence, scientific contracts and historical lineage.

The repository keeps one current truth in `main`. Dated checkpoints and older contracts remain available for lineage, but the newest current-status documents take precedence.

## Authority order

When documents disagree, use this order:

```text
1. docs/00_overview/CURRENT_STATUS.md
2. docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md
3. docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md
4. docs/01_architecture/*
5. docs/03_evidence/world_model/G_ACTION_TU_TA_RUNTIME_20260911.md
6. docs/03_evidence/MILESTONE_SUMMARY.md
7. docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
8. docs/02_source_registry/*
```

Older dated checkpoints and contracts are lineage unless explicitly restored by owner decision.

Do not infer current execution authority from research references, commit history, old Phase-D ladders, or superseded randomized-identification contracts.

## Required read order for a new AI

```text
README.md
→ docs/00_overview/CURRENT_STATUS.md
→ docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md
→ docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md
→ docs/01_architecture/SYSTEM_ARCHITECTURE.md
→ docs/01_architecture/CONTROL_ACTION_PATH.md
→ docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md
→ docs/03_evidence/world_model/G_ACTION_TU_TA_RUNTIME_20260911.md
→ docs/03_evidence/MILESTONE_SUMMARY.md
→ docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

## Canonical pipeline model

```text
FAST PATH
Sensors/PX4/Reference
→ AURA
→ FAST/T1/C1
→ bounded AEGIS path
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
FAST remains active immediate-response baseline
World Model must not block first response
candidate action remains bounded incremental augmentation
StateBank remains causal and always warm
stale/unsupported WM plan -> candidate ZERO/unavailable
legacy EVENT_ONLY_V1 remains unchanged
```

## Current scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1
```

Current owner-approved timing semantics for future bounded-contiguous `G_action` design:

```text
T_D = decision/planning frontier
T_U = first source-bound applied candidate frontier
T_A = exact native accepted ACK frontier
TREATMENT_ONSET=T_U
T_A_SEMANTICS_MODIFIED=false
HOLD_EXPIRY_ORIGIN=T_U_PLUS_PLANNED_HOLD_DURATION_US
G_TARGET_ORIGIN_PROPOSAL=T_U
```

A material FAST change changes baseline `B` and requires versioned review of the action-conditioned data/model contract.

## Current execution boundary

```text
CURRENT_STATUS=INVALID_RUNTIME_IMPLEMENTATION
NEXT_TASK=G_ACTION_TU_TA_RUNTIME_IMPLEMENTATION_REPAIR
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

The current runtime defect is stale pre-gate candidate state in the emitted exposure ledger after fail-closed C1 evaluation. This is not a new timing-semantic conflict.

No AI should freeze or execute MRT until a fresh bounded-contiguous runtime qualifies the source-grounded `T_U`-relative exposure primitive.

## Evidence governance

Engineering smoke data is not formal scientific evidence unless a scientific contract explicitly admits it.

Historical failed/invalid roots remain immutable. Do not post-process an invalid root into a PASS classification.

Large runtime/data artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

Do not place large runtime captures or training artifacts under `/home` or this documentation repository.
