# AURA–WISE–WORLD MODEL–AEGIS vNext
## Future Implementation and Research Roadmap

**Canonical active roadmap version:** `v4 — CTEE formalization — 2026-08-31`  
**Scope:** Moving Mode only  
**Research registry:** `AURA_WISE_WM_AEGIS_SOURCE_REGISTRY_v8`

The exact full canonical roadmap artifact is maintained in the Detect and Response Project/File Library as:

```text
AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_20260831.md
```

GitHub retrieval/index view for the active revision:

[`AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_GITHUB_INDEX_20260831.md`](AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v4_CTEE_GITHUB_INDEX_20260831.md)

Historical v2 snapshot remains retained for provenance:

[`AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v2_20260831.md`](AURA_WISE_WM_AEGIS_FUTURE_IMPLEMENTATION_ROADMAP_v2_20260831.md)

This stable pointer path is retained so repository navigation does not change when a new roadmap revision is promoted.

## Current roadmap state

```text
STRUCTURAL_CLEANUP=CLOSED
PHASE_0A_MECHANISM=CLOSED
PHASE_0B1=CLOSED

PHASE_0B2=OWNER_MARGIN_DECISION_REQUIRED
COUNTERFACTUAL_IDENTIFIABILITY=REFERENCE_ONLY
POST_NATIVE_EVENT_CONTAMINATION_AUDIT=LIMITED

Q1_NOSCIENCE_NO_LAUNCH_CONTRACT=PREPARED
Q1_RUNTIME_AUTHORIZED=false
Q1_RUNTIME_EXECUTED=false

M_STABLE_US=UNFROZEN
W_MAX_US=UNFROZEN
OPTION_B_CONTRACT_READY=false
IMPLEMENTATION_AUTHORIZED=false

FRESH_SCIENCE=BLOCKED
SEALED=LOCKED_PRE_EVALUATION
production_authority=false
```

## Immediate dependency order

```text
0B.2c Q1 bounded no-launch shadow runtime
        OWNER AUTHORIZATION REQUIRED
-> offline full-predicate characterization
-> owner M_STABLE / W_MAX / scheduling / source-time freeze
-> static dependency graph + Tarjan SCC preflight
-> CTEE benchmark / eligibility qualification
   versus simpler timed FSM / timed guards
-> bounded delayed-launch nonscience qualification
-> scientific-pilot owner review
-> fresh randomized G_action science
-> causal dataset acceptance
```

CTEE is not active now. It is a post-freeze **candidate implementation** for temporal/causal eligibility:

```text
three-valued predicates
-> source-bound max-plus frontier
-> readiness potential / blocker
-> atomic recheck + commit / fail-closed
```

Its status remains:

```text
CTEE_NAME=PROJECT_PROPOSAL
CTEE_PUBLICATION_NOVELTY=NOT_PROVEN
CTEE_PRIOR_ART_COMPONENTS=STRONG
```

If CTEE does not outperform a simpler timed-state-machine/timed-guard implementation on correctness, tail ready→commit timing/jitter, or engineering/forensic complexity, use the simpler implementation and retire the proposal.

## Long-term direction

```text
Phase 0 causal closure
-> end-to-end latency + FFT/FRF characterization
-> AURA detector challengers
-> World Model v1 model ladder
-> history and T_D->T_A delay ablations
-> uncertainty-aware prediction
-> WISE candidate enumeration / event-triggered planning
-> TinyMPC / Koopman / RTI only if justified
-> low-dimensional online adaptation
-> formal AEGIS safety/runtime assurance
```

Algorithm roles remain separated:

```text
Tarjan SCC   = static structural validation
CTEE         = candidate live temporal/causal eligibility
Sweep Line   = offline scientific timeline validation
MCMF         = future conditional resource allocation
```

The roadmap does **not** itself authorize runtime, scientific execution, treatment changes, control changes, safety changes, SEALED access or production authority.