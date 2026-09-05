# Document Authority and AI Onboarding Policy

## Purpose

This file defines how humans and AI agents should resolve apparently conflicting documentation in `main`.

The repository intentionally preserves historical, date-stamped and research documents. Historical retention is evidence/provenance; it does **not** mean every document is current runtime authority.

## Authority order

When two documents disagree about current state, execution permission, scientific identity or next action, use this order:

```text
1. CURRENT_STATUS.md
2. latest CURRENT_EXECUTION_LADDER_WM_YYYYMMDD.md
3. scientific contracts under docs/05_scientific_contracts/
4. canonical architecture under docs/01_architecture/
5. evidence summaries under docs/03_evidence/
6. active research roadmap under docs/04_research/
7. dated historical snapshots / superseded execution ladders
```

Within the same authority class, prefer the latest explicitly versioned/current document unless a file states that it is frozen historical evidence.

## Current canonical entry points

```text
docs/00_overview/CURRENT_STATUS.md

docs/00_overview/
CURRENT_EXECUTION_LADDER_WM_20260905.md

docs/05_scientific_contracts/
WM1_RANDOMIZED_IDENTIFICATION.md
```

## Historical-document rule

A historical document may contain statements that were true when it was written, for example:

```text
PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED
Q1_RUNTIME_EXECUTED=false
M_STABLE_US=UNFROZEN
```

Those statements are not current if `CURRENT_STATUS.md` now records later qualified state.

Do not delete or rewrite historical evidence merely to make old text agree with today. Instead:

```text
preserve historical artifact
+
mark it as historical/superseded when necessary
+
maintain one clear current-state authority
```

## Runtime vs research authority

Research files describe hypotheses, future algorithms and candidate development directions. They do not authorize a runtime/scientific change.

Examples:

```text
CTEE
CTEE-F
CALE
CIBES
Koopman / DMDc / SINDy / Tiny-MLP
uncertainty methods
formal AEGIS safety filters
```

A research proposal becomes runtime/scientific authority only through an explicit owner-approved contract/implementation/qualification update reflected in current status.

## Architecture vs execution state

Architecture files answer:

```text
what each module owns
how the pipeline is structured
what authority boundaries exist
what invariants must not change silently
```

Execution ladders answer:

```text
what exact task is allowed next
what has already qualified
what current blocker remains
what must stop fail-closed
```

Do not infer current execution permission from a generic architecture description.

## Scientific-contract rule

Scientific contracts define the estimand, treatment identity, timing/causality rules, complete-root requirement and authority boundaries.

Current randomized-pilot identity is:

```text
MANIFEST_ID=WM1_V2R1_FINAL_RANDOMIZED_SCIENTIFIC_PILOT_V1_1
SHA256=361ff557e1b6f2fb9d6e94803ec0cf77e98f4381b0f05499a2bfeadf08027354
```

If a historical document contains a different manifest digest, treat it as historical unless the current scientific contract explicitly restores it.

## Root/evidence rule

Failed roots are immutable evidence.

```text
infrastructure-invalid root != scientific negative result
partial valid rows != scientific dataset
historical replay != permission to rewrite root outcome
```

A later repair may explain a historical failure more precisely, but must not retroactively convert the failed root into a valid scientific root.

## Time-domain rule

Keep these domains distinct unless an explicit qualified mapping is available:

```text
PX4 native / PX4_BOOT_US
Gazebo simulation time
host monotonic / receipt time
mapped cross-domain time
```

Do not fabricate a PX4 source timestamp for a host-only error.

## AI agent onboarding checklist

Before proposing code or a new experiment, an AI should be able to answer:

```text
1. What is the current scientific estimand?
2. What is the active baseline B?
3. What is the current authority boundary?
4. What infrastructure has already qualified?
5. What is the latest immutable root and its classification?
6. What is the first current blocker, not merely the terminal timeout?
7. What exact next task is authorized?
8. What is explicitly NOT authorized?
9. Is the proposed change implementation-preserving or semantic?
10. Which document is the authority for each claim?
```

If these cannot be answered from current canonical documents, stop and resolve documentation/state ambiguity before changing runtime or scientific semantics.

## Current one-line state

As of 2026-09-05:

```text
fresh_33 stopped fail-closed because the next native GUST event was armed before the previous native event reached canonical CLEAR/retirement; the next gate is an implementation-preserving inter-block lifecycle synchronization repair plus bounded non-scientific qualification before any new randomized pilot.
```
