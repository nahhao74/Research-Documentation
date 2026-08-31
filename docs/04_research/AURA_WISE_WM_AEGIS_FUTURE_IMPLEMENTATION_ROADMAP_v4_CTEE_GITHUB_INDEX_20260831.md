# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap — v4 CTEE GitHub Retrieval Index

**Scope:** Moving Mode only  
**Roadmap revision:** `v4 — CTEE formalization`  
**Updated:** 2026-08-31  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8`

> This file is a GitHub retrieval/index view. The exact full canonical roadmap artifact is `AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_20260831.md` in the Detect and Response Project/File Library. This index does not replace or silently rewrite that source artifact.

## Current Phase-0 state

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

AUTHORITATIVE_TIME_DOMAIN=PX4_BOOT_US
M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN

COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED
FULL_PREDICATE_COUNTERFACTUAL_SUPPORT=NOT_IDENTIFIABLE
W_MAX_FULL_PREDICATE_OFFLINE_SUPPORT=UNRESOLVED
W_MAX_RUNTIME_FEASIBILITY=UNPROVEN

CHARACTERIZATION_STATE=
PREPARED_FOR_OWNER_NOSCIENCE_RUNTIME_AUTHORIZATION

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
MANIFEST_SLOTS_CONSUMED=0
SCIENTIFIC_EXECUTION=NOT_RUN
production_authority=false
```

## Immediate Phase-0 ladder

```text
0A      structural/mechanism closure                         CLOSED
0B.1    GUST-P1 timing/design forensic                       CLOSED
0B.2a   historical/reference-only characterization           COMPLETE TO EVIDENCE LIMIT
0B.2b   Q1 no-launch shadow preparation                      PREPARED
0B.2c   Q1 live no-launch characterization                   OWNER AUTHORIZATION REQUIRED
0B.2d   owner numeric margin/policy freeze                   PENDING
0B.3    Option-B implementation                              BLOCKED
0B.4    deterministic regression                             BLOCKED
0B.5    fresh bounded nonscientific delayed-launch parity    BLOCKED
0B.6    owner scientific-pilot review                        BLOCKED
0B.7    fresh randomized science                             BLOCKED
0B.8    randomized scientific analysis                       BLOCKED
0B.9    causal dataset acceptance                            BLOCKED
```

Q1 must create a true unexposed waiting trajectory around the nominal GUST launch opportunity:

```text
session/block ready
-> identify nominal GUST launch opportunity
-> record nominal_requested_launch_source_us in PX4_BOOT_US
-> DO NOT launch native GUST
-> observe reference / AURA / C1 / session-reset / continuity / provenance
-> evaluate GUST_PREOFFER_REFERENCE_STABILITY_V1 in SHADOW ONLY
-> bounded termination
```

Critical invariant:

```text
NO_NATIVE_GUST_AFTER_NOMINAL_OPPORTUNITY
```

Q1 does not freeze `M_STABLE_US` or `W_MAX_US` and cannot establish `W_MAX_RUNTIME_FEASIBILITY`.

## Owner freeze boundary

Only explicit owner approval may freeze:

```text
APPROVE_OPTION_B=true|false
M_STABLE_US=<numeric source-time interval>
W_MAX_US=<bounded common timeout>
APPROVE_DELAY_RESCHEDULE=true|false
APPROVE_FAIL_CLOSED_TIMEOUT=true|false
APPROVE_SOURCE_TIME_DOMAIN=PX4_BOOT_US|other
```

Until then:

```text
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false
```

## v4 algorithmic-infrastructure structure

v4 deliberately separates four roles:

```text
STATIC STRUCTURAL VALIDATION
    Tarjan SCC

LIVE TEMPORAL / CAUSAL ELIGIBILITY
    CTEE

OFFLINE SCIENTIFIC TIMELINE VALIDATION
    Sweep Line

FUTURE CONDITIONAL RESOURCE ALLOCATION
    MCMF / network flow
```

These roles must not be collapsed into one super-algorithm.

## CTEE — Causal Temporal Eligibility Engine

CTEE is a **project proposal** for live runtime eligibility after Q1 evidence and owner contract freeze. It is not a flight-control law and does not replace AURA, FAST/T1/C1, World Model, WISE, AEGIS, or PX4 inner loops.

Core proposal:

```text
THREE-VALUED PREDICATES
        ↓
SOURCE-BOUND MAX-PLUS FRONTIER
        ↓
READINESS POTENTIAL / BLOCKER
        ↓
ATOMIC RECHECK + COMMIT / FAIL-CLOSED
```

Three-valued predicates preserve:

```text
PASS
FAIL
UNKNOWN / INCONCLUSIVE
```

For source-time prerequisite frontiers `T_i`, the proposed earliest admissible frontier is conceptually:

```text
T_eligible = max_i(T_i)
```

subject to matching source-time domain, session, reset lineage, generation and provenance.

Target properties:

```text
P1 no early action: T_commit >= T_eligible
P2 pre-treatment measurability
P3 arm neutrality
P4 generation consistency
P5 bounded fail-closed behavior
```

Novelty boundary:

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

Do not claim invention of three-valued monitoring, max-plus timing, timed delay/suppression, runtime enforcement, atomic validity-recheck concepts or pre-treatment availability.

## CTEE benchmark / go-no-go

CTEE must compete against simpler alternatives:

```text
A — current/ad-hoc eligibility state machine
B — conventional timed FSM / timed-guard implementation
C — CTEE
```

Evaluate correctness, tail timing/jitter, runtime cost, scientific support/timeout cost and engineering complexity.

Retain CTEE only if it provides a material advantage in correctness, tail ready→commit latency/jitter, or implementation/forensic complexity without changing scientific/control semantics.

If CTEE is equivalent to a simpler timed FSM, retire the CTEE proposal and use the simpler implementation.

## Post-freeze Phase-0 implementation order

```text
Q1 observed no-GUST waiting trajectory
        ↓
owner freezes M_STABLE_US / W_MAX_US / scheduling / source-time policy
        ↓
static dependency graph extraction
        ↓
Tarjan SCC preflight
        ↓
CTEE qualification
  ├─ three-valued predicates
  ├─ source-bound max-plus frontier
  ├─ readiness potential
  └─ atomic recheck
        ↓
bounded nonscientific delayed-launch qualification
```

CTEE may implement an already-approved Option-B contract more deterministically; it may not choose the scientific contract itself.

## Algorithm boundaries

- **Tarjan SCC:** static/preflight dependency-cycle validator; not part of live CTEE loop.
- **Sweep Line:** offline/replay interval validator for reference transition, GUST, AURA-valid, exposure, release, H1000, reset/session, source-gap and plan-TTL intervals; not live scheduling.
- **MCMF/network flow:** future constrained allocation only if a real allocation problem appears; not a Phase-0 runtime dependency.

## Long-term sequence

```text
Q1 no-launch shadow
-> owner margin/policy freeze
-> Tarjan preflight + CTEE benchmark/qualification
-> delayed-launch nonscience qualification
-> fresh randomized G_action identification
-> causal dataset acceptance
-> end-to-end latency + FFT/FRF characterization
-> AURA detector shadow bake-off
-> World Model v1 model ladder
-> history ablation
-> T_D -> T_A delay model
-> uncertainty calibration
-> WISE-0 candidate enumeration
-> event-triggered WISE
-> TinyMPC / Koopman / RTI only if justified
-> low-dimensional online adaptation
-> formal AEGIS safety/runtime assurance
```

The governing design principle remains:

```text
minimum complexity
that produces a demonstrable
closed-loop benefit
```

and:

```text
better causal data > larger model
```

## Authority boundary

This roadmap/index does not authorize runtime, scientific execution, Option-B freeze, treatment change, AURA/FAST/T1/C1 semantic change, safety change, SEALED access or production authority.