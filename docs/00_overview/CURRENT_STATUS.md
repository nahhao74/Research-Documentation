# Current Status — 2026-08-31

## Executive state

The project is in **Phase 0B.2**. Structural/mechanism work is closed, the GUST-P1 causal forensic is closed, and the remaining blocker is a numeric pre-treatment eligibility/scheduling decision for Option B.

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED

OPTION_B_DIRECTION=PREFERRED
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

REFERENCE_STABILITY_PREDICATE=
GUST_PREOFFER_REFERENCE_STABILITY_V1

AUTHORITATIVE_TIME_DOMAIN=
PX4_BOOT_US
# selected/proposed source-time authority
# still inside owner freeze boundary

M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

EVENT_SCHEDULING_POLICY=
DELAY_RESCHEDULE_WITHIN_BLOCK
# RECOMMENDED
# NOT_FROZEN
# NOT_APPROVED

COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=NOT_IDENTIFIABLE
W_MAX_FULL_PREDICATE_OFFLINE_SUPPORT=UNRESOLVED
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN

CHARACTERIZATION_STATE=
PREPARED_FOR_OWNER_NOSCIENCE_RUNTIME_AUTHORIZATION

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

PHASE_0B3_IMPLEMENTATION=BLOCKED
PHASE_0B4_DETERMINISTIC_REGRESSION=BLOCKED
PHASE_0B5_FRESH_NOSCIENCE_PARITY=BLOCKED
PHASE_0B6_SCIENTIFIC_PILOT_OWNER_REVIEW=BLOCKED
PHASE_0B7_FRESH_RANDOMIZED_SCIENCE=BLOCKED
PHASE_0B8_SCIENTIFIC_ANALYSIS=BLOCKED
PHASE_0B9_CAUSAL_DATASET_ACCEPTANCE=BLOCKED

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
MANIFEST_SLOTS_CONSUMED=0
SCIENTIFIC_EXECUTION=NOT_RUN

AEGIS_AUTHORITY=0
ROUND_Z_AUTHORITY=0
production_authority=false
```

The current blocker is **not** a World-Model problem and is no longer the historical C1-frontier timeout. The active workstream remains Phase-0 causal-data qualification.

## Canonical architecture

### Fast path

```text
PX4 / sensors / reference
-> AURA
-> AEGIS FAST / T1 / C1
-> PX4
```

The fast path must remain independently functional. PX4 inner loops remain authoritative.

### Predictive path

```text
PX4 / sensors / reference
-> StateBank always warm
-> World Model / predictive refinement
-> WISE / bounded candidate planning
-> AEGIS
-> PX4
```

The World Model is not yet the active development gate and must never block first response.

## 0B.1 closure — exact causal conclusion

The GUST-P1 failure is classified as:

```text
C_TIMING_DESIGN_INTERACTION_BETWEEN_FROZEN_REFERENCE_TRANSITION_PRECEDENCE
AND_THE_PREOFFER_NATIVE_EVENT_WINDOW
```

Canonical chronology:

```text
GUST ZERO
-> reference transition clears
-> qualified Moving evidence returns
-> DISTURBANCE_ONSET minted

GUST P2
-> reference transition clears
-> qualified Moving evidence returns
-> DISTURBANCE_ONSET minted

GUST P1
-> Moving evidence disappears while REFERENCE_TRANSITION is still active
-> transition clears later
-> qualifying Moving evidence does not recover
-> no DISTURBANCE_ONSET
-> NO_ONSET_CANDIDATE
-> no offer
-> no T_D
-> no ACK
-> no accepted candidate exposure
```

Therefore P1 treatment cannot be the cause of the failure because treatment was never offered or accepted.

## Completed 0B.2 offline characterization

The read-only characterization of the existing immutable GUST evidence reached a hard evidence limit:

```text
COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED
```

Reference-side stable windows are observable for GUST ZERO/P1/P2 across the tested characterization grid through `1,000,000 us`, but the historical native GUST had already occurred before the proposed delayed-launch waiting trajectory.

Consequently post-event AURA/C1/reset state cannot be silently reused as the no-GUST delayed-launch counterfactual.

```text
FULL_PREDICATE PASS=0
FULL_PREDICATE FAIL=0
FULL_PREDICATE UNKNOWN=3
```

The available historical evidence also lacks an exact `nominal_requested_launch_source_us` in `PX4_BOOT_US`, so a valid source-domain wait distribution and `W_MAX_US` coverage envelope cannot yet be computed.

Therefore:

```text
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN
```

No numeric value, including the existing 500 ms reference-change exclusion constant, is promoted to an Option-B margin merely because it already exists in the system.

## Current next gate — Q1

The next bounded evidence gate is:

```text
Q1 — BOUNDED NON-SCIENTIFIC NO-LAUNCH SHADOW QUALIFICATION
```

The Q1 contract is prepared but runtime is not authorized or executed.

Q1 must create a true unexposed waiting trajectory:

```text
session/block ready
-> identify nominal GUST launch opportunity
-> record nominal_requested_launch_source_us in PX4_BOOT_US
-> suppress native GUST for the bounded observation window
-> observe reference/AURA/C1/session/reset/continuity/provenance
-> evaluate GUST_PREOFFER_REFERENCE_STABILITY_V1 in SHADOW ONLY
-> terminate without GUST launch, treatment, T_D, or scientific accounting
```

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

Q1 may establish full-predicate offline timing/support and a candidate `W_MAX_US` **offline coverage** envelope. It still cannot establish actual delayed-launch runtime feasibility because it does not execute the downstream launch/offer/ACK/H1000 chain.

The next owner action is therefore authorization of the bounded Q1 nonscientific runtime, not approval of Option-B implementation and not scientific-pilot authorization.

## Phase-0 dependency ladder

```text
0A mechanism / structural closure                         CLOSED
0B.1 GUST-P1 timing/design forensic                       CLOSED
0B.2a historical/reference-only characterization          COMPLETE TO EVIDENCE LIMIT
0B.2b Q1 no-launch shadow preparation                     PREPARED
0B.2c Q1 live no-launch characterization                  OWNER AUTHORIZATION REQUIRED
0B.2d owner numeric margin/policy freeze                  PENDING
0B.3 Option-B implementation                              BLOCKED
0B.4 deterministic regression                             BLOCKED
0B.5 bounded delayed-launch nonscience qualification      BLOCKED
0B.6 owner scientific-pilot review                        BLOCKED
0B.7 fresh randomized science                             BLOCKED
0B.8 randomized scientific analysis                       BLOCKED
0B.9 causal dataset acceptance                            BLOCKED
```

No additional scientific smoke/corridor should be invented between these gates unless a concrete unresolved validity condition requires one.

## Scientific target remains unchanged

```text
G_action(X,U,h) = Y(B+U,h) - Y(B+ZERO,h)
B = active PX4 + AURA + FAST/T1/C1 baseline
```

Post-treatment FAST/PX4 reactions remain part of the realized closed-loop treatment response.

The frozen randomized campaign remains conceptually:

```text
worker A
8 sessions = 4 CALM + 4 GUST_E
12 blocks/session
96 blocks total
ZERO=48
P1=24
P2=24
```

A technically valid 96/96 campaign does not automatically imply `CAUSAL_DATASET_ACCEPTED`; scientific analysis and acceptance gates remain separate.

## World Model status

Action-conditioned World-Model training is still blocked pending causal dataset acceptance.

The intended future decomposition remains:

```text
Y_future = F_nominal(X,h) + G_action(X,U,h)
```

with later delay-aware handling of `T_D -> T_A`, history ablation, uncertainty, WISE planning and AEGIS safety/runtime assurance.

Current order:

```text
Q1 no-launch evidence
-> owner M_STABLE / W_MAX / scheduling freeze
-> Option-B implementation
-> deterministic regression
-> delayed-launch nonscience qualification
-> scientific pilot + analysis
-> causal dataset acceptance
-> latency / FFT-FRF characterization
-> AURA challenger work
-> World Model v1
-> WISE
-> online adaptation
-> formal AEGIS safety layer
```

## Source Registry

Current research authority:

```text
AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v7
UPDATED=2026-08-29
SOURCES=141
RESOLVED=138
UNRESOLVED=3
UNRESOLVED_IDS=SRC-024,SRC-040,SRC-053
```

Do not invent an unverified v7 JSON path or SHA256.

## Latest read-only validation associated with the 0B.2 characterization

```text
FOCUSED_TESTS=31 PASS
PYTHON_COMPILE=639 files PASS
MARKDOWN_LINKS=PASS
git diff --check=PASS
Codegraph=UP_TO_DATE
```

No live runtime or scientific root was executed for that characterization.

## Storage and authority boundaries

Significant runtime/capture/dataset/training/intermediate artifacts remain under:

```text
/media/nahhao74/KINGSTON
```

`/home` is for source, tests, small deterministic fixtures, configuration and canonical documentation.

```text
SEALED_ACCESS_BOUNDARY=LOCKED_PRE_EVALUATION
FRESH_SCIENCE=BLOCKED
MODEL_TRAINING_ACTION_RESPONSE=BLOCKED_PENDING_CAUSAL_DATA_ACCEPTANCE
production_authority=false
```

Failed scientific roots remain immutable.

## Current roadmap

The active roadmap is the 2026-08-31 v2 roadmap under `docs/04_research/`, with the immediate sequence:

```text
Q1 NO-LAUNCH SHADOW
-> owner M_STABLE / W_MAX / policy freeze
-> delayed-launch nonscience qualification
-> fresh randomized G_action identification
-> latency / FFT-FRF baseline
-> AURA challengers
-> World Model v1
-> WISE
-> bounded adaptation
-> formal AEGIS runtime assurance
```
