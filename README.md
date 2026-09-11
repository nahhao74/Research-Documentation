# Research Documentation — AURA / WISE / World Model / AEGIS vNext

Canonical research documentation for the Moving-Mode UAV Detect & Response pipeline around PX4, AURA, FAST/T1/C1, AEGIS, StateBank, an action-conditioned World Model, and WISE predictive refinement.

Large telemetry, runtime roots, datasets, replay bundles, and generated plots remain outside GitHub under `/media/nahhao74/KINGSTON`. This repository stores architecture, scientific contracts, current execution state, compact evidence history, and research roadmaps.

## Read order

1. [`docs/00_overview/CURRENT_STATUS.md`](docs/00_overview/CURRENT_STATUS.md) — authoritative current state.
2. [`docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_G_ACTION_E8_PREOFFER_REVIEW.md`](docs/00_overview/CURRENT_STATE_CHECKPOINT_20260911_G_ACTION_E8_PREOFFER_REVIEW.md) — latest complete G-action handoff.
3. [`docs/00_overview/DOCUMENT_AUTHORITY.md`](docs/00_overview/DOCUMENT_AUTHORITY.md) — authority and AI-onboarding policy.
4. [`docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md`](docs/05_scientific_contracts/G_ACTION_MRT_V2R1_TU_ORIGIN_PRE_FREEZE_20260911.md) — current pre-freeze timing boundary; not execution authority.
5. [`docs/01_architecture/SYSTEM_ARCHITECTURE.md`](docs/01_architecture/SYSTEM_ARCHITECTURE.md) — structural pipeline.
6. [`docs/01_architecture/CONTROL_ACTION_PATH.md`](docs/01_architecture/CONTROL_ACTION_PATH.md) — control path and PX4 authority.
7. [`docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md`](docs/01_architecture/TIMING_CAUSALITY_STATEBANK.md) — timing, causality, and StateBank.
8. [`docs/03_evidence/MILESTONE_SUMMARY.md`](docs/03_evidence/MILESTONE_SUMMARY.md) — historical audit trail.
9. [`docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md`](docs/04_research/FUTURE_IMPLEMENTATION_ROADMAP.md) — future implementation roadmap.
10. [`docs/02_source_registry/CURRENT_REGISTRY_V9.md`](docs/02_source_registry/CURRENT_REGISTRY_V9.md) — source registry.

Superseded checkpoints remain available as dated lineage; they are not competing current authority.

## Pipeline

```text
                              PREDICTIVE PATH
Sensors / PX4 / Reference ──────┬────> StateBank (always warm)
                                │              │
                                │              v
                                │       World Model / WISE
                                │              │ bounded U_plan
                                │              v
                                │        AEGIS candidate path
                                │              │
                                v              v
                              AURA ─────> FAST/T1/C1 ─────> PX4 ─────> UAV
```

## Current state — 2026-09-11

Current canonical execution:

```text
AURA_EXECUTION_PHASE_V1=CANONICAL
CURRENT_BASELINE_B=PX4+AURA+FAST/T1/C1
FAST_ACTIVE_BASELINE=true
ATTITUDE_MAX_AGE_US=10000
SCIENTIFIC_ACQUISITION_EXECUTED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
```

Current World Model conclusion:

```text
F_ENGINEERING_STATUS=USEFUL_SHORT_HORIZON_ENGINEERING_PREDICTION
G_ENGINEERING_STATUS=NO_PREDICTIVE_GAIN_AT_CURRENT_EFFECTIVE_ACTION_SUPPORT
PRIMARY_G_LIMITATION=INSUFFICIENT_EFFECTIVE_ACTION_HORIZON
MODEL_CAPACITY_INCREASE_JUSTIFIED=false
```

Current treatment timing:

```text
T_U = first source-bound effective E8 candidate application
T_A = exact native accepted ACK frontier
TREATMENT_ONSET=T_U
T_A_ROLE=TRANSACTION_CONFIRMATION
```

Do not promote observed run-specific ordering into a universal timing invariant.

## Closed execution findings

The following are no longer open generic research questions:

```text
AURA V2.1 MultiThreadedExecutor path -> rejected for current architecture
source-rate intervention -> not currently justified
old 3C/5C/7C accepted-event semantics -> not duration-derivable
20 ms treatment -> not supported as default under current source/freshness evidence
model-capacity increase -> not justified
```

Current retained conditional source-history support:

```text
>=4 ms   ~99.96–100%
>=8 ms   ~80.2%
>=12 ms  ~51.5%
>=16 ms  ~13.5%
>=20 ms  ~7.0%
```

These values are engineering support diagnostics, not scientific completion probabilities.

## ZERO / 8 ms / 12 ms campaign

A finite 12-session engineering-only response campaign was executed once under V1 + FAST.

Consumed manifest SHA256:

```text
9cf311644423ab1c65bd52977ef014db1eaeb0e184cd7a1c0c0e3efb3cb13486
```

Observed engineering exposure:

```text
8MS accepted=3; completed=2; one early source-invalid termination
12MS accepted=3; completed=0; all three early source-invalid terminations
```

However the campaign is runtime-invalid for response interpretation and must not be used for a causal ZERO/8/12 signal conclusion.

The consumed manifest and its roots are immutable and must never be replayed as replacement evidence.

## Latest runtime repair

The latest task repaired two implementation defects:

```text
1. runner offer trigger now waits for a current prospective C1 admission witness
2. release validator now binds the exact accepted lifecycle transaction
```

Fresh lifecycle-only qualification:

```text
84 focused tests PASS
ZERO = PASS
8MS  = PASS
12MS = RETAINED_NATIVE_ACCEPTANCE_TIMEOUT_AFTER_QUALIFIED_ADMISSION
```

For accepted transactions, exact T_U/T_A, control-ledger identity, bridge identity, fail-closed semantics, and release binding pass.

No T_U/T_A was fabricated for the rejected 12 ms transaction.

## Current material boundary

The remaining issue is now semantic rather than a generic implementation bug:

```text
current C1-qualified admission witness
→ one assigned offer
→ E8 can still reject the pending offer before native accepted ACK
```

Preventing this requires defining a stronger E8 pre-offer eligibility condition than the currently approved C1 predicate. That changes the opportunity population and therefore requires explicit owner review.

The owner must decide whether:

```text
A. a source-proven, arm-independent, future-free E8 condition becomes common pre-offer eligibility;
B. pre-acceptance E8 rejection remains a legitimate assigned engineering outcome; or
C. conditional G-response identification and C1→E8 practical admission/support are separated.
```

Current next task:

```text
NEXT_TASK=OWNER_REVIEW_G_ACTION_E8_PREOFFER_ADMISSION_CONTRACT
```

No new response campaign should execute before this decision.

## Hard invariants

```text
PX4 remains authoritative
FAST remains active immediate-response baseline
AURA_EXECUTION_PHASE_V1=CANONICAL
ATTITUDE_MAX_AGE_US=10000
legacy EVENT_ONLY_V1 remains unchanged
historical invalid roots remain immutable
large artifacts=/media/nahhao74/KINGSTON
PX4_FIRMWARE_MODIFIED=false
FAST_CONTROL_LAW_MODIFIED=false
T1_C1_MATH_MODIFIED=false
SOURCE_RATE_CHANGED=false
CONTROL_AUTHORITY_CHANGED=false
WM_CONTROL_WRITE=false
WISE_ENABLED=false
AEGIS_WM_AUTHORITY=false
SCIENTIFIC_ACQUISITION_EXECUTED=false
SEALED_PAYLOAD_OPENED=false
G_ACTION_CAUSAL_STATUS=UNQUALIFIED
```

## Repository role

This `Research-Documentation` repository is the canonical research-state and handoff source for the project. Future progress updates should advance `docs/00_overview/CURRENT_STATUS.md` plus a dated checkpoint here, rather than relying on older status text in the runtime repository.
