# Pipeline Current State — 2026-08-29

## Architecture

Moving-mode vNext pipeline: PX4 + AURA + FAST/T1/C1 baseline always active, bounded incremental candidate, StateBank always warm, World Model/WISE predictive refinement, AEGIS fast immediate response, PX4 inner loops authoritative.

Scientific target:

`G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)`

with `B = active PX4 + AURA + FAST/T1/C1 baseline`.

## Current scientific state

The randomized WM1 V2R1 pilot has not yet produced a complete valid 8-session / 96-block campaign. No efficacy claim or `ACTION_RESPONSE_IDENTIFIED` is authorized. SEALED remains locked and `production_authority=false`.

The latest scientific root remained invalid infrastructure pre-science and immutable, but its direct orchestration defect has now been mechanically repaired and qualified outside that root. The specialized pilot runner now passes `bootstrap_only=True` for the session-start `block_index=-1` transaction, returns immediately after the accepted source-complete snapshot ACK, and does not enter candidate/C1/E8/scientific paths.

Exact live specialized-runner preflight passed with all seven StateBank streams present, AURA present, accepted snapshot ACK, zero candidate offers/publishes, zero C1 offer-frontier waits, zero E8 candidate transactions, no scientific `T_D`, zero scientific blocks/actions and zero manifest-slot consumption.

Current state:

`PILOT_BOOTSTRAP_ONLY_CALLSITE_REPAIR_QUALIFIED_READY_FOR_OWNER_FRESH_PILOT_RETRY_REVIEW`

Therefore the immediate remaining gate is owner authorization for exactly one fresh randomized pilot root. The pilot itself remains the gate before action-conditioned WM scientific training.

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
- Specialized randomized-pilot bootstrap call-site defect closed: `bootstrap_only=True` is explicit for `block_index=-1`; exact real-runner preflight passed.
- Follow-on observer handling of intentional bootstrap `accepted_status=None` was mechanically guarded and included in the final qualification.

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

## Current qualified pilot-orchestration evidence

- `RUN_TRANSACTION_CALLSITE_AUDIT=PASS`
- `REGRESSION_TEST_RESULT=PASS_51`
- `LIVE_EXACT_RUNNER_PREFLIGHT=PASS_FINAL_ROOT`
- `STATEBANK_REQUIRED_STREAMS_PRESENT=7/7`
- `BOOTSTRAP_SNAPSHOT_ACK=VALID_ACCEPTED`
- `BOOTSTRAP_ONLY_FLAG_LIVE=PASS`
- `BOOTSTRAP_RETURNED_BEFORE_CANDIDATE_PATH=true`
- `CANDIDATE_OFFER_COUNT=0`
- `CANDIDATE_PUBLISH_COUNT=0`
- `C1_OFFER_FRONTIER_WAIT_COUNT=0`
- `E8_CANDIDATE_TRANSACTION_COUNT=0`
- `FIRST_SCIENTIFIC_T_D=NOT_REACHED`
- `MANIFEST_SLOTS_CONSUMED=0`
- native source max gap 8000 us; no >20-ms gap; clock/provenance invalid count 0.

## Immediate next gate

Owner review may now authorize exactly one fresh WM1 V2R1 randomized pilot root using the repaired specialized runner and frozen manifest.

A successful complete 8-session / 96-block valid pilot is still required before interpreting P1/P2 treatment signal and before proceeding to action-conditioned World Model training. If the complete valid pilot has insufficient treatment SNR, move to owner experiment-design review rather than training on unresolved signal.
