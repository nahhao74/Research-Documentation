# Document Authority and AI Onboarding Policy

## Purpose

This file defines how humans and AI agents must read `main` without mixing current authority, architecture, evidence, scientific contracts and historical lineage.

The repository keeps one current truth in `main`. Dated checkpoints and older contracts remain available for lineage, but the newest current-status documents take precedence.

## Authority order

When documents disagree, use this order:

```text
1. docs/00_overview/CURRENT_STATUS.md
2. docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_G_ACTION_E8_PREOFFER_REVIEW.md
3. docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md
4. docs/01_architecture/*
5. docs/03_evidence/*
6. docs/03_evidence/MILESTONE_SUMMARY.md
7. docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
8. docs/02_source_registry/*
```

Older dated checkpoints and contracts are lineage unless explicitly restored by owner decision.

Do not infer current execution authority from research references, commit history, old Phase-D ladders, superseded randomized-identification contracts, or older checkpoint `NEXT_TASK` fields.

## Required read order for a new AI

```text
README.md
→ docs/00_overview/CURRENT_STATUS.md
→ docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_G_ACTION_E8_PREOFFER_REVIEW.md
→ docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md
→ docs/01_architecture/SYSTEM_ARCHITECTURE.md
→ docs/01_architecture/CONTROL_ACTION_PATH.md
→ docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md
→ docs/03_evidence/MILESTONE_SUMMARY.md
→ docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md
```

The earlier `CURRENT_STATE_CHECKPOINT_20260911_TU_TA_RUNTIME_REPAIR.md` remains lineage, not current execution authority.

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
AURA_EXECUTION_PHASE_V1 remains canonical
ATTITUDE_MAX_AGE_US=10000
```

## Current scientific target

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1
```

Current owner-approved timing semantics for bounded G-action design:

```text
T_D = decision/planning frontier
T_U = first source-bound effective E8 candidate application
T_A = exact native accepted ACK frontier
TREATMENT_ONSET=T_U
T_A_SEMANTICS_MODIFIED=false
HOLD_EXPIRY_ORIGIN=T_U_PLUS_ASSIGNED_DURATION
```

Do not promote observed run-specific ordering into a universal invariant unless source semantics establish it.

A material FAST change changes baseline `B` and requires versioned review of the action-conditioned data/model contract.

## Current execution boundary

```text
CURRENT_STATUS=BLOCKED_MATERIAL_RUNTIME_SEMANTIC_CHANGE
NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

The original offer-before-C1 race and release-validator defect have been repaired and fresh lifecycle qualification was run.

Latest qualification:

```text
ZERO=PASS
8MS=PASS
12MS=RETAINED_NATIVE_ACCEPTANCE_TIMEOUT_AFTER_QUALIFIED_ADMISSION
```

The remaining owner boundary is prospective eligibility semantics:

```text
C1-qualified admission witness
→ one assigned offer
→ E8 can still reject before native accepted ACK
```

Preventing that rejection requires defining a stronger E8 pre-offer eligibility condition. Such a condition must not be introduced silently because it changes the opportunity population and may alter the scientific interpretation.

No AI should execute another response-identification campaign until the owner decides whether stronger source-proven E8 pre-offer eligibility is allowed, whether E8 rejection remains an assigned outcome, or whether conditional response and admission-support questions are separated.

## Current closed results that must not be reopened casually

```text
F short-horizon prediction useful
G no predictive gain at current effective action support
model-capacity escalation not justified
old event-only 3C/5C/7C not duration-derivable
AURA V2.1 executor rejected for current architecture
20 ms default treatment not supported as scientific default
source-rate change not currently justified
consumed ZERO/8/12 response manifest immutable
```

## Evidence governance

Engineering smoke/qualification data is not formal scientific evidence unless a scientific contract explicitly admits it.

Historical failed/invalid roots remain immutable. Do not post-process an invalid root into a PASS classification.

The consumed 12-session response manifest SHA256 is:

```text
9cf311644423ab1c65bd52977ef014db1eaeb0e184cd7a1c0c0e3efb3cb13486
```

It must never be replayed as replacement evidence.

Large runtime/data artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

Do not place large runtime captures or training artifacts under `/home` or this documentation repository.

## Repository role

`Research-Documentation` is the canonical research-state / handoff repository for this project. When updating progress, update this repository's `CURRENT_STATUS.md` and a dated checkpoint before relying on older runtime-repository documentation.
