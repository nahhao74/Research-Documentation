# Pipeline Current State — 2026-08-29

## Architecture

Moving-mode vNext pipeline: PX4 + AURA + FAST/T1/C1 baseline always active, bounded incremental candidate, StateBank always warm, World Model/WISE predictive refinement, AEGIS fast immediate response, PX4 inner loops authoritative.

Scientific target:

`G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)`

with `B = active PX4 + AURA + FAST/T1/C1 baseline`.

## Current scientific state

The randomized WM1 V2R1 pilot has not yet produced a complete valid 8-session / 96-block campaign. No efficacy claim or `ACTION_RESPONSE_IDENTIFIED` is authorized. SEALED remains locked and `production_authority=false`.

The latest attempted scientific root stopped **before the first scientific T_D**, with zero scientific blocks/actions/manifest slots consumed. The direct cause was a specialized pilot-runner orchestration defect: the `block_index=-1` bootstrap call omitted the existing `bootstrap_only=True` flag and therefore fell through into the candidate-offer/C1-frontier path.

The StateBank/AURA bootstrap readiness repair itself passed live in that root: all seven required streams were present and the snapshot ACK was accepted. The next implementation step is therefore the minimal call-site repair plus exact specialized-runner pre-science qualification; no broad subsystem redesign is indicated by current evidence.

## Closed blockers / qualified infrastructure

- Incremental candidate live mechanism qualified with FAST/T1/C1 baseline active.
- E8 pending-ACK precedence repaired and qualified.
- Strict qualified-record JSON serialization repaired and qualified.
- uXRCE SensorCombined head-of-line stall localized to synchronous diagnostics logging and mechanically repaired.
- Canonical 183018-us false source gap decomposed into an 8000-us native source delta plus a -175018-us Timesync offset transition.
- Dual-domain timestamp semantics approved: native PX4 source continuity is separate from cross-domain clock alignment.
- `SensorCombinedStampedV1` versioned observational wrapper provides atomic native source identity + sender mapping provenance while preserving the standard SensorCombined semantics.
- Live single-session wrapper qualification passed; live 8-session CALM/GUST_E lifecycle soak passed.
- StateBank/AURA startup race closed with an explicit all-required-stream readiness barrier plus atomic barrier recheck; exact pre-science bootstrap lifecycle qualified 8/8.

## Frozen key contracts

- Native source continuity: native PX4 source timestamp + generation; 20,000-us threshold unchanged.
- Clock alignment: separate causal mapping epoch/provenance predicate.
- Mapping transition is not a native source gap.
- Transition-time science remains fail-closed unless exact live transition certification is established.
- T_D/T_A remain native PX4 source/accepted frontiers.
- H1000 remains 1,000,000 native source us, candidate-only.
- ZERO/P1/P2 exposure is exact; no compensation/re-dose/post-hoc reconstruction.
- Failed scientific roots are immutable; no patch-and-continue or partial-root pooling.
- Large runtime/data artifacts remain under `/media/nahhao74/KINGSTON`.

## Immediate next gate

Repair the specialized randomized-pilot bootstrap invocation to pass `bootstrap_only=True`, audit all `run_transaction()` call sites, and run one exact pre-science specialized-runner qualification proving snapshot ACK -> immediate bootstrap return with zero candidate offer, zero C1 offer-frontier wait, zero T_D and zero manifest slot consumption.

Only after that qualification should the owner review authorization for exactly one fresh randomized pilot root.
